# webpack基本操作

1. ##### webpack介绍

   ```js
   webpack是一款模块加载器兼打包工具，它能把各种资源，例如JS（含JSX）、coffee、样式（含less/sass）、图片、html等都作为模块来使用和处理
   为什么使用webpack？现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，引入了模块化的开发，例如requireJS
   安装webpack：
   //全局安装
   npm install -g webpack
   //安装到你的项目目录（首先通过cmd进入到项目目录，再执行以下操作）
   npm install --save-dev webpack
   
   // 创建package.json文件
   1、运行：npm init -y 快速创建项目，自动生成package.json
   
   2、创建项目目录
   package.json目录:配置文件
   dist目录：用于存放打包后的项目
   src目录：存放项目文件
   	index.html
   	index.js
   		console.log('ok')
   
   3、安装webpack(如果慢的话，使用cnpm)
   	npm install webpack -D
   	npm install webpack-cli -D
   
   4、创建一个webpack.config.js，配置webpack打包时的信息。（注意：webpack是基于node构建，所以webpack支持所有的Node API语法）
   commonJS规范：暴露一个模块的方法和es6注意区别
   module exports = {
       mode:'development' // development为开发模式，只打包不压缩，production打包+压缩。
   }
   注意：webpack4.x版本，提供了约定文件 打包的入口文件是，src-->index.js，打包的输出文件放在dist目录下的main.js
   
   5、在index.html中引入打包好的main.js
   <script src="../dist/main.js"></script>
   访问index.html，在控制台输出ok，成功！！！
   
   6、我们在index.html中引入的js为打包后的main.js，但是如果我们重新编辑修改代码后，ctrl+s保存，发现并未重新打包，那刷新浏览器内容还是不会变更？怎么解决？
   使用webpack-dev-server：提供一个webpack的服务，让我们可以通过ip访问这个服务
   安装：npm install webpack-dev-serve
   重新配置package.json
   	“scripts”：{
           “dev”:"webpack-dev-server --open"  // --open编译好后，自动打开浏览器
       }
   	--port 3000 // 默认端口号是：8080，通过port可以自行修改
   	--host 127.0.0.1 // 通过ip访问
   	......
   修改index.html中的main.js。通过webpack-dev-server生成的main.js存放于内存中，可以理解为在根目录下，但是在实际目录中看不到！！！，为什么要存放内存中？因为：只有内存读取/修改更快，且不伤害物理内存（即磁盘）。试想一下，如果我们在开发时，修改一个变量就保存一次，就编译一次，就热更新一次，就重新修改一次磁盘，那过不了多久磁盘估计就报废了！所以存放内存，是最好的办法！
   <script src="/main.js"></script>
   这次就可以通过webpack服务访问：localhost:8080
   编辑内容、保存，就会发现热更新可以使用！！！
   
   7、webpack插件
   html-webpack-plugin：把界面生成到内存中去
   安装：npm install html-webpack-plugin -D
   在webpack.config.js中
   const path = require('path');
   const HtmlWebPackPlugin = require('html-webpack-plugin')
   //创建一个插件实例对象
   const htmlPlugin = new HtmlWebPackPlugin{
       template:path.join(__dirname,'./src/index.html'), // 源文件，注意这里是根路径 
       filename:'index.html' // 生成的，在内存中的首页
   }
   ```

2. ##### 基于vue-cli的webpack

   ```js
   注意事项：
   1、webpack打包时，如果图片资源过大就会重新创建一个文件夹，所以尽量用小的图片，如10kb以下的，路径最好用绝对路径如：'../../static/loading.png'
   2、package.json中的 dependencies（用于正式打包所用的依赖，越少越好）和devDependencies（dev环境中需要，我们在跑dev的时候尽量放在这个当中）
   3、如果在打包中无法识别es6语法，引入es2015(npm install babel-preset-es2015)和stage（npm install babel-preset-stage-0）,并在.babelrc中引用，如：
   {
       "presets": [ "es2015", "stage-0" ],
       "plugins": ["transform-vue-jsx", "transform-runtime"]
   }
   4、压缩文件的配置
   plugins:[
       new UglifyJsPlugin({   // 压缩使用
           uglifyOptions: {
               compress: {
                   warnings: false
               }
           },
           sourceMap: config.build.productionSourceMap,
           parallel: true
       }),
       ......
   ]
   5、webpack打包后的css动画animate无法执行问题，修改打包时，将css和js打包到一起。这样在dist目录下就不会生成.min.css，而样式文件全部都在.min.js文件中。修改build文件夹下的vue-loader.conf.js中的loaders项
   loaders: utils.cssLoaders({
       sourceMap: sourceMapEnabled,
       extract: false  // isProduction (默认值)
   }),
       
   6、webpack打包时，css样式中z-index被修改，重新计算。 参考：https://blog.csdn.net/Q_Jack521/article/details/78422864
   打包css解决办法：
   var OptimizeCSSPlugin = require('optimize-css-assets-webpack-plugin')
   ......
   new OptimizeCSSPlugin({
       cssProcessorOptions: {
           safe: true
       }
   })
   
   7、打包方法  参考：https://jingsam.github.io/2016/11/18/bundle-vue-components.html
   				https://webpack.docschina.org/configuration/externals/
   第一：开源项目，那么我们自然希望别人用我们的包的时候，只需要npm install 一下就行了，我们包里面依赖的文件件全部都打到包里。
   第二：就是我们自己做的包，自己引用，那么需要依赖什么东西，我们可以通过cdn等外部方式连接进来，就不需要将所有的东西打到一个包里，增加包的大小，（app端尤其重要）。
   打包时将mintui部分引用组件和其他组件包，一起打包（默认）
   那么打包时怎样将vue，mintui等依赖文件不打包，而只是引用。在我们的引用项目中再引入vue，mintui等？
   在webpack.base.conf.js中的entry下面(在module.exports中添加暴露的模块)，紧接着添加
   externals: {
       vue: {
           root: 'Vue',
   		commonjs: 'vue',
   		commonjs2: 'vue',
   		amd: 'vue'
       }
   },
   或者
   externals: {
       jquery: 'jQuery'
       Vue:'vue'，
   	......
   },
       
   8、npm run build打包项目中js文件过大，解决办法： 参考：http://www.cnblogs.com/wjunwei/p/9242142.html
   8.1、文件通过cdn在index.html中引入（如vue，vue-router，vuex等）
   <link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
   <script src="https://spkg.smarteventcloud.cn/vue/2.5.16/vue.min.js"></script>
   <script src="https://spkg.smarteventcloud.cn/vuex/3.0.1/vuex.min.js"></script>
   <script src="https://spkg.smarteventcloud.cn/vue-router/3.0.1/vue-router.min.js"></script>
   ......
   并在module.exports中添加
   externals: {
       'vue': 'Vue',
   	'vue-router': 'VueRouter',
   	'element-ui': 'ELEMENT',
   	......
   },
   那么对应的main.js中的
   import Vue from 'vue'
   import ElementUI from 'element-ui'
   Vue.use(ElementUI)
   import，use等，就可以删除了(如果不删除的话，依然会打包，这样项目文件还是那么大)
   注意：如果vue-router通过了cdn引入，那么在new Router(......)的时候，就应该使用webpack.base.conf.js中映射的VueRouter
   new VueRoute(......)
       
   vue-cli4的配置
   module.exports = {
       ......
       configureWebpack:{  // 不需要打包的文件
           externals:{
               vue:'Vue'.
               'vue-router':'VueRouter',
               axios:'axios'
           },
           css:[],
           js:[   // js扩展插件的引入方式
               '//cdn.xxx.xxx/vue.min.js',
               '//cdn.xxx.xxx/vue-router.min.js'
               '//cdn.xxx.xxx/axios.min.js'
           ]
       }
   }
   当然，还可以直接在index.html中直接引入cdn地址
       
   8.2、路由懒加载
   import Home from './views/Home.vue'
   const routes = [
       {
           path:'/',
           component:Home,
           meta:{title:'首页'}
       }
       ......
   ]
   修改为：
   const routes = [
       {
           path:'/',
           meta:{title:'首页'},
           component: (resolve) => require(['./views/Home.vue'],resolve)
       }
       ......
   ]
   npm run build的时候就会发现多了很多1.xxxx.js,2.xxxx.js等多个js文件。但app.js变小了
   8.3、map文件一般比较大，打包的时候会影响打包速度（建议禁止打包map文件）取消map打包方法：
   productionSourceMap: false,
   
   9、vue-cli3配置多个运行环境  参考：https://www.cnblogs.com/webtaotao/p/13540956.html
   9.1、首先在项目目录下创建不同的.env文件(目前我们讨论 开发环境 dev,测试环境test,发布环境 prod) 所以我们创建如下三个文件(.env.prod、.env.test、.env.development)，如果需要其他环境 按照以上描述，继续创建.env.XXX环境名称
   9.2、配置三个文件（以devement为例）
   ENV = 'development'
   port = 9549
   VUE_APP_API_URL = 'http://61.240.148.87:8309'
   // 当然，如果还有更多的环境变量，接口地址等，可以继续配置
   9.3、完成以上所需环境配置之后改写 package.json中的运行命令
   "dev": "vue-cli-service serve --mode development",
   "ch": "vue-cli-service serve --mode ch",
   "build": "vue-cli-service build"
   ```

3. ##### webpack打包、压缩工具类插件，并发布到npm

   ```js
   描述：有时候我们需要一些公用的插件，样式，方法等，因为在不同项目都需要。那么抽离发布npm包管理就是一种很好的解决办法（总比粘贴复制强）
   参考文档：
   https://blog.csdn.net/weixin_36185028/article/details/81117730
   https://www.cnblogs.com/hezihao/p/7930155.html
   https://blog.csdn.net/u012443286/article/details/79577545
   https://segmentfault.com/q/1010000012933464
   实现步骤：
   1、创建项目
   npm init 初始化项目，生成package.json
   2、新建目录，即我们需要暴露的js文件（当然公用css文件也行），比如根目录下的js下的validator.js。
   const fun1 = () => {
       ......
   }
   const obj = {
       name:'luoliang',
       age:18
   }
   export {
   	fun1,
       obj
   }
   3、安装webpack(用于打包，压缩文件)，直接发布原js文件其实也可以了，但是如果js过大的话，还是使用webpack打包压缩一下更好！
       npm install webpack webpack-cli uglifyjs-webpack-plugin
   4、配置webpack打包项
   根目录下新建一个webpack.config.js
   const path = require("path");
   const uglify = require('uglifyjs-webpack-plugin');
   module.exports = {
       mode:'development',
       // 入口配置的对象中，属性为输出的js文件名，属性值为入口文件
       entry: {
           plugins:".src/index.js"
       },
       //输出路径和文件名，使用path模块resolve方法将输出路径解析为绝对路径
       output: {
           libraryTarget: 'umd', // 这个很重要！！！定义模块的运行方式(解决了无法export和import的问题)
           path: path.resolve(__dirname, "./dist/js"), //将js文件打包到dist/js的目录
           filename: "[name].js" //使用[name]打包出来的js文件会分别按照入口文件配置的属性来命名
       },
       // 压缩
       plugins:[
           new uglify()
       ],
       module:{
   		rules:[
   			{
   				test: /\.js$/,  // 处理js文件
                   exclude: node_modules/, 
                   loader: "babel-loader"
   			}
   		]
   	}
   };
   5、安装babel,在打包的时候，将es6+转换成es5语法.
   npm install @babel/core @babel/preset-env babel-loader  // babel-core babel-preset-env前面两个不行的话，再装这两个
   配置babel，在根目录下，新建一个.babelrc文件
   {
       "presets": ["@babel/preset-env"]
   }
   6、配置package.json
   {
     "name": "gtwx_plugins",
     "version": "1.0.6",
     "description": "公用插件、样式",
     "main": "dist/js/plugins.js",  // 发布npm包时，指定入口
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "npx webpack --mode development" // 新建打包命令
     },
     "author": "luoliang",
     "license": "ISC",
     "dependencies": {
       "@babel/core": "^7.12.10",
       "@babel/preset-env": "^7.12.11",
       "babel-core": "^6.26.3",
       "babel-loader": "^8.2.2",
       "babel-preset-env": "^1.7.0",
       "webpack": "^5.16.0"
     },
     "devDependencies": {
       "webpack-cli": "^4.4.0"
     },
     "files": [  //发布npm包时，指定文件，白名单
       "dist",
       "css",
       "js"
     ]
   }
   7、执行步骤
   npm run build //生成dist/js/plugins.js文件
   npm publish // 发布
   
   ```

   

4. 虚位以待！！！