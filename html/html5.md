# HTML5的新特性

参考：https://www.cnblogs.com/binguo666/p/10928907.html

1. ##### 本地存储

   ```js
   1、cookie（不属于h5，在之前就已经有cookie）
   let userName = 'luoliang';
   document.cookie = "userName" + "=" + userName;
   1.1、cookie取值：
   document.cookie取出来的值是：userName=admin;password=1;pageSize=6 的字符串，所以要字符串拆分、筛选
   var cookies = document.cookie.split(";");
   for (var i = 0; i < cookies.length; i++) {
       var str = cookies[i].split("=");
       //console.log(str[0]);
       if (str[0] == "userName") { //选出字符为userName的字段
           console.log(str[1]);
       }
   }
   1.2、为cookie设置过期时间，时间一到cookie自动销毁（没有删除cookie的方法）
   var exp = new Date();
   exp.setTime(exp.getTime() + 60 * 2000);//过期时间2分钟 (getTime()得到的是毫秒)
   document.cookie = "userName" + "=" + userName + ";expires=" + exp.toGMTString(); //把时间转换为字符串(现在常用：toUTCString()方法)
   封装删除cookie的方法：
   function delCookie(name){
       var exp = new Date();
       exp.setTime(exp.getTime() - 1);
       var cval=getCookie(name);
       if(cval!=null){
           document.cookie= name + "="+cval+";expires="+exp.toGMTString();
       }
   }
   1.3、为cookie设置 域名访问权限
   问题描述：有个地址www.aaa.com一级域名 aaa.com 如果在该一级域名下，还有一个二级域名eg:www.bbb.aaa.com 这里的bbb就是二级域名，那么问题来了，设置在www.aaa.com下的cookie 在 www.bbb.aaa.com下可以访问，子域可以访问顶域的cookie。那假如设置在www.bbb.aaa.com的cookie想在 www.aaa.com下怎么访问？（顶域无法获取子域数据）
   解决办法：
   将子域的cookie设置为顶域
   eg: setCookie('token','.......',{domain:window.idomain}) // 这里的window.idomain 可以设置为aaa.com
   
   2、localStorage存储
   存值:window.localStorage.setItem(key,value)：将value存储到key字段
   取值:window.localStorage.getItem(key):获取指定key本地存储的值
   删除:window.localStorage.removeItem(key):删除指定key本地存储的值
   localStorage在存储单个key，value时，很方便，但在存储JSON对象时，应当先将JSON对象转换为JSON字符串
   var students = {
       liyang:{name:"liyang",age:17},
       lilei:{name:"lilei",age:18}
   } // 需要存储的JSON对象
   students = JSON.stringify(students);//将JSON对象转化成字符串
   localStorage.setItem("students",students);//用localStorage保存转化好的的字符串
   取值时：
   var students = localStorage.getItem("students");// 取students变量
   students = JSON.parse(students);//把字符串转换成JSON对象
   
   3、sessionStorage存储
   sessionStorage与localStorage方法相同，唯一不同的就是localStorage是本地存储，不删除的话就会常驻浏览器内存中。而sessionStorage是会话存储，当关闭浏览器是自动销毁！！！
   ```

2. ##### navigator对象：通常用于获取浏览器和操作系统的信息(详情查看w3cschool)

   ```js
   
   ```

3. ##### 语义标签

   ```html
   html5语义标签，可以使开发者更方便清晰构建页面的布局
   <header>	定义了文档的头部区域
   <footer> 	定义了文档的尾部区域 
   <nav> 	定义文档的导航 
   <section>	 定义文档中的节
   <article>	 定义文章
   <aside>	 定义页面以外的内容
   <details>	定义用户可以看到或者隐藏的额外细节
   <summary>	标签包含details元素的标题 
   <dialog>	定义对话框 
   <figure>	定义自包含内容，如图表
   <main>	定义文档主内容
   <mark>	定义文档的主内容
   <time>	定义日期/时间
   ```

4. ##### 增强型表单

   ```html
   1、html5修改一些新的input输入特性，改善更好的输入控制和验证!
   eg: <input type="color"/>
   color	主要用于选取颜色
   date	选取日期
   datetime	选取日期(UTC时间)
   datetime-local	选取日期（无时区）
   month	选择一个月份
   week	选择周和年
   time	选择一个时间
   email	包含e-mail地址的输入域
   number	数值的输入域
   url	url地址的输入域
   tel	定义输入电话号码和字段
   search	用于搜索域
   range	一个范围内数字值的输入域
   
   2、html5新增表单属性
   placehoder	输入框默认提示文字
   required	要求输入的内容是否可为空
   pattern	 描述一个正则表达式验证输入的值
   min/max	 设置元素最小/最大值
   step	 为输入域规定合法的数字间隔
   height/wdith	用于image类型<input>标签图像高度/宽度
   autofocus	规定在页面加载时，域自动获得焦点
   multiple	规定<input>元素中type=file时，可选择多个值。
   
   3、新增5个表单元素
   <datalist>	用户会在他们输入数据时看到域定义选项的下拉列表
   <progress>	进度条，展示连接/下载进度
   <meter>	刻度值，用于某些计量，例如温度、重量等
   <keygen>	提供一种验证用户的可靠方法，生成一个公钥和私钥
   <output>	用于不同类型的输出，比如尖酸或脚本输出
   ```

   

5. ##### 视频和音频

   ```html
   html5提供了音频和视频文件的标准，既使用<audio>和<video>元素
   1、音频：<audio src=" "></audio>
   <audio controls>    //controls属性提供添加播放、暂停和音量控件。
       <source src="horse.ogg" type="audio/ogg">
     	<source src="horse.mp3" type="audio/mpeg"> 
   	您的浏览器不支持 audio 元素。        //浏览器不支持时显示文字
   </audio>
       
   2、视频：<video src=" "></video>
   <video width="320" height="240" controls>
       <source src="movie.mp4" type="video/mp4">
       <source src="movie.ogg" type="video/ogg">
   	您的浏览器不支持Video标签。
   </video>
   ```

   

6. ##### Canvas绘图

   ```
   详情查看：js中的canvas部分
   ```

   

7. ##### SVG绘图

   ```
   什么是SVG?
     SVG指可伸缩矢量图形
     SVG用于定义用于网络的基于矢量的图形
     SVG使用XML格式定义图形
     SVG图像在放大或改变尺寸的情况下其图形质量不会有损失
     SVG是万维网联盟的标准
   SVG的优势?
   与其他图像格式相比，SVG的优势在于：
      SVG图像可通过文本编译器来创建和修改
      SVG图像可被搜索、索引、脚本化或压缩
      SVG是可伸缩的
      SVG图像可在任何的分辨率下被高质量的打印
      SVG可在图像质量不下降的情况下被放大
    
   SVG与Canvas区别？
   SVG适用于描述XML中的2D图形的语言，Canvas随时随地绘制2D图形（使用javaScript）
   SVG是基于XML的，意味这可以操作DOM，渲染速度较慢，在SVG中每个形状都被当做是一个对象，如果SVG发生改变，页面就会发生重绘。而Canvas是一像素一像素地渲染，如果改变某一个位置，整个画布会重绘。
   Canvas：
   1、依赖分辨率
   2、不支持事件处理器
   3、能够以.png或.jpg格式保存结果图像
   4、文字呈现功能比较简单
   5、最合适图像密集的游戏
   SVG：
   1、不依赖分辨率
   2、支持事件处理器
   3、复杂度会减慢搞渲染速度
   4、适合大型渲染区域的应用程序
   5、不适合游戏应用
   ```

   

8. ##### 地理定位

   ```
   使用getCurrentPosition()方法来获取用户的位置。以实现“LBS服务”
   <script>
       var x=document.getElementById("demo");
       function getLocation(){
           if (navigator.geolocation){
               navigator.geolocation.getCurrentPosition(showPosition);
           }
           else{
               x.innerHTML="Geolocation is not supported by this browser.";
           }
       }
       function showPosition(position){
           x.innerHTML="Latitude: " + position.coords.latitude + "<br />Longitude: " + position.coords.longitude;
       }
   </script>
   ```

   

9. ##### 拖放API

   ```html
   拖放是一种常见的特性，即捉取对象以后拖到另一个位置。在html5中，拖放是标准的一部分，任何元素都能够拖放。
   <div draggable="true"></div>
   
   当元素拖动时，我们可以检查其拖动的数据。
   <div draggable="true" ondragstart="drag(event)"></div>
   <script>
   function drap(ev){
       console.log(ev);
   }
   </script>
   支持事件监听：
   ondragstart	在拖动操作开始时执行脚本
   ondrag	只要脚本在被拖动就运行脚本
   ondragenter	当元素被拖动到一个合法的防止目标时，执行脚本
   ondragover	只要元素正在合法的防止目标上拖动时，就执行脚本
   ondragleave	当元素离开合法的防止目标时
   ondrop	将被拖动元素放在目标元素内时运行脚本
   ondragend	在拖动操作结束时运行脚本
   ```

   

10. ##### Web Worker

    ```html
    Web Worker可以通过加载一个脚本文件，进而创建一个独立工作的线程，在主线程之外运行。
    Web Worker的基本原理就是在当前javascript的主线程中，使用Worker类加载一个javascript文件来开辟一个新的线程，起到互不阻塞执行的效果，并且提供主线程和新县城之间数据交换的接口：postMessage、onmessage。
    <!DOCTYPE HTML>
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <script type="text/javascript">
    //WEB页主线程
    var worker = new Worker("worker.js"); //创建一个Worker对象并向它传递将在新线程中执行的脚本的URL
    worker.postMessage("hello world");     //向worker发送数据
    worker.onmessage =function(evt){     //接收worker传过来的数据函数
       console.log(evt.data);              //输出worker发送来的数据
    }
    </script>
    </head>
    <body></body>
    </html>
    
    //worker.js
    onmessage =function (evt){
    	var d = evt.data;//通过evt.data获得发送来的数据
    	postMessage( d );//将获取到的数据发送会主线程
    }
    ```

    

11. ##### WebSocket

    ```html
    WebSocket协议为web应用程序客户端和服务端之间提供了一种全双工通信机制。
    特点：
    （1）握手阶段采用HTTP协议，默认端口是80和443
    （2）建立在TCP协议基础之上，和http协议同属于应用层
    （3）可以发送文本，也可以发送二进制数据。
    （4）没有同源限制，客户端可以与任意服务器通信。
    （5）协议标识符是ws（如果加密，为wss），如ws://localhost:8023
    ```

    

12. 虚位以待！！！