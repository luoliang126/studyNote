# uni-app笔记

1. ##### 安装

   ```js
   1、编辑器HBuilderX（App开发版）
   下载链接https://www.dcloud.io/hbuilderx.html
   2、解压即可得到安装文件及目录，点击HBuilderX.exe即可运行
   3、新建一个项目
   选择uni-app项目，输入项目名称，选择模板内置uni-ui模板（这是官方退出的一套UI组件，推荐使用，如果自己写UI组件的话，将会是一件很痛苦的事），最后点击创建！
   4、运行--在浏览器端，微信端，模拟器端都可以运行
   ```

2. ##### 项目介绍

   ```js
   1、components用于存放公用组件，可以是引入的ui，也可以是自己写的组件。
   2、pages用于业务逻辑界面书写。
   3、static静态文件（图片，资源文件等）
   4、App.vue入口界面，用于全局样式，或者应用的生命周期。
   	全局样式：
   	App.vue中的<style></style>一般不加scoped，因为这里需要定义全局样式，以及通过外部引入css样式
   
   	生命周期：
       // 用于进入应用时调用
       onLaunch: function(options) {
           console.log('App Launch');
           console.log(options);
       },
   	// 应用被唤起时调用
       onShow: function() {
           console.log('App Show');
       },
       // 应用关闭时调用
       onHide: function() {
           console.log('App Hide');
       }
   5、main.js入口文件。
   6、manifest.json配置文件。
   7、page.json配置页面信息。
   {
   	"pages": [{
   		"path": "pages/index/index",
   		"style": {
   			"navigationBarTitleText": "uni-ui基础项目"
   		}
   	},{
   		"path": "pages/login/login",
   		"style": {
   			"navigationBarTitleText": "login登录页"
   		}
   	},{
   		"path": "pages/mine/mine",
   		"style": {
   			"navigationBarTitleText": "我的界面"
   		}
   	}],
   	"tabBar": {
   	    "color": "#7A7E83",
   	    "selectedColor": "#3cc51f",
   	    "borderStyle": "black",
   	    "backgroundColor": "#ffffff",
   	    "list": [{
   	        "pagePath": "pages/index/index",
   	        // "iconPath": "static/image/icon_component.png",
   	        // "selectedIconPath": "static/image/icon_component_HL.png",
   	        "text": "首页"
   	    }, {
   	        "pagePath": "pages/login/login",
   	        // "iconPath": "static/image/icon_API.png",
   	        // "selectedIconPath": "static/image/icon_API_HL.png",
   	        "text": "登录"
   	    }, {
   	        "pagePath": "pages/mine/mine",
   	        // "iconPath": "static/image/icon_API.png",
   	        // "selectedIconPath": "static/image/icon_API_HL.png",
   	        "text": "我的"
   	    }]
   	},
   	"globalStyle": {
   		"navigationBarTextStyle": "white",
   		"navigationBarTitleText": "uni-app",
   		"navigationBarBackgroundColor": "#007AFF",
   		"backgroundColor": "#FFFFFF"
   	}
   }
   详解：
   pages是配置路由文件信息，默认第一个为当前激活状态（路由）
   tabBar底部菜单列表，与上方的路由文件相对应。（默认排序方向，从左往右）
   8、uni.scss 公用样式文件。
   ```

3. ##### 全局事件的触发和监听(跨父--子组件间的通讯方式)

   ```js
   微信小程序的生命周期函数
   监听indexMethod
   onLaunch:function(){
       uni.$on('indexMethod',(rel) => {
           console.log(rel);
       })
   }
   触发indexMethod
   emitMethod(){
       uni.$emit('indexMethod',{name:'luoliang'})
   }
   ```

4. ##### tabBar跳转和路由跳转

   ```js
   1、直接界面跳转,保留当前页面，跳转到应用内的某个页面（来回切换界面，会卡顿，因为没有销毁)
   uni.navigateTo({
       url: '../login/login'
   });
   这里的路径是相对路径
   
   可以通过路由地址传参：
   uni.navigateTo({
       url: '../login/login?id=123&name=luoliang'
   });
   接收界面(在onLoad生命周期函数里获取)
   onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
       console.log(option.id); //打印出上个页面传递的参数。
       console.log(option.name); //打印出上个页面传递的参数。
   }
   
   2、如果定义在pages.json中的tabBar的切换。
   uni.switchTab({
       url: '/pages/index/index'
   });
   
   3、使用redirectTo跳转，关闭当前页面，跳转到应用内的某个页面。
   uni.redirectTo({
       url: 'test?id=1'
   });
   
   4、返回 uni.navigateBack。关闭当前页面，返回上一页面或多级页面。可通过 getCurrentPages() 获取当前的页面栈，决定需要返回几层。
   ```

5. ##### http请求

   ```js
   
   ```

6. 虚位以待！