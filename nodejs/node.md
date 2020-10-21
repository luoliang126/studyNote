# nodejs笔记

1. ##### npm

   ```js
   1、npm的基本操作，参考：https://blog.csdn.net/lu_embedded/article/details/79153826
   npm更新自己
   	npm install -g npm   // 将npm更新到最新版本
   search	在存储库中查找模块包
   	npm search change_password  // change_password为包名
   install	使用在存储库或本地位置上的一个package.json文件来安装包	npm install
       npm install express
       npm install express@0.1.1 //安装指定的express的 0.1.1的这个包
       npm install express@latest //安装express的最新包（一般情况不跟latest就是最新的包）
       npm install ../tModule.tgz  //安装压缩包
   	install -g	在全局可访问的位置安装一个包	npm install express -g
   uninstall	卸载一个模块
       npm uninstall express
   remove	删除一个模块
   	npm remove express
   update  更新一个模块到最新版本
   	npm update smart-filer  // 更新smart-filer到最新的版本
   pack	把在一个package.json文件中定义的模块封装成.tgz文件
   	npm pack
   view	显示模块的详细信息
   	npm view express
   publish	把在一个package.json文件中定义的模块发布到注册表，当前与package.json在同一文件夹下的包发布。(在发布包到npm官网上时，常用)
   	npm publish
   unpublish	取消发布您已发布到注册表的一个模块（在某些情况下，还需使用 --force 选项）
   	npm unpublish myModule
   owner 允许您在存储库中添加、删除包和列出包的所有者
   	npm add <username> myModule
   	npm rm <username> myModule
   	npm ls myModule
   whoami	（根据指定注册表模块）打印用户名
   	npm whoami
   adduser	将用户信息添加到当前的开发环境
   	npm adduser
   login	等同于adduser
   	npm login
   logout	将用户信息从当前的开发环境中清除
   	npm logout
   init	初始化Node包的信息，会创建package.json文件
   
   2、nrm的使用，管理npm的不同版本
   
   3、npm发包（基于vue-cli3），参考：https://www.cnblogs.com/jasonwang2y60/p/11382349.html
   3.1、创建项目
   vue create programme
   3.2、开发前准备，修改目录
   把src目录改为examples作为查看组件的演示目录，新建packages目录作为组件编写的目录。在packages下新建index.js作为导出组件入口，作为整个组件库的导出，新建组件文件夹作为组件源码的放置
   3.3、修改入口文件，在项目根目录下新建vue.config.js并添加如下修改 src 目录 为 examples 目录
   module.exports = {
       // 修改 src 目录 为 examples 目录
       pages: {
           index: {
               entry: 'examples/main.js',
               template: 'public/index.html',
               filename: 'index.html'
           }
       }
   }
   3.4、编写组件 ，和平时写组件一样
   3.5、在packages/index,js 整合所有的组件，对外导出，即一个完整的组件库
   // 导入颜色选择器组件
   import SvgIcon from './svg-icon'
   // 存储组件列表
   const components = [
     SvgIcon
   ]
   // 定义 install 方法，接收 Vue 作为参数。如果使用 use 注册插件，则所有的组件都将被注册
   const install = function (Vue) {
     // 判断是否安装
     if (install.installed) return
     // 遍历注册全局组件
     components.map(component => Vue.component(component.name, component))
   }
   // 判断是否是直接引入文件
   if (typeof window !== 'undefined' && window.Vue) {
     install(window.Vue)
   }
   export default {
     // 导出的对象必须具有 install，才能被 Vue.use() 方法安装
     install,
     // 以下是具体的组件列表
     SvgIcon
   }
   3.6、本地开发，预览
   在 examples/main.js中引入写好的组件
   import Vue from 'vue'
   import App from './App.vue'
   // 导入组件库
   import SvgIcon from './../packages/index'
   // 注册组件库
   Vue.use(SvgIcon)
   Vue.config.productionTip = false
   new Vue({
     render: h => h(App),
   }).$mount('#app')
   这样在examples中，就可以直接使用SvgIcon组件了
   <svg-icon icon-class="Abook"></svg-icon>
   3.7、设置发布忽略项
   在根目录添加 .npmignore 文件，设置忽略发布文件。
   .DS_Store
   node_modules/
   examples/
   packages/
   public/
   vue.config.js
   babel.config.js
   *.map
   *.html
   # local env files
   .env.local
   .env.*.local
   # Log files
   npm-debug.log*
   yarn-debug.log*
   yarn-error.log*
   # Editor directories and files
   .idea
   .vscode
   *.suo
   *.ntvs*
   *.njsproj
   *.sln
   *.sw*
   3.8、package.js 中新增一条编译为库的命令
   vue-cli-service build --target lib --name myLib [entry]
   target : 构建目标，默认为应用模式。这里修改为 lib 启用库模式。
   lib : 输出目录，默认 dist 。这里我们改成 lib
   [entry] : 最后一个参数为入口文件，默认为 src/App.vue 。这里我们指定编译 packages/ 组件库目录。
   例如：
   "lib": "vue-cli-service build --target lib --name smart-alipay --dest dist src/main.js",
   3.9、修改package.js中的入口文件，以及包的信息
   "name": "vue-svgicon-coms",
   "version": "1.0.1",
   "author": "jason",
   "description": "vue-svg-components",
   "main": "lib/vue-svgicon-coms.umd.min.js", // 入口文件
   "keyword": "vue svg icon",
   "license": "MIT",
   "private": false
   3.10、执行编译
   npm run lib
   3.11、发布（当然前提是必须有一个npm账号）
   npm publish
   ```

2. ##### node版本管理nvm，参考：https://blog.csdn.net/baidu_30907803/article/details/80734275

3. 虚位以待！！！