# css制作图形

1. ##### 背景渐变效果

   ```css
   <div class="bg"></div>
   .bg{
       width: 500px;
       height: 100px;
   
       /*垂直方向渐变默认,由上及下*/
       background: linear-gradient(#000000,#FAFAFA);
   
       /*由左上及右下*/
       /*background:-webkit-linear-gradient(left top,#000000,#FAFAFA);*/
       /*background: -moz-linear-gradient(left top, #ace, #f96);*/
       /*background: -o-linear-gradient(left top, #ace, #f96);*/
   
   	/*从左及右，均分五部分，依次颜色渐变,可以不断叠加*/
       /*background: -webkit-linear-gradient(left, #ace, #f96, #ace, #f96, #ace);*/
       /*background: -moz-linear-gradient(left, #ace, #f96, #ace, #f96, #ace);*/
       /*background: -o-linear-gradient(left, #ace, #f96, #ace, #f96, #ace);*/
   
   	/*调节各部分的占比*/
       /*background: -webkit-linear-gradient(left, #ace, #f96 50%, #ace, #f96 50%, 		#ace);*/
       /*background: -moz-linear-gradient(left, #ace, #f96 5%, #ace, #f96 95%, #ace);*/
       /*background: -o-linear-gradient(left, #ace, #f96 5%, #ace, #f96 95%, #ace);*/
   }
   ```

2. ##### 椭圆形

   ```css
   <div class="oval"></div>
   .oval{
       width: 200px;height: 100px;background: #e9337c;
       -webkit-border-radius: 100px / 50px;
       -moz-border-radius: 100px / 50px;
       border-radius: 100px / 50px;
   }
   ```

3. ##### 三角形

   ```css
   <!--三角形箭头向上-->
   .triangle {
       width: 0;height: 0;
       border-bottom: 140px solid #fcf921;
       border-left: 70px solid transparent;
       border-right: 70px solid transparent;
   }
   
   <!--三角形箭头向右-->
   .triangle {
       width: 0;height: 0;
       border-top: 70px solid transparent;
       border-left: 140px solid #ff5a00;
       border-bottom: 70px solid transparent;
   }
   
   <!--三角形箭头向下-->
   .triangle {
       width: 0;height: 0;
       border-top: 140px solid #20a3bf;
       border-left: 70px solid transparent;
       border-right: 70px solid transparent;
   }
   
   <!--三角形箭头向左-->
   .triangle {
       width: 0;height: 0;
       border-top: 70px solid transparent;
       border-right: 140px solid #6bbf20;
       border-bottom: 70px solid transparent;
   }
   
   <!--三角形左上角-->
   .triangle {
       width: 0px;
       height: 0px;
       margin: 10px 10px;
       float: left;
       border-top: 100px solid #7efde1;
       border-right: 100px solid transparent;
   }
   
   <!--三角形右下角-->
   .triangle{
       width: 0px;
       height: 0px;
       margin: 10px 10px;
       float: left;
       border-bottom: 100px solid #7efde1;
       border-left: 100px solid transparent;
   }
   ```

4. ##### 菱形

   ```css
   .diamond {
       width: 120px;height: 120px;background: #1eff00;
       /* Rotate */
       -webkit-transform: rotate(-45deg);
       -moz-transform: rotate(-45deg);
       -ms-transform: rotate(-45deg);
       -o-transform: rotate(-45deg);
       transform: rotate(-45deg);
       /* Rotate Origin */
       -webkit-transform-origin: 0 100%;
       -moz-transform-origin: 0 100%;
       -ms-transform-origin: 0 100%;
       -o-transform-origin: 0 100%;
       transform-origin: 0 100%;
       margin: 60px 0 10px 310px;
   }
   ```

5. ##### 平行四边形

   ```css
   .parallelogram {
       margin-left: 100px;width: 160px;height: 100px;background: #8734f7;
       -webkit-transform: skew(30deg);
       -moz-transform: skew(30deg);
       -o-transform: skew(30deg);
       transform: skew(30deg);
   }
   ```

6. ##### 五边形

   ```css
   .pentagon {
       width: 89px;
       position: relative;
       border-width: 50px 18px 0;
       border-style: solid;
       border-color: #277bab transparent;
       margin: 20px 0 0 0;
   }
   
   .pentagon:before {
       content: "";
       height: 0;
       width: 0;
       position: absolute;
       top: -85px;
       left: -18px;
       border-width: 0 45px 35px;
       border-style: solid;
       border-color: transparent transparent #277bab;
   }
   ```

7. ##### 五角星

   ```css
   .star {
   	width: 0;
   	height: 0;
   	margin: 50px 0;
   	color: #fc2e5a;
   	position: relative;
   	display: block;
       border-right: 100px solid transparent;
       border-bottom: 70px solid #fc2e5a;
       border-left: 100px solid transparent;
       -moz-transform: rotate(35deg);
       -webkit-transform: rotate(35deg);
       -ms-transform: rotate(35deg);
       -o-transform: rotate(35deg);
   }
   
   .star:before {
   	content: '';
   	height: 0;
   	width: 0;
   	position: absolute;
   	display: block;
   	top: -45px;
   	left: -65px;
       border-bottom: 80px solid #fc2e5a;
       border-left: 30px solid transparent;
       border-right: 30px solid transparent;
       -webkit-transform: rotate(-35deg);
       -moz-transform: rotate(-35deg);
       -ms-transform: rotate(-35deg);
       -o-transform: rotate(-35deg);
   }
   
   .star:after {
       content: '';
       width: 0;
       height: 0;
       position: absolute;
       display: block;
       top: 3px;
       left: -105px;
       color: #fc2e5a;
       border-right: 100px solid transparent;
       border-bottom: 70px solid #fc2e5a;
       border-left: 100px solid transparent;
       -webkit-transform: rotate(-70deg);
       -moz-transform: rotate(-70deg);
       -ms-transform: rotate(-70deg);
       -o-transform: rotate(-70deg);
   }
   ```

8. ##### 六角星

   ```css
   .star_six {
   	width: 0;
   	height: 0;
   	display: block;
   	position: absolute;
   	margin: 10px auto;
       border-left: 50px solid transparent;
       border-right: 50px solid transparent;
       border-bottom: 100px solid #de34f7;
   }
   
   .star_six:after {
   	content: "";
   	width: 0;
   	height: 0;
   	position: absolute;
   	margin: 30px 0 0 -50px;
       border-left: 50px solid transparent;
       border-right: 50px solid transparent;
       border-top: 100px solid #de34f7;
   }
   ```

9. ##### 六边形

   ```css
   .six {
   	width: 100px;height: 55px;
       background: #fc5e5e;position: relative;margin: 10px;
   }
   
   .six:before {
   	content: "";width: 0;height: 0;
       position: absolute;top: -25px;left: 0;
       border-left: 50px solid transparent;
       border-right: 50px solid transparent;
       border-bottom: 25px solid #fc5e5e;
   }
   
   .six:after {
   	content: "";width: 0;height: 0;
       position: absolute;bottom: -25px;left: 0;
       border-left: 50px solid transparent;
       border-right: 50px solid transparent;
       border-top: 25px solid #fc5e5e;
   }
   ```

10. ##### 八边形

    ```css
    .eight {
    	width: 100px;height: 100px;background: #ac60ec;
        position: relative;margin：50px;
    }
    
    .eight:before {
    	content: "";width: 100px;height: 0;
        position: absolute;top: 0;left: 0;
        border-bottom: 29px solid #ac60ec;
        border-left: 29px solid #f4f4f4;
        border-right: 29px solid #f4f4f4;
    }
    
    .eight:after {
    	content: "";width: 100px;height: 0;
        position: absolute;bottom: 0;left: 0;
        border-top: 29px solid #ac60ec;
        border-left: 29px solid #f4f4f4;
        border-right: 29px solid #f4f4f4;
    }
    ```

11. ##### 心形

    ```css
    .heart {
    	position: relative;
    }
    
    .heart:before,.heart:after {
    	content: "";width: 70px;height: 115px;
        position: absolute;background: red;left: 70px;top: 0;
        -webkit-border-radius: 50px 50px 0 0;
        -moz-border-radius: 50px 50px 0 0;
        border-radius: 50px 50px 0 0;
        -webkit-transform: rotate(-45deg);
        -moz-transform: rotate(-45deg);
        -ms-transform: rotate(-45deg);
        -o-transform: rotate(-45deg);
        transform: rotate(-45deg);
        -webkit-transform-origin: 0 100%;
        -moz-transform-origin: 0 100%;
        -ms-transform-origin: 0 100%;
        -o-transform-origin: 0 100%;
        transform-origin: 0 100%;
    }
    
    .heart:after {
    	left: 0;
        -webkit-transform: rotate(45deg);
        -moz-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        -o-transform: rotate(45deg);
        transform: rotate(45deg);
        -webkit-transform-origin: 100% 100%;
        -moz-transform-origin: 100% 100%;
        -ms-transform-origin: 100% 100%;
        -o-transform-origin: 100% 100%;
        transform-origin: 100% 100%;
    }
    ```

12. ##### 吃人豆

    ```css
    .pacman {
        width: 0;height: 0;
        border-right: 70px solid transparent;
        border-top: 70px solid #ffde00;
        border-left: 70px solid #ffde00;
        border-bottom: 70px solid #ffde00;
        border-top-left-radius: 70px;
        border-top-right-radius: 70px;
        border-bottom-left-radius: 70px;
        border-bottom-right-radius: 70px;
    }
    ```

13. ##### 太极阴阳

    ```
    .yin-yang {
        width: 96px;
        height: 48px;
        background: #eee;
        border-color: red;
        border-style: solid;
        border-width: 2px 2px 50px 2px;
        border-radius: 100%;
        position: relative;
    }
    .yin-yang:before {
        content: "";
        position:
        absolute;
        top: 50%;
        left: 0;
        background: #eee;
        border: 18px solid red;
        border-radius: 100%;
        width: 12px;
        height: 12px;
    }
    .yin-yang:after {
        content: "";
        position: absolute;
        top: 50%;
        left: 50%;
        background: red;
        border: 18px solid #eee;
        border-radius:100%;
        width: 12px;
        height: 12px;
    }
    ```
    
    
    
14. ##### 四角边框，参考：https://blog.csdn.net/XreqcxoKiss/article/details/105236123

    ```
    原理：linear-gradient() 函数用于创建一个线性渐变的 “图像”。为了创建一个线性渐变，你需要设置一个起始点和一个方向（指定为一个角度）的渐变效果。你还要定义终止色。终止色就是你想让Gecko去平滑的过渡，并且你必须指定至少两种，当然也会可以指定更多的颜色去创建更复杂的渐变效果。
    
    <div class="div1"></div>
    <div class="div2"></div>
    .div1{
        position: absolute;
        top: 20px;
        left: 20px;
        width: 100px;
        height: 100px;
        background: linear-gradient(to left, #f00, #f00) left top no-repeat,
        linear-gradient(to bottom, #f00, #f00) left top no-repeat,
        linear-gradient(to left, #f00, #f00) right top no-repeat,
        linear-gradient(to bottom, #f00, #f00) right top no-repeat,
        linear-gradient(to left, #f00, #f00) left bottom no-repeat,
        linear-gradient(to bottom, #f00, #f00) left bottom no-repeat,
        linear-gradient(to left, #f00, #f00) right bottom no-repeat,
        linear-gradient(to left, #f00, #f00) right bottom no-repeat;
        background-size: 1px 20px, 20px 1px, 1px 20px, 20px 1px;
    }
    .div2{
        border: 1px red solid;
        position: absolute;
        top: 20px;
        left: 150px;
        width: 100px;
        height: 100px;
        background: linear-gradient(to left, #f00, #f00) left top no-repeat,
        linear-gradient(to bottom, #f00, #f00) left top no-repeat,
        linear-gradient(to left, #f00, #f00) right top no-repeat,
        linear-gradient(to bottom, #f00, #f00) right top no-repeat,
        linear-gradient(to left, #f00, #f00) left bottom no-repeat,
        linear-gradient(to bottom, #f00, #f00) left bottom no-repeat,
        linear-gradient(to left, #f00, #f00) right bottom no-repeat,
        linear-gradient(to left, #f00, #f00) right bottom no-repeat;
        background-size: 2px 20px, 20px 2px, 2px 20px, 20px 2px;
      }
    ```

    

15. 虚位以待！！！

    