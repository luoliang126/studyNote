# koa2笔记

1. ##### 使用koa-cli快速搭建项目

   ```js
   使用koa-generator快速搭建koa项目(类似koa-cli) 
   参考文档：https://blog.csdn.net/ansu2009/article/details/53884287
   1、安装koa-generator
   npm install -g koa-generator（建议全局安装）
   2、生成koa项目
   koa2 test_koa //（test_koa是你需要建立的项目名称）
   3、cd 到 test_koa目录下，安装依赖
   npm install
   4、执行package.json中的指令
   npm run start
   默认端口号：localhost:3000 
   ```

2. ##### 使用原始方法搭建

   ```js
   1、创建项目文件夹，并在该目录下创建app.js
   // 导入koa，和koa 1.x不同，在koa2中，我们导入的是一个class，因此用大写的Koa表示:
   const Koa = require('koa');
   // 创建一个Koa对象表示web app本身:
   const app = new Koa();
   // 对于任何请求，app将调用该异步函数处理请求：
   app.use(async (ctx, next) => {
       await next();
       ctx.response.type = 'text/html';
       ctx.response.body = '<h1>Hello, koa2!</h1>';
   });
   // 在端口3000监听:
   app.listen(3000);
   console.log('app started at port 3000...');
   
   2、创建包管理工具package.json
   {
       "name": "hello-koa2",
   	"version": "1.0.0",
   	"description": "Hello Koa 2 example with async",
   	"main": "app.js",
   	"scripts": {
   		"start": "node app.js"
   	},
   	"keywords": [
           "koa",
           "async"
   	],
   	"author": "Michael Liao",
   	"license": "Apache-2.0",
   	"repository": {
           "type": "git",
   		"url": "https://github.com/michaelliao/learn-javascript.git"
   	},
   	"dependencies": {
           "koa": "2.0.0"
       }
   }
   3、执行npm install 安装package.json中的依赖项目，会自动生成node_modules以及安装的依赖
   
   4、接下来运行在package.json中的配置项 npm start
   
   5、地址栏中输入localhost:3000 。经典的hello koa就看到了
   ```

3. ##### koa的中间件middleware

   ```
   app.use(async (ctx, next) => {
       await next();
       ctx.response.type = 'text/html';
       ctx.response.body = '<h1>Hello, koa2!</h1>';
   });
   app.use(async (ctx, next) => {
       if (ctx.request.path === '/') {
       	ctx.response.body = 'index page';
       } else {
       	await next();
       }
   });
   app.use(async (ctx, next) => {
       if (ctx.request.path === '/test') {
       	ctx.response.body = 'TEST page';
       } else {
       	await next();
       }
   });
   每收到一个http请求，koa就会调用通过app.use()注册的async函数，并传入ctx和next参数。
   我们可以对ctx操作，并设置返回内容。但是await next()是什么？
   原因是koa把很多async函数组成一个处理链，每个async函数都可以做一些自己的事情，然后用await next()来调用下一个async函数。
   我们把每个async函数称为middleware，这些middleware可以组合起来，完成很多有用的功能。
   返回403
   app.use(async (ctx, next) => {
       if (await checkUserPermission(ctx)) {
       	await next();
       } else {
       	ctx.response.status = 403;
       }
   });
   在上面我们发现了当请求地址path 不同时，执行不同的代码，但这样肯定很不方便，所有有了路由
   ```

4. ##### koa路由模块

   ```js
   1、安装koa-router
   npm install koa-router
   
   2、在package.json的dependencies中发现已经添加了koa-router模块，接下来修改app.js
   const Koa = require( Koa =  'koa');
   const app = new Koa();
   // 注意require('koa-router')返回的是函数:
   const router = require('koa-router')();
   // 添加 url-route:
   router.get('/', async (ctx, next) => {
       ctx.response.body = '<h1>Index</h1>';
   });
   router.get('/hello/:name', async (ctx, next) => {
       var name = ctx.params.name;
       ctx.response.body = `<h1>Hello, ${name}!</h1>`;
   });
   // 添加 router middleware:
   app.use(router.routes());
   app.listen(3000);
   console.log('app started at port 3000...');
   
   3、上面都是get请求，post请求呢？？？
   直接router.post('/path', async fn) 不就行了？但用post请求处理URL时，我们会遇到一个问题：post请求通常会发送一个表单，或者JSON，它作为request的body发送，但无论是Node.js提供的原始request对象，还是koa提供的request对象，都不提供解析request的body的功能！
   所以，我们又需要引入另一个middleware来解析原始request请求，然后，把解析后的参数，绑定到ctx.request.body中。
   使用koa-bodyparser中间件应运而生
   npm install koa-bodyparser
   // 引入解析post请求的中间件
   const bodyParser = require('koa-bodyparser');
   app.use(bodyParser());
   注意：koa-bodyparser必须在router之前被注册到app对象上。
   
   4、在app.js中少量请求地址还好，假如有很多个接口时，app.js就会很臃肿，每次修改接口都会修改到app.js文件，这当然不是我们想要的。模块划分。
   创建一个controllers文件夹，并在该文件夹下，创建路由模块（根据功能划分）
   eg:index.js（可以多个路由文件，通过controller.js统一暴露）
   var fn_index = async (ctx, next) => {
       ctx.response.body = `<h1>Index</h1>
       <form action="/signin" method="post">
       <p>Name: <input name="name" value="koa"></p>
       <p>Password: <input name="password" type="password"></p>
       <p><input type="submit" value="Submit"></p>
       </form>`;
   };
   var fn_signin = async (ctx, next) => {
       var name = ctx.request.body.name || '',
           password = ctx.request.body.password || '';
       console.log(`signin with name: ${name}, password: ${password}`);
       if (name === 'koa' && password === '12345') {
           ctx.response.body = `<h1>Welcome, ${name}!</h1>`;
       } else {
           ctx.response.body = `<h1>Login failed!</h1><p><a href="/">Try again</a></p>`;
       }
   };
   ......
   module.exports = {
       'GET /': fn_index,
       'POST /signin': fn_signin
       ......
   };
   
   5、路由按功能划分好后，怎样在app.js中导入controllers中的路由文件呢？导入后怎样使用接口呢？？？
   使用中间件controller.js。在controllers文件夹下新建一个controller.js，用于将上面定义的所有路由中间件暴露出去
   const fs = require('fs');
   function addMapping(router, mapping) {
       for (var url in mapping) {
           if (url.startsWith('GET ')) {
               var path = url.substring(4);
               router.get(path, mapping[url]);
           } else if (url.startsWith('POST ')) {
               var path = url.substring(5);
               router.post(path, mapping[url]);
           } else {
               console.log(`invalid URL: ${url}`);
           }
       }
   }
   function addControllers(router) {
       var files = fs.readdirSync(__dirname);
       var js_files = files.filter((f) => {
           return f.endsWith('.js');
       });
       console.log(js_files);
       for (var f of js_files) {
           let mapping = require(__dirname + '/' + f);
           addMapping(router, mapping);
       }
   }
   module.exports = function (dir) {
       let controllers_dir = dir || 'controllers', // 如果不传参数，扫描目录默认为'controllers'
           router = require('koa-router')();
       addControllers(router, controllers_dir);
       return router.routes();
   };
   ```

5. ##### node操作mongo数据库

   ```js
   1、数据模型
   2、当设计多个跨表操作时，可以引入多个model操作。
   
   3、动态创建model
   ```

6. ##### 跨域问题

   ```js
   1、引入koa2-cors
   npm install koa2-cors;
   2、使用
   const cors = require('koa2-cors');
   const app = new Koa();
   app.use(cors());
   
   注意：如以上还是不行，自己手动配置一个localCore.js
   /**
    * 关键点：
    * 1、如果需要支持 cookies,
    *    Access-Control-Allow-Origin 不能设置为 *,
    *    并且 Access-Control-Allow-Credentials 需要设置为 true
    *    (注意前端请求需要设置 withCredentials = true)
    * 2、当 method = OPTIONS 时, 属于预检(复杂请求), 当为预检时, 可以直接返回空响应体, 对应的 http 状态码为 204
    * 3、通过 Access-Control-Max-Age 可以设置预检结果的缓存, 单位(秒)
    * 4、通过 Access-Control-Allow-Headers 设置需要支持的跨域请求头
    * 5、通过 Access-Control-Allow-Methods 设置需要支持的跨域请求方法
    */
   module.exports = async (ctx, next) => {
       ctx.set('Access-Control-Allow-Origin', '*'); //允许来自所有域名请求(不携带cookie请求可以用*，如果有携带cookie请求必须指定域名)
       // ctx.set("Access-Control-Allow-Origin", "http://localhost:8080"); // 只允许指定域名http://localhost:8080的请求
   
       ctx.set('Access-Control-Allow-Methods', 'OPTIONS, GET, PUT, POST, DELETE'); // 设置所允许的HTTP请求方法
   
       ctx.set('Access-Control-Allow-Headers', 'x-requested-with, accept, origin, content-type'); // 字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段.
       // 服务器收到请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应。
   
       ctx.set('Content-Type', 'application/json;charset=utf-8'); // Content-Type表示具体请求中的媒体类型信息
   
       ctx.set('Access-Control-Allow-Credentials', true); // 该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。默认情况下，Cookie不包括在CORS请求之中。
       // 当设置成允许请求携带cookie时，需要保证"Access-Control-Allow-Origin"是服务器有的域名，而不能是"*";
   
       ctx.set('Access-Control-Max-Age', 300); // 该字段可选，用来指定本次预检请求的有效期，单位为秒。
       // 当请求方法是PUT或DELETE等特殊方法或者Content-Type字段的类型是application/json时，服务器会提前发送一次请求进行验证
       // 下面的的设置只本次验证的有效时间，即在该时间段内服务端可以不用进行验证
   
       ctx.set('Access-Control-Expose-Headers', 'myData'); // 需要获取其他字段时，使用Access-Control-Expose-Headers，
       // getResponseHeader('myData')可以返回我们所需的值
       /*
       CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：
           Cache-Control、
           Content-Language、
           Content-Type、
           Expires、
           Last-Modified、
           Pragma。
       */
   
       /* 解决OPTIONS请求 */
       if (ctx.method == 'OPTIONS') {
           ctx.body = '';
           ctx.status = 204;
       } else {
           await next();
       }
   };
   
   使用：const officicalRouter = require('./routes/officical.router');
   	app.use(localCore);
   ```

7. 虚位以待！