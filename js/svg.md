# SVG画图

1. ##### svg介绍

   svg是使用xml来描述二维图形和绘图的工具。

   svg特点：

   1、可伸缩的矢量图形，是对图像的形状描述，本质上是文本类型，体积很小，所以矢量图形不会因为压缩放大而变形失真，而其他的绘图工具（比如canvas）都是基于像素处理（在不同分辨率下会失真）。

   2、svg是文本，它可以直接插入dom树结构中，成为dom的一部分，也可以使用css、js来进行操作，所以它是可以交互的。

2. ##### svg的形状绘图

   ```
   svg基础
   定义外层svg标签 <svg width="500" height="500" viewBox="50 50 50 50"></svg>
   width、height:分别代表svg在当前dom中，所占的宽、高
   viewBox(x,y,w,h):视图窗口的位置、大小。可选，左上角的x,y坐标，视图的宽、高
   
   svg的样式属性
   fill：填充色
   stroke：描边色
   stroke-width：边框宽度
   
   绘制线条<line>
   <line x1="0" y1="0" x2="300" y2="300" style="stroke:rgb(99,99,99);stroke-width:10"></line>
   x1、y1：定义线条开始点的坐标（细心就会发现，如果从0，0点开始，那么画出来的线条，起始位置的线条应该是有直角的，因为超出了svg的范围，隐藏掉了）
   x2、y2：定义线条结束点的坐标
   
   绘制折线<polyline>
   <polyline 
       points="10,10,20,20,20,40,40,40,40,60"
       style="fill:white;stroke:black;stroke-width:2"
   ></polyline>
   points:全是点的坐标，每2个值算一组，分别对应x，y坐标
   注意：在绘制折线的时候，如果不加fill填充色为svg背景色的时候，那么会出现填充效果，这并不是我们想要的折线效果！！！
   
   绘制多边形<polygon>
   <polygon 
   	points="220,100 300,210 170,250 123,234"
   	style="fill:#cccccc;stroke:#000000;stroke-width:1"
   ></polygon>
   points:多边形的各个坐标
   
   绘制路径<path>
   <path 
       d="M153 334
       C153 334 151 334 151 334
       C151 339 153 344 156 344
       C164 344 171 339 171 334
       C171 322 164 314 156 314
       C142 314 131 322 131 334
       C131 350 142 364 156 364
       C175 364 191 350 191 334
       C191 311 175 294 156 294
       C131 294 111 311 111 334
       C111 361 131 384 156 384
       C186 384 211 361 211 334
       C211 300 186 274 156 274"
   	style="fill:white;stroke:red;stroke-width:2"
   ></path>
   d：代表绘制顺序，是一个字符串，内容就是动作与作画时的点
   M： moveto 移动到
   L： lineto 直线到
   H： horizontal lineto 水平直线到
   V： vertical lineto 垂直直线到
   C： curveto 曲线
   S： smooth curveto 平滑曲线
   Q： quadratic Belzier curve 二次贝兹曲线
   T： smooth quadratic Belzier curveto 光滑二次贝兹曲线
   A： elliptical Arc 椭圆弧
   Z： closepath 闭合路径，回到起始点
   注意：不区分大小写！！！
   
   绘制矩形图<rect>
   <rect 
       width="300" 
       height="100"
       style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)"
   ></rect>
   <rect 
   	x="20" 
   	y="20" 
   	rx="10"
   	ry="10"
   	width="250" 
   	height="250"
   	style="fill:blue;stroke:pink;stroke-width:5;fill-opacity:0.1;stroke-opacity:0.9"
   ></rect>
   x,y:矩形的起始点坐标
   rx，ry：矩形圆角
   
   绘制圆形<circle>
   <circle 
   	cx="100" 
   	cy="50" 
   	r="40" 
   	stroke="black"
   	stroke-width="2" 
   	fill="red"
   ></circle>
   cx,cy:圆心坐标（没有，就默认0,0）
   r：圆半径
   
   绘制椭圆<ellipse>
   <ellipse 
   	cx="300" 
   	cy="150" 
   	rx="200" 
   	ry="80"
   	style="fill:rgb(200,100,50);stroke:rgb(0,0,100);stroke-width:2"
   ></ellipse>
   cx,cy:椭圆的圆点坐标
   rx，ry：椭圆的水平半径，垂直半径
   
   绘制文本<text>
   <text x="300" y="300">
   	Hello world
   </text>
   x,y：文本起始点坐标
   
   复制一个形状<use>
   <rect 
   	id="rect"
       width="300" 
       height="100"
       style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0)"
   ></rect>
   <use href="#rect" x="10" y="10"></use>
   复制一个名为id=“rect”的图形，并把它放在x，y坐标处
   
   插入图片<image>
   <image 
   	xlink:href="path/image.jpg"
   	width="100"
   	height="100"
   ></image>
   xlink:href 图片的路径
   
   动画效果<animate>
   
   多个形状组合成一个组<g>
   
   自定义形状,内部代码不会显示出来<defs>
   
   自定义形状，该形状可以用来平铺一个区域<pattern>
   
   通过以上，发现要绘制复杂的图形也不是不可以，但如果靠代码去写的话会浪费时间，这里推荐一款在线编辑器
   链接：https://c.runoob.com/
   ```

   

3. ##### svg的滤镜效果

   ```
   
   ```

   

4. ##### svg的渐变效果

   ```
   
   ```

   

5. 虚位以待！！！