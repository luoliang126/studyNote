# canvas画图

1. ##### canvas介绍

   ```js
   HTML5 canvas 元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成.
   canvas 标签只是图形容器，您必须使用脚本来绘制图形。
   你可以通过多种方法使用Canva绘制路径,盒、圆、字符以及添加图像，甚至制作动画。
   ```

2. ##### canvas用法

   ```js
   // 创建canvas画布，通过属性width、height设置宽高，不建议使用css样式。比如：我们设置了#myCanvas{width:400px;height:400px;},那么我们定义的属性width、height就会被拉升，从而影响视图效果
   <canvas id="myCanvas" width="200" height="200"></canvas>
   // 拿到canvas画布
   var c=document.getElementById("myCanvas");
   // 创建context对象：getContext("2d") 对象是内建的 HTML5 对象，拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。
   var ctx=c.getContext("2d"); // 2d视图，如果想创建3d的话，使用的是webgl 
   // 作画
   ctx.fillStyle="#FF0000";
   ctx.fillRect(0,0,150,75);
   
   getContext("2d")对象的属性和方法
   属性：
   1、fillStyle:设置或返回用于填充绘画的颜色、渐变或模式。
      createLinearGradient / createRadialGradient：填充绘图的渐变对象
   <canvas id="tianchong" width="200" height="100"></canvas>
   var tianchong = document.getElementById("tianchong");
   var tianchongEle = tianchong.getContext("2d");
   var grd = tianchongEle.createLinearGradient(0,0,170,0);//1.渐变开始点的x坐标 2.渐变开始点的y坐标 3.渐变结束点的x坐标 4.渐变结束点的y坐标
   grd.addColorStop(0,"black");    //两种色由0-->1，三种由0-->0.5-->1,依次类推越多色彩越丰富
   grd.addColorStop(1,"white");
   tianchongEle.fillStyle=grd;     //将最终渐变色给fillStyle
   tianchongEle.fillRect(20,20,150,100);   //fillRect为实心填充方式，1.X方向偏移量 2.y方向偏移量 3.宽度 4.高度
   
   <canvas id="tianchong1" width="200" height="100"></canvas>
   var tianchong1 = document.getElementById("tianchong1");
   var tianchong1Ele = tianchong.getContext("2d");
   //1.渐变的开始圆的x坐标 2.渐变的开始圆的y坐标 3.开始圆的半径 4.渐变的结束圆的x坐标 5.渐变的结束圆的y坐标 6.结束圆的半径
   var grd=ctx.createRadialGradient(75,50,5,90,60,100);  
   grd.addColorStop(0,"red");
   grd.addColorStop(1,"white");
   tianchong1Ele.fillStyle=grd;
   tianchong1Ele.fillRect(20,20,150,100);
   
   2、strokeStyle:设置或返回用于笔触的颜色、渐变或模式。
   strokeStyle和fillStyle使用方法类似(strokeStyle是边框，fillStyle是实心)，strokeRect和fillRect使用方法类似
   
   3、shadowColor:设置或返回用于阴影的颜色。(与shadowBlur配合使用)
   
   4、shadowBlur:设置或返回用于阴影的模糊级别。(与shadowColor配合使用)
   
   5、shadowOffsetX:设置或返回阴影与形状的水平距离。
   
   6、shadowOffsetY:设置或返回阴影与形状的垂直距离。
   <canvas id="myCanvas1" width="200" height="100"></canvas>
   var shadowPosition=document.getElementById("myCanvas1");
   var shadowPositionEle=shadowPosition.getContext("2d");
   shadowPositionEle.shadowBlur=10;    //值越大越模糊，模糊宽度越宽
   shadowPositionEle.shadowOffsetX=20; //X方向的偏移量
   shadowPositionEle.shadowOffsetY=20; //Y方向的偏移量
   shadowPositionEle.shadowColor="black";
   shadowPositionEle.fillStyle="red";
   shadowPositionEle.fillRect(20,20,100,80);
   
   7、lineCap:设置或返回线条末端线帽的样式
   ctx.lineCap="round";
   butt:"默认",向线条的每个末端添加平直的边缘。
   round:向线条的每个末端添加圆形线帽
   square:向线条的每个末端添加正方形线帽。
   
   8、lineJoin:当两条线条交汇时的交点形状
   ctx.lineJoin="round";
   bevel:斜角
   round:圆角
   miter:(默认)尖角。
   
   9、lineWidth:设置或返回当前的线条宽度。（number类型）
   
   10、miterLimit:设置或返回最大斜接长度。
   
   方法：
   1、clearRect()在给定的矩形内清除指定的像素。
   
   2、beginPath()起始一条路径，或重置当前路径。(用于在一个canvas对象中，画多条或多个图)
   
   3、closePath()创建从当前点回到起始点的路径。
   
   4、moveTo(x,y)把路径移动到画布中的指定点，不创建线条。
   x:X坐标
   y:Y坐标
   
   5、lineTo(x,y)添加一个新点，然后在画布中创建从该点到最后指定点的线条。
   x:X坐标
   y:Y坐标
   
   6、arc(x,y,r,sAngle,eAngle,couterclockwise)创建弧/曲线（用于创建圆形或部分圆）
   x:X坐标，y:Y坐标 （注意是圆心坐标）
   r:半径 (需要作画圆弧的半径)
   sAngle,eAangle:起始角度(是以弧度计算！！！)
   couterclockwise:可选参数，默认为false（顺时针），true(逆时针)
   弧度计算公式：
   0----π/2(90度)----π(180度)----3π/2(270度)----2π(360度即0度)
   度与弧度的转换公式：
   度 = 弧度 x 180°/π ===》 弧度 = 度 x π/180度
   
   7、arcTo()创建两切线之间的弧/曲线。
   
   8、rotate()旋转当前绘图。
      
   9、translate()	重新映射画布上的 (0,0) 位置。
      
   10、transform()	替换绘图的当前转换矩阵
       
   11、setTransform()	将当前转换重置为单位矩阵。然后运行 transform()。
       
   12、fillText()	在画布上绘制"被填充的"文本。
       
   13、strokeText()	在画布上绘制文本（无填充）。
       
   14、drawImage()	向画布上绘制图像、画布或视频。
   
   15、stroke() 之前的各种操作可以理解为描线（只是一个轮廓，并没有实际作画），执行stroke方法，才是真正在canvas上画。
   ```

3. 虚位以待