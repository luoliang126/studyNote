# mpvue笔记

1. ##### 介绍

   ```js
   1、mpvue一套基于vue的小程序开发框架 参考：http://mpvue.com/
   2、mpvue一套可以以vue方式开发小程序，自动生成dist目录，只需要在小程序编辑器中引入dist目录预览即可。
   3、mpvue-weui 一套基于mpvue的小程序开发的UI组件库，基于weui
   4、 安装步骤：
   4.1、node，npm安装
   4.2、全局安装 vue-cli
   4.3、创建一个基于 mpvue-quickstart 模板的新项目
   vue init mpvue/mpvue-quickstart my-project（如果没有指定my-project目录，就是默认当前文件夹）
   4.4、安装依赖 npm install
   4.5、运行 npm run dev
   4.6、调试：npm run dev时会自动生成一个dist目录，用小程序IDE打开这个目录（appId如果是已经注册的小程序，就用小程序的appId,如果没有就用默认测试账号）。打开之后就可以通过小程序的IDE编辑，预览，上传等。
   注意：1、注册小程序的时候，每个邮箱只能绑定一个小程序。每个小程序可以邀请多个开发人员。
   	2、上传的小程序代码，要通过注册小程序的邮箱登录，然后发布。
   	3、一个公众平台可以同时关联10个小程序，且同一平台的小程序间可以相互调用，传参。
   5、mpvue使用方法和注意事项：
   5.1、小程序中的组件和生命周期函数，在mpvue中都可以继续使用。
   5.2、小程序中pages下的每个文件夹就是一个路由地址，当中的main.js可以理解为配置,index.vue则为html文件
   main.js配置
   export default {
       config: {
           navigationBarTitleText: '查看启动日志'
       }
   }
   5.3、路由的跳转
   方法一：使用类似于a标签的跳转
   <navigator url="../personal/main">
       跳转到personal页面
   </navigator>
   方法二：使用方法跳转
   wx.navigateTo({
       url: '../personal/main'
   })
   ```

2. ##### mpvue中使用vuex

   ```js
   1、在src目录下新建一个store的文件夹，并在该文件夹下新建一个store.js文件
   
   2、store.js，跟vue中的vuex一样
   import Vue from 'vue'
   import Vuex from 'vuex'
   Vue.use(Vuex)
   const store = new Vuex.Store({
       state: {
           count: 0,
           testName:'test123'
       },
       mutations: {
           increment: (state) => {
               const obj = state
               obj.count += 1
           },
           decrement: (state) => {
               const obj = state
               obj.count -= 1
           },
           changeTestName(state,params){
               console.log(params);
               state.testName = params
           }
       }
   })
   export default store
   
   3、在main.js中挂载全局
   import store from './store/store.js'
   Vue.prototype.$store = store;
   
   4、在页面组件中的使用
   console.log(this.$store.state.testName);  // 查看
   this.$store.commit("changeTestName",7070); // 修改
   
   ？？？moudles多个store大型项目，待验证！
   ```

3. ##### mpvue中使用日历表 <a href="https://www.imooc.com/article/39390" target="_blank">参考文档</a>

4. 虚位以待！