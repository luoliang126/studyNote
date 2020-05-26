# 微信小程序笔记

1. ##### 微信小程序开发（基于原生）

   ```js
   1、创建项目（需要AppID号，如果没有就不填）
   2、项目文件解析
   .js后缀的文件是脚本文件，
   .json后缀的文件是配置文件(config)，
   .wxss后缀的是样式表文件(类似.css)，
   .wxml后缀的文件是页面结构文件(类似.html)。
   3、 小程序的遍历
   <view wx:for="{{items}}" wx:for-index="index" wx:for-item="item">
       {{ + index+1 + }}、{{ + item.title + }}
   </view>
   其中：核心代码就是 wx:for，对应一个数组。而 wx:for-index 指明后面如果要用数组索引的话，用什么名字，如果名字是 index，则可省略，直接使用。
   而 wx:for-item 指明后面如果要用数组索引对应的项的话，用什么名字，如果名字是 item，则可省略，直接使用。对象中的 + 只是为了显示，代码中不需要
   4、小程序全局变量定义及获取使用
   在全局app.js中定义
   globalData: {
   	userInfo: null,
       totalNum:null,
       ajax: Ajax
   }
   使用时
   直接 var app = getApp();
   这里的app就是获取到app.js中的全局globalData对象
   直接使用 app.globalData
   ```

2. 虚位以待！