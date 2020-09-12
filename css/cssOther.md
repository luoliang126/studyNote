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

2. 虚位以待