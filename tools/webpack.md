# webpack基本操作

1. ##### webpack介绍

   ```js
   webpack是一款模块加载器兼打包工具，它能把各种资源，例如JS（含JSX）、coffee、样式（含less/sass）、图片、html等都作为模块来使用和处理
   为什么使用webpack？现今的很多网页其实可以看做是功能丰富的应用，它们拥有着复杂的JavaScript代码和一大堆依赖包。为了简化开发的复杂度，引入了模块化的开发，例如requireJS（但只能加载js文件）
   安装webpack：
   //全局安装
   npm install -g webpack
   //安装到你的项目目录（首先通过cmd进入到项目目录，再执行以下操作）
   npm install --save-dev webpack
   
   创建package.json文件
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

3. 虚位以待！！！