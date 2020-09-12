# css响应式布局

1. ##### 使用@media媒体查询方式

   ```css
   <div class="response1">
   	这里是响应式页面
   </div>
   @media screen and (min-width:1200px){
   	.response1{
       	width:500px;
       }
   }
   
   @media screen and (min-width: 960px) and (max-width: 1199px) {
   	.response1{
       	width:400px;
       }
   }
   
   @media screen and (min-width: 768px) and (max-width: 959px) {
       .response1{
    	   width:300px;
       }
   }
   
   @media only screen and (min-width: 480px) and (max-width: 767px){
   
   }
   
   @media only screen and (max-width: 479px) {
   
   }
   一般分为以上几个阶段，低于768px的一般就是平板或者移动端，可以根据js来判断，大于1200的可以再分例如：1366px，1440px，1920px
   ```

2. ##### meta标签的使用

   ```css
   <meta name="viewport" content="height = [pixel_value | device-height] ,width = [pixel_value | device-width ] ,initial-scale = float_value, minimum-scale = float_value ,maximum-scale = float_value, user-scalable = [yes | no] , target-densitydpi = [dpi_value | device-dpi | high-dpi | medium-dpi | low-dpi]"/>
   
   width：控制viewport的宽度，可以指定为一个固定宽度，也可以为默认设备的宽度device-width（缩放为 		100% 时的 CSS 的像素）
   height：同width一样的设备高度，可以为固定值，也可以为height = device-height 对应的设备高度
   initial-scale: 初始缩放。即页面初始缩放程度。这是一个浮点值，是页面大小的一个乘数。例如，如果你设置		初始缩放为“1.0”，那么，web页面在展现的时候就会以target density分辨率的1:1来展现。
   		如果你设置为“2.0”，那么这个页面就会放大为2倍。
   maximum-scale, minimum-scale :最大/最小缩放。即允许的最大/最小缩放程度。这也是一个浮点值，用以指		出页面大小与屏幕大小相比的最大乘数。例如，如果你将这个值设置为“2.0”，
   		那么这个页面与target size相比，最多能放大2倍。
   user-scalable: 用户是否可以缩放界面 yes可以，no不可以，如果设了no那 maximum-scale, minimum-		scale无效
   target-densitydpi：一个屏幕像素密度是由屏幕分辨率决定的，通常定义为每英寸点的数量（dpi）。Android		支持三种屏幕像素密度：低像素密度，中像素密度，高像素密度。默认为始终，
   		一般不建议操作，如果非要操作请查看			 
   		http://www.cnblogs.com/zonglonglong/p/4205434.html
   ```

3. ##### rem布局

   ```js
   原理：不久前打开QQ邮箱，发现一个现象，当屏幕很小的时候QQ邮箱的字体就很小，当浏览器不断的拖大的过程中字体就好跟着慢慢变大。这让我甚是惊奇，我也一直想实现这样的一个屏幕适配方案。后来有了下面的方案，其实就是为了把这个方案分享出去才会有了这篇文章，说起来也简单，在采用rem作为大小单位的同时通过JS根据屏幕大小计算出一个合适的数值赋值给当前的html即可。在onLoad的时候执行下面的代码（后面的20根据自己的需要来改
   
   var fontSize = $(window).width()/20;//屏幕的宽
   $("html").css("font-size",fontSize +"px");
   
   var fontSize = document.body.clientWidthw
   var htmlElement = document.getElementsByTagName("html")[0];
   htmlElement.style.fontSize=fontSize+”px”;
   其实不难看出，就是拿当前的屏幕尺寸去除一个数值，得到字体大小，进而将这个字体大小赋值给html根节点（根标签）即可，然后全文设置字体大小或控件大小时使用rem作为单位。也可以配合百分比设置控件大小。这样一来基本上可以根据屏幕尺寸完美缩放自己的界面了。
   
   // es6语法封装
   class iRem {
       constructor() {}
       static install(doc, win) {
           var docEl = doc.documentElement,
           var isPad = (docEl.clientWidth || window.innerWidth) >= 768;
           var clientWidth = docEl.clientWidth;
           if (!clientWidth) return;
           isPad ? docEl.style.fontSize = 100 * (clientWidth / 768) + 'px' : 		
           docEl.style.fontSize = 100 * (clientWidth / 375) + 'px';
       }
   }
   export default iRem;
   // 使用
   iRem.install(document);
   ```
   
4. 虚位以待！