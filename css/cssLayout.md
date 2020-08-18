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

4. ##### 弹性盒子flex布局 参考：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

   ```css
   <div class="cssLayout-content">
       <div class="div2">项目二</div>
       <div class="div1">项目一</div>
       <div class="div3">项目三</div>
   </div>
   .cssLayout-content{
       display: -webkit-flex;
       display: -safri-flex;
       display: flex;
       flex-direction: row;
       flex-wrap: nowrap;
       justify-content: space-around;
       align-items:flex-start;
       align-content:flex-start;
   }
   1、flex-direction属性，决定主轴的方向（即项目的排列方向）
   	flex-direction: row | row-reverse | column | column-reverse;
       row（默认值）：主轴为水平方向，起点在左端，从左到右
       row-reverse：主轴为水平方向，起点在右端，从右到左
       column：主轴为垂直方向，起点在上沿，从上到下
       column-reverse：主轴为垂直方向，起点在下沿，从下到上
   2、flex-wrap属性，定义如果一条轴线排不下，如何换行 
   	flex-wrap: nowrap | wrap | wrap-reverse;
       nowrap（默认）：不换行。
       wrap：换行，第一行在上方（即多的在第一行，少的在第二行）。
       wrap-reverse：换行，第一行在下方(即多的在第二行，少的在第一行)。
   3、flex-flow属性：是flex-direction属性和flex-wrap属性的简写形式，
   	默认值为row nowrap （不常用）
   4、justify-content属性：定义了项目在主轴上的对齐方式 
   	justify-content: flex-start | flex-end | center | space-between | space-around;
       flex-start（默认值）：左对齐
       flex-end：右对齐
       center： 居中
       space-between：两端对齐，项目之间的间隔都相等。
       space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
   5、align-items属性，定义项目在交叉轴上如何对齐。
   	align-items: flex-start | flex-end | center | baseline | stretch;
       flex-start：交叉轴的起点对齐。
       flex-end：交叉轴的终点对齐。
       center：交叉轴的中点对齐（垂直居中！！！！！！）。
       baseline: 项目的第一行文字的基线对齐。
       stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
   6、align-content属性，定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用 
   	align-content: flex-start | flex-end | center | space-between | space-around | 						stretch;
       flex-start：与交叉轴的起点对齐。
       flex-end：与交叉轴的终点对齐。
       center：与交叉轴的中点对齐。
       space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
       space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
       stretch（默认值）：轴线占满整个交叉轴。
       
   cssLayout-content内部子元素
   .div1{
       order:1;
       flex-grow:2;
   }
   .div2{
       order:2;
       flex-shrink:1;
   }
   .div3{
       order:3;
       flex-basis: auto
   }
   1、order属性：定义项目的排列顺序。数值越小，排列越靠前，默认为0。
   2、flex-grow属性，定义项目的放大比例，默认为0即如果存在剩余空间，也不放大。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
   3、flex-shrink属性，定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
   4、flex-basis属性，定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
   5、flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选
   6、align-self属性，允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
   ```
   
5. ##### vw和vh布局 参考：https://www.cnblogs.com/luxiaoxing/p/7544375.html

   ```css
   vh和vw：相对于视口的高度和宽度，而不是父元素的（CSS百分比是相对于包含它的最近的父元素的高度和宽度）。1vh 等于1/100的视口高度，1vw 等于1/100的视口宽度。比如：浏览器高度950px，宽度为1920px, 1vh = 950px/100 = 9.5px，1vw = 1920px/100 = 19.2 px。
   1.vw：1vw等于视口宽度的1%。
   2.vh：1vh等于视口高度的1%。
   3.vmin：选取vw和vh中最小的那个（背景图片平铺问题）。
   4.vmax：选取vw和vh中最大的那个。
   
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

