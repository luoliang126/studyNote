# css布局

1. ##### 左右布局（左侧固定，右侧自适应）

   ```css
   <div class="all">
       <div class="right">
       	<div class="right_in"> 内容</div>
       </div>
       <div class="left">我是固定的</div>
   </div>
   
   .all{width:100%;height:200px;position:relative;}
   .left{
   	width:200px;height:100%;float:left;
   	background:#990;margin-left: -100%;
   }
   .right{width:100%;background:#F90;height:100%;float:left;}
   .right_in{margin-left:200px;height:100%;}
   ```

2. ##### 左中右（左右固定，中间自适应）

   ```css
   <div class="all">
       <div class="left2">左</div>
       <div class="middle2">中</div>
       <div class="right2">右</div>
   </div>
   .all{position:relative;}
   .left2{width:200px;height:100%;background:red;float: left;}
   .middle2{height: 100%;background:blue;position: absolute;left:210px;right: 210px;}
   .right2{width:200px;height: 100%;background: green;float:right;}
   ```

3. ##### 上中下（上下固定，中间自适应）

   ```css
   // 使用定位方法
   <div class="box">
       <div class="head1">上</div>
       <div class="body1">中</div>
       <div class="foot1">下</div>
   </div>
   .box{height: 500px;width:100%;position: relative;}
   .head1 {
    	height:50px;line-height:50px;
    	background:#690;
    	width:100%;
    	position:absolute;
    	top:0;text-align:center;
   }
   .body1{
     	background:#FC0;
       width:100%;
       overflow:auto;
       top:50px;
       position:absolute;
       bottom:50px;
       text-align: center;
   }
   .foot1 {
       height:50px;
       line-height:50px;
       background:#690;
       width:100%;
       position:absolute;	
       bottom:0;
       text-align:center;
   }
   
   // table方法（不常用）
   <div class="box2">
       <div class="head2">上</div>
       <div class="body2">中</div>
       <div class="foot2">下</div>
   </div>
   .box2 {
       text-align:left;
       background:#666;
       display:table;
       width:100%;
       margin:0 auto;
       position:relative;
       height: 500px;
   }
   .head2 {
       background:#fcc;
       height:50px;
       vertical-align:bottom;
       display:table-row;
   }
   .body2 {background:#ccf;display:table-row;}
   .foot2{background:#fcc;height:50px;vertical-align:bottom;display:table-row;}
   ```

4. ##### 弹性盒子flex布局

   ```css
   详情，请查看css3 flex部分！
   ```
   
5. ##### vw和vh布局 参考：https://www.cnblogs.com/luxiaoxing/p/7544375.html

   ```css
   vh和vw：相对于视口（浏览器的可视区域）的高度和宽度，而不是父元素（CSS百分比是相对于包含它的最近的父元素的高度和宽度）的宽度和高度。1vh 等于1/100的视口高度，1vw 等于1/100的视口宽度。比如：浏览器高度950px，宽度为1920px, 1vh = 950px/100 = 9.5px，1vw = 1920px/100 = 19.2 px。
   1.vw：1vw等于视口宽度的1%。
   2.vh：1vh等于视口高度的1%。
   3.vmin：选取vw和vh中最小的那个（背景图片平铺问题）。
   4.vmax：选取vw和vh中最大的那个。
   5、再配合上css3的计算高度算法实现布局。
   eg:上--中--下布局
   上：高度60px
   中：自适应高度
   下：固定40px
   <div class="header"></div>
   <div class="body"></div>
   <div class="footer"></div>
   .header{
       width:100%;
       height:60px;
       background-color:red;
   }
   .body{
       width:100%;
       height:calc(100% - 60px); // 注意间隔
       background-color:blue;
   }
   .footer{
       position:fixed;
       width:100%;
       height:40px;
       bottom:0;
   }
   
   兼容性问题：在移动端iOS8以上，以及Android4.4以上获得支持，并且在微信x5内核中也得到完美的全面支持。
   ```

6. ##### css更换皮肤方案

   ```css
   1、使用css变量来进行主题色的修改，替换主题色变量，然后用setProperty来进行动态修改。（注意兼容问题）
   参考：https://www.cnblogs.com/leiting/p/11203383.html
   定义主题色：
   body{
      --themeColor:#000; // 初始的themeColor
   }
   使用时：
   .main{
      color: var(--themeColor);
   }
   修改主题色：
   document.body.style.setProperty('--themeColor', '#ff0000');
   
   2、基于sass，less的换肤方案
   ```

7. 虚位以待！

