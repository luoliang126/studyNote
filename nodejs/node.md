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
   show 显示包的信息
   	npm show package
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
   	npm init
   conifg  查看npm的配置信息
   	npm config get registry // 获取npm拉取包的地址
       npm config set registry http://xxxxx // 设置npm的registry地址
   --force 强制安装（npm在安装包时，会在npm安装目录创建一个类似node_modules的镜像，如果安装的包以前安装过，那么就直接用本地镜像，而不是远端，使用force表示强行从远端获取，并更新本地镜像！）
   	npm install package --force
   
   2、nrm的使用，管理npm的不同版本
   ```
   
2. ##### node版本管理nvm，参考：https://blog.csdn.net/baidu_30907803/article/details/80734275

   ```js
   描述：每个项目都有固定的node版本，或者推荐>多少版本才能运行，假如有两个项目存在冲突，怎么办？使用nvm管理不同node版本
   
   1、下载安装nvm
   	nvm version  // 查看版本号，以及是否安装成功！
   
   2、进入终端操作
       安装指定node版本
       nvm install 6.0.0
   	nvm install 6.0.0 32 // windows32位node版本
   
   3、切换node版本
   nvm list // 查看当前安装的所有node版本
   nvm user 6.0.0 // 使用6.0.0版本
   node -v // 查看当前node版本
   ```

   

3. ##### npm发包

   ```js
   基于vue-cli3发布vue组件方法，参考：https://www.cnblogs.com/jasonwang2y60/p/11382349.html
   1、创建项目
   vue create programme
   
   2、开发前准备，修改目录
   把src目录改为examples作为查看组件的演示目录，新建packages目录作为组件编写的目录。在packages下新建index.js作为导出组件入口，作为整个组件库的导出，新建组件文件夹作为组件源码的放置
   
   3、修改入口文件，在项目根目录下新建vue.config.js并添加如下修改 src 目录 为 examples 目录
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
   
   4、编写组件 ，和平时写组件一样
   
   5、在packages/index,js 整合所有的组件，对外导出，即一个完整的组件库
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
   
   6、本地开发，预览
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
   
   7、设置发布忽略项
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
   
   8、package.js 中新增一条编译为库的命令
   vue-cli-service build --target lib --name myLib [entry]
   target : 构建目标，默认为应用模式。这里修改为 lib 启用库模式。
   lib : 输出目录，默认 dist 。这里我们改成 lib
   [entry] : 最后一个参数为入口文件，默认为 src/App.vue 。这里我们指定编译 packages/ 组件库目录。
   例如：
   "lib": "vue-cli-service build --target lib --name smart-alipay --dest lib src/main.js",
   
   9、修改package.js中的入口文件，以及包的信息
   "name": "vue-svgicon-coms",
   "version": "1.0.1",
   "author": "jason",
   "description": "vue-svg-components",
   "main": "lib/vue-svgicon-coms.umd.min.js", // 入口文件
   "keyword": "vue svg icon",
   "license": "MIT",
   "private": false,  // 注意如果private为true说明为私有包，不允许发布
   
   10、执行编译
   npm run lib
   
   11、发布（当然前提是必须有一个npm账号）
   npm publish
   
   问题描述：上面是基于vue-cli的npm包发布，他会将项目中的依赖一起打包到npm包里。但有时候我们只发布一些插件，例如js文件，无关vue组件，所以就没必要将vue等的依赖一起打包，这样会增加包的大小。
   参考：https://blog.csdn.net/weixin_44198965/article/details/104209771
   	https://blog.csdn.net/xyphf/article/details/84846009
   新建一个项目smart-plugins
   初始化 npm init
   此时会生成一个package.json
   {
     "name": "smart-plugins",
     "version": "1.0.0",
     "description": "公司一些公用的包",
     "main": "src/index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "luoliang",
     "license": "ISC"
   }
   新建src资源文件目录
   	新建index.js入口文件，引入需要暴露的js插件
   	import { Iframe,receiveMessage,sendMessageToParent } from './Iframe.js';
       export {
           Iframe,
           receiveMessage,
           sendMessageToParent
       }
   执行npm publish（注意：这里少了一个webpack打包的过程，可以在这里引入webpack或者grut等打包），发布出去的包可以用，但全部都是源码，没有编译，没有压缩。这里已webpack为例，解决打包压缩问题。
   安装webpack和webpack-cli
   npm install webpack webpack-cli
   配置package.json，添加打包压缩命令
   "scripts":{
       ......
       "build":"webpack"
   }
   这里默认webpack的打包，以及打包的输出文件（当然可以配置webpack，详情查看smart-core-util插件发布）。
   执行npm run build后会生成一个dist目录，以及一个main.js文件，此时我们需要修改包的入口为该main.js
   {
       ......
       "main":"dist/main.js"
   }
   再执行npm publish发布该包，此时的包就已经是打包压缩过后的了。
   ```

4. ##### npm私有仓库搭建

   ```js
   问题描述：内网开发。在一些公司内部，由于各种原因，会用到npm官网上发布的第三方包，但是开发时又不希望开发人员随意使用外界的包，必须通过管理人员通过后才可引入。但前端开发的包依赖管理node_modules又是必须的，所有搭建一个内网或者叫局域网的私有仓库就很有必要了。内网的npm仓库拉取速度更快。
   
   方法一：适用于私有仓库可以连接外网，每次需要引用外网包的时候打开，然后生成一个本地镜像，断开网络，下次再有拉取的时候，就可以直接取本地镜像文件。当然也可以将已有的镜像文件上传到私有仓库（可以使用拷贝的方式，当然也可以使用脚本的方式上传）
   使用verdaccio插件
   安装(必须全局)
   	npm install --global verdaccio
   	
   运行，即启动一个npm仓库服务（可以查看到仓库的物理地址）
   	verdaccio
   	
   查找到config.yaml配置文件（大部分不用修改）
   	#只添加一句，监听端口，使其他局域网用户可以通过ip访问本机的npm私有仓库，注意关闭防火墙！！！
   	listen:0.0.0.0:4873
   	
   本地的私有仓库启动好后，那么安装npm包的地址，就应该指向该私有仓库地址
   npm config set registry http://localhost:4873
   npm config get registry // 查看npm的镜像源
   
   注册私有仓库用户
   npm adduser -registry http://localhost:4873
   ......然后输入用户名密码邮箱等操作
   
   完成后，查看当前用户
   npm who am i
   
   当然发布自己开发的私有包，也是npm publish，发布完成后，安装npm install 包名
   yarn命令直接操作，不需要做其他配置！！！
   
   方法二：适用于私有仓库本身就无法连接外网。那么就必须将已有的npm包，通过拷贝或者脚本上传的方式存储于私有仓库内。
   step1：
   将项目需要的npm包全部在package.json中标注
   step2：
   安装node-tgz-downloader:(最好全局)
   npm install node-tgz-downloader -g
   step3：
   执行npm install 并生成package-lock.json (用于锁定依赖包的版本)
   注意：如果没有生成package-lock.json，可以刷新一下目录。如果还是不行，查看一下 npm config get package-lock，以及设置 npm config set package-lock true。假如还是不行，删除node_modules再重新npm install一次
   step4：
   执行download-tgz package-lock package-lock.json 会将所有package-lock.json中标注的依赖包（以及对应的版本号）拉取到本地，并在package-lock.json所在目录生成一个tarballs目录，里面就是我们项目所需要的所有安装包（注意：是没有解压的.tgz压缩文件）
   step5:
   上传tarballs目录中的所有.tgz文件到，私有仓库或本地服务
   单独上传：可以直接使用 npm publish xxx.tgz
   批量上传：执行脚本，一般后端人员提供！
   ```

   

5. ##### yarn操作

   ```
   安装yarn（全局）
   npm install -g yarn
   
   查看版本
   yarn --version
   
   初始化项目（同npm）
   yarn init
   
   yarn的配置项
   yarn config list // 显示所有配置项
   yarn config get key //获取具体key配置项
   delete key // 删除某个key配置项
   set	key value // 设置某个key配置项为value
   
   添加包
   yarn add package // 安装一个package包，并添加到package.json和yarn.lock文件中
   yarn add package@version // 安装指定版本
   yarn add package@tag // 安装某个包下的tag
   
   yarn安装某个依赖包
   yarn install // 安装package.json中的所有包，并将包及所有依赖保存进yarn.lock
   yarn install --force // 强制重新下载所有包
   yarn install --production // 值安装dependencies里的包
   yarn install --no-lockfile // 不读取或者生产yarn.lock
   
   yarn发布包
   yarn publish
   
   yarn更新包
   yarn upgrade package // 包更新了，但是package.json中包的版本标识未更新！
   //将package包更新到1.0.0版本，并将package.json一起更新。在使用时最好先查看一下最新版本号yarn info package
   yarn upgrade  package@1.0.0
   yarn add package // 重新安装package，也会更新package.json
   
   显示一个包的信息
   yarn info package
   
   yarn移除包
   yarn remove package // 移除一个package包，并更新package.json和yarn.lock
   
   ```

   

6. 虚位以待！！！