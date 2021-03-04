# css3新特性

参考：https://www.jianshu.com/p/56b7302d7f7f

1. ##### css3 变量var的使用。参考：https://www.zhangxinxu.com/wordpress/2016/11/css-css3-variables-var/

   ```css
   CSS中原生的变量定义语法是：--变量名称。JS中的变量只能是以字母、_、$开头，但在CSS变量中，这些限制通通没有！
   使用语法时：var(--变量名称)
   eg：
   body {
     --深蓝: #369;
     background-color: var(--深蓝);
   }
   注意：css变量语法
   1、唯一性，变量一次只能声明一个，不能同时声明多个。
   2、变量覆盖
   .box {
     --1: #369;
   }
   body {
     background-color: var(--1, #cd0000);
   }
   最终background-color:#Cd0000;
   3、变量不合法的缺省特性
   body {
       --color: 20px;
       background-color: #369;
       background-color: var(--color, #cd0000);
   }
   // 上面等同于
   body {
       --color: 20px;
       background-color: #369;
       background-color: transparent;
   }
   所以background-color:transparent;
   CSS变量，只要语法是正确的，就算变量里面的值是个乱七八糟的东西，也是会作为正常的声明解析，如果发现变量值是不合法的，例如上面背景色显然不能是20px，则使用背景色的缺省值，也就是默认值代替
   4、完整声明变量，不要为自己偷懒找借口！！！
   body {
     --size: 20;   
     font-size: var(--size)px;
   }
   你以为font-size:20px 吗？太天真了！！！语法错误，所以会取浏览器默认值font-size:14px;
   完整定义才是，写好代码的关键！
   body {
     --size: 20px;   
     font-size: var(--size);
   }
   // 当然也可以这样
   body {
     --size: 20;   
     font-size: calc(var(--size) * 1px);
   }
   ```

2. ##### **过渡** transition

   ```css
   过渡是元素从一种样式逐渐改变为另一种的效果。
   实现过渡效果的两个要件：
       规定把效果添加到哪个 CSS 属性上
   	规定效果的时长
   语法：transition： transition-property transition-duration transition-timing-function transition-delay
   
   1、transition-property：属性名称
   none：将过渡效果设置为“无过渡效果”，或停止当前过渡效果。
   all：（默认）为所支持的所有CSS属性在值变化时执行动画过渡效果。
   
   2、transition-duration：该属性是用于设定一个属性的值过渡被触发开始时到结束时所需要经历的时间，单位为秒（s），如：“0.3s”或“.3s”，它的默认值为“0”，单位不能省略，否则过渡动画无法执行。
   
   3、transition-timing-function：过渡方法
   ease：默认值，逐渐变慢
   linear：匀速
   ease-in：加速
   ease-out：减速
   ease-in-out：先加速，后减速
   cubic-bezier([参数])：定义一个时间曲线（贝塞尔曲线），可以为其配置四个参数，前两个参数为“ x1 ”和“ x2 ”，定义“开始控制点”，后两个参数为“ y1 ”和“ y2 ”，定义“结束控制点”。而“开始点”和“结束点”是通过这两条“转换点控制轴”分别去调整两个点来实现曲线的变化的，如果对Photoshop里面的“钢笔工具”的“路径”比较熟悉的话，稍加联想应该能理解到这个时间曲线轴的改变原理，这种曲线叫做“贝塞尔曲线”。
   
   4、transition-delay：过渡延迟，该属性规定在用户进行操作后多久开始执行动画，也就是延迟执行过渡动画的时间，单位同样是秒“s”，默认值同样为“0”。
   
   怎样使用transition？
   1、transition：属性 时长;
   .transition-div{
       width:100px;
       height:100px;
       background-color:red;
       transition:width 1s;
   }
   .transition-div:hover{
       width:200px;
       height:200px
   }
   鼠标移入后，div宽度由100px变为200px，逐渐变慢
   2、transition: 属性 时长 过渡方法 延迟
   .transition-div{
       width:100px;
       height:100px;
       background-color:red;
       transition:width 3s ease-in-out 1s;
   }
   .transition-div:hover{
       width:200px;
       height:200px
   }
   鼠标移入后，延迟1s后，div宽度由100px变为200px,先加速后减速，持续时间3s
   3、多个过渡效果
   属性1，属性2，......：指定一个或多个属性名称列表，以逗号“,”进行分隔，当指定的属性产生变化时，为其执行指定属性的动画过渡效果。
   .transition-div{
       width:100px;
       height:100px;
       background-color:red;
       transition:width 1s, height 1s;
       transition:all 1s // 当然定义一个all全部过渡效果也可以实现
   }
   .transition-div:hover{
       width:200px;
       height:200px
   }
   ```

   

3. ##### **动画** animation

   ```css
   语法：
   animation: animation-name animation-duration animation-timing-function animation-fill-mode animation-delay animation-iteration-count animation-direction animation-play-state;
   
   1、animation-name：规定一个动画名称，需要绑定到选择器的 keyframe 名称。
   @keyframes myfirst {
       0%   {background: red; left:0px; top:0px;}
       25%  {background: yellow; left:200px; top:0px;}
       50%  {background: blue; left:200px; top:200px;}
       75%  {background: green; left:0px; top:200px;}
       100% {background: red; left:0px; top:0px;}
   }
   
   2、animation-duration：规定动画完成一个周期所花费的秒或毫秒。默认是0。
   
   3、animation-timing-function：规定动画的速度曲线。默认是 "ease"。
   linear:动画从头到尾的速度是相同的。
   ease: 默认。动画以低速开始，然后加快，在结束前变慢。
   ease-in：动画以低速开始。
   ease-out：动画以低速结束。
   ease-in-out：动画以低速开始和结束。
   cubic-bezier(n,n,n,n)：在cubic-bezier函数中自己的值。可能的值是从0到1的数值。
   
   4、animation-fill-mode：规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。
   none：默认值。动画在动画执行之前和之后不会应用任何样式到目标元素。
   forwards：在动画结束后（由 animation-iteration-count 决定），动画将应用该属性值。
   backwards：动画将应用在 animation-delay 定义期间启动动画的第一次迭代的关键帧中定义的属性值。这些都是 from 关键帧中的值（当 animation-direction 为 "normal" 或 "alternate" 时）或 to 关键帧中的值（当 animation-direction 为 "reverse" 或 "alternate-reverse" 时）。
   both：动画遵循 forwards 和 backwards 的规则。也就是说，动画会在两个方向上扩展动画属性。
   initial：设置该属性为它的默认值。
   inherit：从父元素继承该属性。
   
   5、animation-delay：规定动画何时开始，默认是0不延迟。
   
   6、animation-iteration-count：规定动画被播放的次数。默认是1。
   n：一个数字，定义应该播放多少次动画
   infinite：指定动画应该播放无限次（永远）
   
   7、animation-direction：规定动画是否在下一周期逆向地播放。默认是 "normal"。
   normal：默认值。动画按正常播放
   reverse：动画反向播放。
   alternate：动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。
   alternate-reverse：动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。
   initial：设置该属性为它的默认值。
   inherit：从父元素继承该属性。
   
   8、animation-play-state：规定动画是否正在运行或暂停。默认是 "running"。
   paused：指定暂停动画
   running：指定正在运行的动画
   
   使用方法：
   .animation-div{
       animation: myfirst 5s linear 2s infinite alternate;
       /* Safari 与 Chrome: */
       -webkit-animation: myfirst 5s linear 2s infinite alternate;
   }
   @keyframes myfirst {
       0%   {background: red; left:0px; top:0px;}
       25%  {background: yellow; left:200px; top:0px;}
       50%  {background: blue; left:200px; top:200px;}
       75%  {background: green; left:0px; top:200px;}
       100% {background: red; left:0px; top:0px;}
   }
   @-webkit-keyframes myfirst {  /* Safari 与 Chrome */
       0%   {background: red; left:0px; top:0px;}
       25%  {background: yellow; left:200px; top:0px;}
       50%  {background: blue; left:200px; top:200px;}
       75%  {background: green; left:0px; top:200px;}
       100% {background: red; left:0px; top:0px;}
   }
   div在2s后，按照相同的速度，往复不停的执行myfirst动画。
   myfirst动画：在奇数次（1、3、5...）正向播放0%--25%--50%--75%--100%，在偶数次（2、4、6...）反向播放100%--75%--50%--25%--0%。
   
   
   ```

   

4. ##### **形状转换** transform

   ```css
   transform属性应用于元素的2D或3D转换。这个属性允许你将元素旋转，缩放，移动，倾斜等。
   
   语法：
   transform: none|transform-functions;
   none：定义不进行转换。
   matrix(n,n,n,n,n,n)：定义2D转换，使用六个值的矩阵。
   matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)：定义3D转换，使用16个值的4x4矩阵。
   translate(x,y)：定义2D转换。
   translate3d(x,y,z)：定义3D转换。
   translateX(x)：定义转换，只是用X轴的值。
   translateY(y)：定义转换，只是用Y轴的值。
   translateZ(z)：定义3D转换，只是用Z轴的值。
   scale(x[,y]?)：定义2D缩放转换。
   scale3d(x,y,z)：定义3D缩放转换。
   scaleX(x)：通过设置X轴的值来定义缩放转换。
   scaleY(y)：通过设置Y轴的值来定义缩放转换。
   scaleZ(z)：通过设置Z轴的值来定义3D缩放转换。
   rotate(angle)：定义2D旋转，在参数中规定角度。
   rotate3d(x,y,z,angle)：定义3D旋转。
   rotateX(angle)：定义沿着X轴的3D旋转。
   rotateY(angle)：定义沿着Y轴的3D旋转。
   rotateZ(angle)：定义沿着Z轴的3D旋转。
   skew(x-angle,y-angle)：定义沿着X和Y轴的2D倾斜转换。
   skewX(angle)：定义沿着X轴的2D倾斜转换。
   skewY(angle)：定义沿着Y轴的2D倾斜转换。
   perspective(n)：为3D转换元素定义透视视图。
   
   注：配合animation可以制作不同的动画效果
   ```

   

5. ##### 阴影shadow（包括：文本text-shadow、盒模box-shadow）

   ```css
   文本text-shadow
   语法：
   text-shadow: X-offset Y-offset blur color;
   X-offset:必须。阴影水平偏移量，其值可以是正负值。如果值为正值，则阴影在对象的右边，其值为负值时，阴影在对象的左边；
   Y-offset:必须。阴影垂直偏移量，其值也可以是正负值。如果为正值，阴影在对象的底部，其值为负值时，阴影在对象的顶部；
   blur：可选。阴影模糊半径，但其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
   color:可选。阴影颜色，如不设定颜色，浏览器会取默认色，但各浏览器默认取色不一致，建议不要省略此参数。
   
   盒模box-shadow
   box-shadow规定了边框的阴影部分
   语法：
   box-shadow: inset X-offset Y-offset radius spread color;
   
   inset：可选。如不设值，默认投影方式是外阴影，如取其唯一值“inset”，其投影为内阴影。
   X-offset:必须。阴影水平偏移量，其值可以是正负值。如果值为正值，则阴影在对象的右边，其值为负值时，阴影在对象的左边；
   Y-offset:必须。阴影垂直偏移量，其值也可以是正负值。如果为正值，阴影在对象的底部，其值为负值时，阴影在对象的顶部；
   blur：可选。阴影模糊半径，但其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
   spread：可选。阴影扩展半径，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
   color：可选。阴影颜色，如不设定颜色，浏览器会取默认色，但各浏览器默认取色不一致，建议不要省略此参数。
   
   使用：
   1、单边阴影：box-shadow:inset x-offset y-offset blur spread color;
   .box-shadow{
       width:100px;
       height:100px;
       background-color:red;
       box-shadow:inset 0px 10px 10px -5px #000;
   }
   div上边10px、内阴影，模糊10px，阴影缩小-5px，阴影颜色#000。注意-5px很有用！！！
   
   2、双边阴影：box-shadow:x-offset y-offset blur color;
   .box-shadow{
       width:100px;
       height:100px;
       background-color:red;
       box-shadow: 10px 10px 10px blue;
   }
   往 右/下 方向偏置10px，模糊10px（阴影不扩展），颜色为blue
   
   3、四边阴影：box-shadow:x-offset y-offset blur spread color;
   .box-shadow{
       width:100px;
       height:100px;
       background-color:red;
       box-shadow:0px 0px 10px 5px #000;
   }
   div四周阴影，阴影10px，扩散5px，颜色#000
   
   4、四周单独阴影：box-shadow: x-offset y-offset blur spread color, x-offset y-offset blur spread color......;
   根据偏置量，单独定义每边的颜色，以及缩放等
   .box-shadow{
       width:100px;
       height:100px;
       background-color:red;
       box-shadow:0px 10px 10px -5px #000,10px 0px 10px -5px blue,-10px 0 10px -5px purple,0 -10px 10px -5px yellow;
   }
   
   5、关于外阴影/内阴影 偏移量问题？
   // 外阴影
   box-shadow: 0px -10px 0px 0px #ff0000,   /*上边阴影  红色*/
               10px 0px 0px 0px #2279ee,    /*右边阴影  蓝色*/
               0px 10px 0px 0px #eede15,    /*下边阴影  黄色*/
               -10px 0px 0px 0px #3bee17;   /*左边阴影  绿色*/
   // 内阴影
   box-shadow: inset 0px 10px 0px 0px #eede15;    /*上边内阴影  黄色*/
   			inset -10px 0px 0px 0px #3bee17,   /*右边内阴影  绿色*/
   			inset 0px -10px 0px 0px #ff0000,   /*下边内阴影  红色*/
               inset 10px 0px 0px 0px #2279ee,    /*左边内阴影  蓝色*/
   细心点就会发现，内/外阴影其实是不一样的，那怎样记住呢？
   最直观的就是建立一个坐标系：
   以内阴影为基准：在当前边往上为正，往右为正，往下为负，往左为负
   那么外阴影则刚好相反：在当前边往上为负，往右为负，往下为正，往左为正
   ```

   

6. ##### 边框border-image，border-width

7. ##### 文字换行

8. ##### 颜色rgba（rgb为颜色值，a为透明度） 

9. ##### 渐变

   ```css
   线性渐变（上下、左右、角度方向）
   background: linear-gradient(direction, color-stop1, color-stop2, ...); // 颜色越多渐变越多
   1、从上到下（默认情况下）
   h2{
       background: -webkit-linear-gradient(red, blue); /* Safari 5.1 - 6.0 */ 
       background: -o-linear-gradient(red, blue); /* Opera 11.1 - 12.0 */ 
       background: -moz-linear-gradient(red, blue); /* Firefox 3.6 - 15 */
       background: linear-gradient(red, blue); /* 标准的语法 */
   }
   2、从左到右
   h2{
       background: -webkit-linear-gradient(left, red , blue); /* Safari 5.1 - 6.0 */ 
       background: -o-linear-gradient(right, red, blue); /* Opera 11.1 - 12.0 */ 
       background: -moz-linear-gradient(right, red, blue); /* Firefox 3.6 - 15 */ 
       background: linear-gradient(to right, red , blue); /* 标准的语法 */
   }
   3、线性渐变：对角 //从左上角到右下角
   h2{
       background: -webkit-linear-gradient(left top, red , blue); /* Safari 5.1 - 6.0 */ 
       background: -o-linear-gradient(bottom right, red, blue); /* Opera 11.1 - 12.0 */ 
       background: -moz-linear-gradient(bottom right, red, blue); /* Firefox 3.6 - 15 */ 
       background: linear-gradient(to bottom right, red , blue); /* 标准的语法 */
   }
   4、任意角度渐变（注意0deg是从y轴正向开始，顺时针旋转！！！）
   background: linear-gradient(-45deg, red, yellow);
   5、透明度写法
   background: linear-gradient(to bottom right, rgba(255,0,0,0), rgba(255,0,0,1));
   
   径向渐变（由内到外渐变）
   background: radial-gradient(shape size at position, start-color, ..., last-color);
   1、颜色节点均匀分布的径向渐变（三个各占33.3333%）
   background: radial-gradient(red, yellow, green);
   2、颜色节点按比例分布的径向渐变（三个按指定比例占比）
   background: radial-gradient(red 10%, yellow 10%, green 80%);
   3、设置形状（圆形circle宽高应保持一直，椭圆形ellipse宽高不一致），当然这需要配合容器的宽高来使用！！！
   background: radial-gradient(circle, red, yellow, green);
   ```

   

10. ##### 弹性布局flex，参考：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

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

    

11. ##### 媒体查询

12. ##### 盒模型定义

13. ##### linear-gradient线性渐变

    ```
    
    ```

    

14. ##### css3的换肤解决方案

    ```css
    使用基于sass\less的换肤方案，有一个缺陷。就是一个应用只有一个主题色，如果还想多扩展几个主题色的话，就会很麻烦。css3推出一种变量命名，绑定到根节点的方式。
    css3作用域
    // 作用域根节点
    :root{
        --theme-color:red;
    }
    // 作用域id='app'的dom节点下（包含）
    #app{
        --theme-color:red;
    }
    
    // 定义一个themeColor.js文件，配置所需的颜色值
    const themeColorList = {
        'red':{
            '--theme-color':'red',  // 同一个主题'red'下就有多个颜色值了them-color1,them-color2......
            '--theme-color1':'green',
        },
        'black':{
            '--theme-color':'black',
            '--theme-color1':'green',
        }
        ......
    }
    export default themeColorList;
    
    // 在入口文件中操作根节点，写入theme-color
    const currentTheme = window.document.documentElement.getAttribute('data-theme'); // 获取当前的主题色
    const temp = themeColorList[currentTheme]; // 从themeColor中匹配出当前主题色对应的颜色列表
    for(var theme in temp){
        // 写入到根节点,当然也可以使用到具体的dom节点下，因为css3具有作用域，凡是不在该作用域下的，都无法使用！！！
        document.documentElement.style.setProperty(theme,temp[theme]);
    }
    // 在具体界面使用时
    .userName{
        color:var(--theme-color);
    }
    // js使用方法：
    var styles = getComputedStyle(document.documentElement);
    var value = styles.getPropertyValue("--theme-color");
    console.log(value);
    
    对比sass\less方法和css3方法：
    其实一般应用主题色只有一个，无疑sass\less是最好的解决办法。但实际UI设计开发的时候，往往会存在多个颜色，所以扩展上讲css3可能更适合！再者使用sass\less方法，无法在js中获取主题色。我们只能通过预先设置好选择器，再使用js动态添加该选择器的方法，达到动态颜色变更。
    ```

    

15. 虚位以待！！！

