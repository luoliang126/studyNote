# nuxt笔记

1. ##### nuxt介绍

   ```
   nuxt.js(基于vue的服务端渲染，SSR) 参考：https://zh.nuxtjs.org
   ```

2. ##### nuxt安装

   ```js
   1、基于vue-cli创建nuxt项目 （如果是根目录则不需要 project-name ）
   vue init nuxt-community/starter-template <project-name>
   
   2、安装依赖
   cd <project-name>
   npm install
   
   3、在package.json配置文件中
   npm run dev  本地跑dev环境
   npm run build 创建dist目录用于发布（在.nuxt文件夹下会创建一个dist目录，用于发布）
   npm run start 线上跑prd环境
   ```

3. ##### nuxt的header部分

   ```
   nuxt没有index.html,它的app.html是这种格式，那么我们怎样修改title以及引入css样式呢？
   <!DOCTYPE html>
   <html {{ HTML_ATTRS }}>
       <head>
   	    {{ HEAD }}
       </head>
       <body {{ BODY_ATTRS }}>
       	{{ APP }}
       </body>
   </html>
   
   在vue组件中，只需要在head中修改link为css，script为js
   export default{ // 对象返回方式
       head: {
           link:[{
           	rel: 'stylesheet',href: 'http://api.map.baidu.com/library/DrawingManager/1.4/src/DrawingManager_min.css'
           }],
           script: [{
   	        src: 'http://api.map.baidu.com/api?v=2.0&ak=dTY9oTCoGLowbHuaUGMGohP4SnRGEE24'
           },{
       	    src:'http://api.map.baidu.com/library/DrawingManager/1.4/src/DrawingManager_min.js'
           }]
       }
   }
   或者
   export default{  // 函数返回方式
       head(){
           return{
               title: this.title,
               meta: [
               	{ hid: 'description', name: 'description', content: 'My custom description' }
               ],
               script: [{
   	            src: 'http://api.map.baidu.com/api?v=2.0&ak=dTY9oTCoGLowbHuaUGMGohP4SnRGEE24'
               },{
       	        src:'http://api.map.baidu.com/library/DrawingManager/1.4/src/DrawingManager_min.js'
               }],
           },
       }
   }
   ```

4. ##### nuxt.js路由

   ```js
   1、nuxt的路由是根据项目结构自动生成的，page为项目总文件，里面建立.vue的组件或者文件夹（根据文件夹再建立子路由和嵌套路由），来生成路由
   注意：asyncData(){......}该方法只能用于路由组件，才会在服务器端获取数据，嵌入的子组件是没有asyncData这个方法的.
   fetch、asyncData、validate也是只能在路由组件中使用
   
   2、动态路由（在路由后面 + 传递参数）
   建立文件的时候已" _ "开头，eg：
   pages
       serch
   		_index.vue
   最里层的index.vue路由文件，以" _ "开头，内容跟普通文件一样，里面有asyncData()方法，用于服务端渲染。
   ```

5. 基本使用 

   ```js
   1、asyncData用于通过http获取数据(服务端获取)
   export default {
       data(){
           return{
   
           }
       },
       async asyncData(context){
           let id = context.params.configServiceDetail; //获取路由传递参数id
           let detailData = await configService.getDetail('GetMaterial',id);
           return{
               detailData:detailData
           }
       }
   }
   注意：定义在asyncData中的return 跟 data中的return类似。所以界面上直接使用detailData
   如果是定义了nuxt/axios那么也可以这样使用,app是context的一个父级
   async asyncData({app}){
       let id = app.context.params.configServiceDetail; //获取路由传递参数id
       let detailData = await configService.getDetail('GetMaterial',id);
       return{
   	    detailData:detailData
       }
   }
   
   2、layouts文件夹的使用，整体布局（公共组件，报错界面等）
   
   3、validate方法，用于路由参数校验
   <script>
   export default {
       validate({ params }){  //注意是一个对象params
           let paramsReg = /^,|\d$/
           return paramsReg
       }
   }
   </script>
   如果validate返回的是false，那么重定向到layouts中的error.vue界面
   ```

6. 虚位以待

