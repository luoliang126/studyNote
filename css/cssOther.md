# css遇到的疑难杂症

1. ##### fixed定位问题，参考：https://www.cnblogs.com/mengff/p/9908958.html

   ```css
   position:fixed是相对于浏览器窗口定位的，但在实际项目中，我们往往会被父元素包裹，使用fixed定位：
   <div class="father">
   	<div class="fixed"></div>
   </div>
   .father{
   	width:100%;
   	height:100%;
   }
   .fixed{
   	position:fixed;
   	bottom:0;
   }
   这样是没问题的，fixed还是相对于浏览器窗口实现定位，但有时候，会受到father父元素的影响，而出现不能以窗口进行定位：
   1、第一种情况，父元素使用了动画效果
   <div class='father'>
     <div class='fix'>我固定了吗？</div>
   </div>
   .father{
     width: 200px;
     height: 200px;
     line-height: 200px;
     text-align: center;
     background: #6699FF;
     animation: move 4s cubic-bezier(0.4,0,0.6,1) infinite;
   }
   .fix{
     position: fixed;
     top: 20px;
     left: 20px;
     width: 200px;
     height: 200px;
     background: #9966FF;
     color:#FFF;
   }
   @keyframes move{
     0% {transform:translateX(100px);}
     50% {transform:translateX(500px);}
     100% {transform:translateX(100px);}
   }
   
   2、第二种情况，如果父级元素的z-index的层次比同级元素低，父元素下的子元素就算fixed的z-index比父级高，也会被父级同级元素遮挡。
   <div class="out">
       <div class="other">与父级同级</div>
           <div class='father1'>
           <div class='fix1'>子元素</div>
   	</div>
   </div>
   .out{
       position:relative;
       height:100px;
   }
   .other{
       width:100px;
       height:100px;
       position: absolute;
       z-index: 10;
       background-color:yellow;
   }
   .father1{
       width:100px;
       height:100px;
       position:absolute;
       z-index: 9;
       background-color:blue;
   }
   .fix1{
       width:50px;
       height:50px;
       position:absolute;
       background-color:red;
       z-index: 999;
   }
   总结：以上两种情况都证明了，fixed定位是以浏览器窗口为依据实现的定位，但是还是会受到包裹他的父元素影响。因此，position:fixed元素若要以窗口进行定位，最好是放在body根标签下。
   ```

2. ##### 字体图标库

   ```js
   阿里文字图标库 参考：http://www.iconfont.cn
   使用方法一：使用png/jpg/svg
   1、从阿里的图标库中，下载需要的图片SVG,png,jpg（可以选择颜色等）
   2、存放在项目中(建议放在assets目录下)
   3、定义css样式
   i.icon-back{
       width: 20px;
       height: 20px;
       background-image: url('../IconFont/back.svg')
   }
   i.icon-user{
       width: 20px;
       height: 20px;
       background-image: url('../IconFont/user.svg')
   }
   4、引入css，并使用
   <i class="icon icon-back"></i>
   <i class="icon icon-user"></i>
   5、当然也可以下载png格式的图片，不过同样一张图片svg只需要1-2kb，而png则需要4-5kb左右。
   尤其app端适合使用svg格式。
   <i class="icon icon-back"></i>
   <i class="icon icon-user"></i>
   修改svg图标的颜色：使用记事本打开svg图片,找到fill和path修改颜色
   
   使用方法二：symbol模式(这也是阿里推荐的使用方式)
   1、创建自己的阿里项目，并上传图片svg格式
   2、查看symbol的使用说明
   3、将项目中遇到的所有svg图片，通过下载至本地，会得到一个压缩包，解压后就可以看到我们的文件，放入assets目录中。
   4、找到相应的js，css文件，并main.js中引入
   // 引入icon的图标库
   import '@/assets/icon/iconfont.js'
   import '@/assets/icon/iconfont.css'
   5、使用时
   <svg class="icon user" aria-hidden="true">
       <use xlink:href="#icon-password"></use>
   </svg>
   其中xlink:href="#icon-password" 对应的样式就是取自阿里的图标库
   6、直接修改图标颜色和字体大小
   注意：可能会出现无法修改图标颜色和大小等问题，参考：https://www.cnblogs.com/jopny/p/9454785.html
   解决办法一：找到js文件（压缩过后的，然后fill="#......"，将颜色填充fill取消）
   解决办法二：在阿里图标网上，选择批量操作，选择批量去色，然后重新下载项目，再导入即可
   
   使用方法三：symbol可以使用本地包模式，也可以使用线上模式，需要引入一个js文件，就在定义图标那里生成！！！
   在项目的index.html中直接引入该js即可。
   注意：通过https，http都可同时引入，唯一不同的是，ios系统无法识别http，所以建议都用https。
   
   手动制作字体图标库，参考：http://caibaojian.com/svg-iconfont.html
   实现步骤，使用ps出图生成svg格式，在利用icomoon生成字体图标（线上方法）。导出字体图标库。
   ```

3. ##### 滚动条样式

   ```js
   在IE下（此处无法演示，直接代码）
   /*IE下的滚动条(属性直接加在需要产生滚动条中,如body标签),注：IE下的滚动条只有颜色修改，没有宽度的修改*/
   body{
       margin: 0;padding: 0;
       scrollbar-arrow-color: #f4ae21; /*三角箭头的颜色*/
       scrollbar-face-color: #333; /*滚动条的颜色*/
       scrollbar-track-color: #999; /*滚动条背景颜色*/
       scrollbar-3dlight-color: #666; /*滚动条亮边的颜色（不常用）*/
       scrollbar-highlight-color: #666; /*滚动条空白部分的颜色（不常用）*/
       scrollbar-shadow-color: #999; /*滚动条阴影的颜色（不常用）*/
       scrollbar-darkshadow-color: #666; /*立体滚动条强阴影的颜色（不常用）*/
       scrollbar-base-color:red; /*滚动条的基本颜色（不常用）*/
       Cursor:url(mouse.cur); /*自定义个性鼠标*/
   }
   
   在webkit内核中
   /*webkit下的滚动条，全局作用不限标签*/
   /*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸,有width,height,background,border*/
   ::-webkit-scrollbar {
       width: 16px;
       height: 16px;
       background-color: #F5F5F5;
   }
   /*定义滚动条外轨道 内阴影+圆角，可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果*/
   ::-webkit-scrollbar-track{
   	-webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
       border-radius: 10px;
       background-color: #F5F5F5;
   }
   /*定义滑块 内阴影+圆角，滚动条里面可以拖动的那部分*/
   ::-webkit-scrollbar-thumb{
       border-radius: 10px;
       -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
       background-color: #555;
   }
   /*内层轨道，滚动条中间部分*/
   ::-webkit-scrollbar-track-piece{
   
   }
   /*滚动条两端的按钮。可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果。*/
   ::-webkit-scrollbar-button{
       display: block;
       background: blue;
   }
   /*边角*/
   ::-webkit-scrollbar-corner{
   
   }
   /*定义右下角拖动块的样式*/
   ::-webkit-resizer{
   
   }
   
   在fireFox Gecko内核下
   /* 滚动条背景基本样式 */
   scrollbar{
       -moz-appearance: none !important;
       background-color: transparent !important;/* 滚动条背景透明 */
       background-image: none !important; /* 滚动条背景图案不显示 */
       position: relative !important; /* 更改滚动条的定位方式为相对 */
       overflow: hidden !important;
       z-index: 999999999 !important; /* 把滚动条提到Z轴最上层 */
   }
   /* 滚动条按钮基本样式 */
   scrollbar thumb{
       -moz-appearance: none !important;
       background-color: rgba(0,100,255,.25) !important;
       border-radius: 0px !important;
       border: 1px !important; /* 滚动条按钮边框 */
       border-color: rgba(0,100,255,.1) !important;  /* 滚动条按钮边框颜色和透明度 */
   }
   /* 滚动条按钮:鼠标悬停与点击拖动时基本样式 */
   scrollbar:hover thumb,
   scrollbar thumb:hover,
   scrollbar thumb:active {
       background-color: rgba(0,100,255,.75) !important;
       border: 0px !important;
   }
   /* 垂直滚动条 */
   /* 把滚动条位置移到屏幕外，这里的像素应该等于垂直滚动条宽度的负值 */
   scrollbar[orient="vertical"]{ margin-left: -5px !important;
   	min-width: 5px !important; max-width: 5px !important;
   }
   /* 垂直滚动条按钮的左边框样式 */
   scrollbar thumb[orient="vertical"]{
       border-style: none none none solid !important;
   }
   /* 水平滚动条 */
   /* 把滚动条位置移到屏幕外，这里的像素应该等于垂直滚动条宽度的负值 */
   scrollbar[orient="horizontal"]{ margin-top: -5px !important;
   	min-height: 5px !important; max-height: 5px !important;
   }
   /* 水平滚动条按钮的上边框样式 */
   scrollbar thumb[orient="horizontal"]{
       border-style: solid none none none !important;
   }
   /* 去除垂直与水平滚动条相交汇的角落 */
   scrollbar scrollcorner{display: none ! important; }
   /* 滚动条两端按钮不显示 */
   scrollbar scrollbarbutton { display: none ! important; }
   /* 滚动条两端按钮，需要先删掉上面一行 */
   scrollbarbutton{ 
       -moz-appearance: none !important;
   	position: relative !important;
       overflow: hidden !important;
       background-color: rgba(0,100,255,.25) !important;
       border: none !important;
   }
   scrollbar:hover scrollbarbutton, scrollbar scrollbarbutton:hover{
       background-color: rgba(0,100,255,.75) !important;
   }
   /* 竖滚动条两端按钮的高度 */
   scrollbar[orient="vertical"] scrollbarbutton {
       max-height:10px !important; min-height:10px !important;
   }
   /* 横滚动条两端按钮的宽度 */
   scrollbar[orient="horizontal"] scrollbarbutton {
       max-width: 10px !important; min-width: 10px !important;
   }
   ```

4. ##### box-shadow

   ```css
   详情查看css3中的box-shadow部分
   ```

5. ##### 横向滚动css实现方法，参考：https://blog.csdn.net/w390058785/article/details/80373154

   ```css
   父元素
   .item{
       overflow-x: auto;
       white-space:nowrap;
   }
   可以配合使用取消横向滚动条，效果更佳
   ```

6. ##### border-radius

   ```css
   1、一个值的时候
   border-radius:10px  // 四周圆角10px
   
   2、两个值的时候
   border-radius:10px 0px; // 分别为左上、右下为10px，右上、左下为0px
   
   3、四个值的时候
   border-radius:10px 10px 0 0 // 分别为左上、右上、右下、左下
   ```

   

7. 虚位以待！！！