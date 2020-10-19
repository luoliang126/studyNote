# js其他知识、问题点

1. ##### js实现打印

   ```js
   调用h5的window.print方法可以实现打印网页内容功能，但这里有很多小问题？
   方法一：使用截取dom节点（需要打印的部分）
   1、首先截取需要打印的dom节点。
   2、然后将原来的body内容拷贝，备份。
   3、将截取的dom节点添加到body根节点
   4、调用window.print方法实现打印。
   5、打印完后（可能没打印，而是预览后取消），将备份的body内容，重新添加到body中，实现界面还原。
   
   方法二：使用@media print属性
   @media print{
       .noprint{
   	    display:none
       }
   }
   // 页面上把不需要打印的部分添加class="noprint",然后执行window.print()方法时，凡是加了noprint的都不在打印范围以内。
   
   方法三：创建一个新的iframe，然后将需要打印的部分填充到iframe中去，调用iframe.contentWindow.print实现打印。
   
   比较优缺点：
   方法一：截取dom打印需要将原来的body节点备份，打印完后在重新将备份内容返回给body，表面看没问题，实际在使用中（以vue为例），dom节点备份后，再返回填充后，原来的dom节点上绑定的 @click事件等，将会失效（具体原因暂未找到）。优化办法是：在执行window.print后刷新当前界面，这样就不用备份，以及还原了，但浏览器会自动刷新一次，体验上如果可以接受，就不存在太大问题。
   方法二：使用css3属性@media print可以实现，不打印的dom节点添加class=“noprint”，但是这种方法仅仅适用于dom节点很少，或者只有一个html界面的情况下，如果像大型的SPA引用，怎么给其他组件的dom标签添加noprint样式，就会变得很复杂，而且没有优化解决办法。
   方法三：使用iframe打印的弊端在于，如果我外层文档document引用了elementUI等组件样式，我再创建iframe，并将需要打印部分的dom节点，添加到iframe中去，但原本的elementUI样式不会添加到iframe中去，所以就会造成样式的不一致。如果是少量dom节点还好（可以自行修改样式），大量的css样式将会变得很复杂。所以优化办法是：我们需要打印部分的dom节点、css样式等都手动写（最好不要用UI组价库，样式也直接内嵌到dom节点上，而不是用一个css链接），这样的话我们创建的dom片段就是一个自带样式的dom片段，在添加到iframe打印的时候，将会很自然。
   总结：还是那句，没有最好的解决办法，只有最适合当前情况的解决办法。
   
   打印样式修改
   1、调整打印页边距
   @page{
   	margin:0; // 具体值已打印效果为准
   	margin:auto 0mm; （上下设置为自动居中，左右边距为0）
   }
   2、调整打印方向（横向/纵向）
   @page{
   	size：portrait;（纵向)
   	size：landscape;（横向)
   }
   更多时候，我们在调用window.print()的时候，更希望用js方法调整打印方向。
   原理：动态创建一个style样式表，在执行打印的时候将其append到需要打印的容器中（默认为body）
   let $changePrint = function (type) {
     let html = '';
     type = type ? type : 1;
     if(type=='1'){
       html='<style type="text/css" media="print">\n' + '  @page { size: landscape; }\n' + '</style>';
     }else{
       html='<style type="text/css" media="print">\n' + '  @page { size: portrait; }\n' + '</style>';
     }
     return html;
   };
   document.body.append($changePrint(1));// 将样式append到根节点，或者需要打印的容器中。
   ```

2. ##### 前端微服务 参考：https://zhuanlan.zhihu.com/p/96717332

   ```js
   1、使用 HTTP 服务器的路由来重定向多个应用，即路由分发
   2、在不同的框架之上设计通讯、加载机制，诸如Single-SPA 和 Mooa
   3、通过组合多个独立应用、组件来构建一个单体应用
   4、iFrame。使用 iFrame 及自定义消息传递机制
   5、使用纯 Web Components 构建应用
   ```

3. ##### js日期、时间对象

   ```js
   日期对象：
   var date = new Date; //获取到的是，标准时间   Fri Mar 24 2017 16:06:02 GMT+0800 (中国标准时间)
   var year = date.getFullYear(); //获取到具体年份 2017，注意：如果是date.getYear()获取到的是117，而不是2017；
   var month = date.getMonth()+1; //月份是从0开始计算，所以加1
   var day = date.getDate(); //获取今日
   var hour = date.getHours(); //获取当前小时
   var minute = date.getMinutes(); //获取当前分钟
   var second = date.getSeconds(); //获取当前秒
   var weekday = date.getDay(); //获取星期几 如：5  代表星期五
   var time = data.getTime(); //获取到的是以毫秒计算的数值（时间戳）
   var arr = ["星期日","星期一","星期二","星期三","星期四","星期五","星期六"]
   divEle.innerHTML = year + "年" + month + "月" + day + "日" + hour + "时" + minute + "分" + second + "秒" + arr[weekday];
   
   倒计时：
   var curtime = new Date;
   var endtime = new Date("2017,6,7")
   var lefttime = endtime.getTime() - curtime.getTime(); //　转换为时间戳进行　减法
   // 转换为毫秒1天 = 24h  1h = 60minutes 1minute = 60seconds 1second = 1000ms
   var day = Math.ceil(lefttime/(24*60*60*1000));　// 转化为 ‘天’
   var second = parseInt(lefttime%60)	//此处用取整，不用math.ceil(因为倒计时不用向上取整)
   var minute = parseInt(lefttime/60%60);	//转换为m
   var hour = parseInt(lefttime/(60*60)%24);	//对24取模是因为每天有24h，我们只需要余下的部分
   var date = parseInt(lefttime/(24*60*60));
   
   日期的越界：在处理像2月这种不知道最后一天是28，29的时候最有用
   var preMonthLastday = new Date(2017,5,0)  //6月的第0天，也就是上个月的最后一天，即2017年5月31日
   获取本月的最后一天也可以用：var currentMonthLastDay = new Date(2017,6,0); //下月的第0天
   
   js时间格式化format 参考：https://blog.csdn.net/vasilis_1/article/details/73649961
   // 对Date的扩展，将 Date 转化为指定格式的String
   // 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符，
   // 年(y)可以用 1-4 个占位符，
   // 毫秒(S)只能用 1 个占位符 (是 1-3 位的数字)
   Date.prototype.Format = function (fmt) {
       var o = {
           "M+": this.getMonth() + 1, // 月份
           "d+": this.getDate(), // 日
           "h+": this.getHours(), // 小时
           "m+": this.getMinutes(), // 分
           "s+": this.getSeconds(), // 秒
           "q+": Math.floor((this.getMonth() + 3) / 3), // 季度
           "S": this.getMilliseconds() // 毫秒
       };
       if (/(y+)/.test(fmt)){
           fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
   	}
       for (var k in o)
           if (new RegExp("(" + k + ")").test(fmt))
               fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
       		return fmt;
   }
   // 使用：
   new Date().Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423
   new Date().Format("yyyy-M-d h:m:s.S") ==> 2006-7-2 8:9:4.18
   
   ```

4. ##### js正则

   ```js
   js RegExp 对象(正则表达式)
   使用正则字面量方式（推荐使用方式）
   1、/^......$/方式
   校验登录名：只能输入5-20个以字母开头、可带数字、“_”、“.”的字串
   <input type="text" id="userName" placeholder="请输入用户名"/>
   var userNameVal = document.getElementById("userName").value;
   var reg = /^[a-zA-Z]{1}([a-zA-Z0-9]|[._]){4,19}$/
   reg.test(userNameVal); //返回 true或者false
   其中[a-zA-Z]{1} 表示第一个字符要求是字母(大小写均可)。
   ([a-zA-Z0-9]|[._]){4,19} 表示从第二位开始（因为它紧跟在上个表达式后面）的一个长度为4到19位的字符串，它要求是由大小写字母、数字或者特殊字符集[._]组成。
   var regNumber = /\d+/;  //验证0-9的任意数字最少出现1次，返回true/false。
   var regString = /[a-zA-Z]+/; //验证大小写26个字母任意字母最少出现1次。
   
   2、/....../ig 方式，i代表忽略大小写（如果不写，会区别字母大小写），g代表全局匹配（如果不写，只默认匹配出现的第一个）
   let a = 'abcd123'
   let reg = /b/g   //全局匹配 'b'
   let regRange = /[a-z]/  // 区间匹配(a~z)
   console.log(reg.test(a)) //true
   批注：
   1、使用正则表达字面量的效率 高于 使用构造函数方式。
   2、在使用 var specialKey = /^[a-zA-Z0-9]$/ 验证字母和数字，但是只有一个，两个以上要用+连接符，例如： var specialKey = /^[a-zA-Z0-9]+$/
   3、特殊字符串中 - 时，需要转义一次
   let regSpe = /^[\-._@]$/  //因为 - 代表区间（什么之间），所以需要使用 \ 转换一次
   
   3、使用new RegExp()构造函数
   let re = new RegExp("cat", "g");  //全局匹配cat
   let tmp = re.test("catastrophe");
   console.log(tmp);  //true
   批注：使用构造函数方式，效率低于正则表达字面量方式。
   
   常见的一些正则匹配
   1、验证手机号,固定电话
   var phoneReg = /^(13[0-9]{9})|(18[0-9]{9})|(14[0-9]{9})|(17[0-9]{9})|(15[0-9]{9})$/; //手机号
   var phoneReg = /^(\d3,4\d3,4|\d{3,4}-|\s)?\d{7,14}$/; //固定电话
   phoneReg.test(13666239422);
   2、验证邮箱
   var emailReg = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/;
   3、验证纯数字
   var number = /^[0-9]*$/;
   4、验证身份证
   let idReg = /^\d{15}|\d{}18$/
   更多参考：https://www.cnblogs.com/xinwusuo/p/5948908.html
   
   华为公司的验证密码方法，参考：http://www.cnblogs.com/mukekeheart/p/5635650.html
   1、密码长度:
       5 分: 小于等于4 个字符
       10 分: 5 到7 字符
       25 分: 大于等于8 个字符
   2、字母:
       0 分: 没有字母
       10 分: 全都是小（大）写字母
       20 分: 大小写混合字母
   3、数字:
       0 分: 没有数字
       10 分: 1 个数字
       20 分: 大于1 个数字
   4、符号:
       0 分: 没有符号
       10 分: 1 个符号
       25 分: 大于1 个符号
   5、奖励:
       2 分: 字母和数字
       3 分: 字母、数字和符号
       5 分: 大小写字母、数字和符号
       
   更多正则表达式匹配规则，参考：http://tool.oschina.net/uploads/apidocs/jquery/regexp.html
   ```

5. ##### js对象创建方式

   ```js
   1、工厂模式：创建对象交给一个工厂方法来实现，可以传递参数，但主要缺点是无法识别对象类型，因为创建对象都是使用Object的原生构造函数来完成的，即person instanceof Object，而不是createPerson，因为它根本就不是一个构造函数
   function createPerson(name, age, job) {
       var o = new Object();
       o.name = name;
       o.age = age;
       o.job = job;
       o.getName = function () {
           return this.name;
       }
       return o; // 使用return返回生成的对象实例
   }
   var person = createPerson('Jack', 19, 'SoftWare Engineer');
   
   2、构造函数模式：解决了构造函数指向问题，但在创建对象时，特别针对对象的属性指向函数时，会重复的创建函数实例。每次执行getName方法时都会创建函数实例。
   function Person(name,age,job){
       this.name = name;
       this.age = age;
       this.job = job;
       this.getName = function () {
           return this.name;
       }
   }
   var person1 = new Person('Jack', 19, 'SoftWare Engineer');
   var person2 = new Person('Liye', 23, 'Mechanical Engineer');
   
   3、原型模式：解决了构造函数中每次执行getName方法时都会创建函数实例问题。但不能传递初始化参数，这在一定程序带来不便；另外，最主要是当对象的属性是引用类型时，它的值是不变的，总是引用同一个外部对象，所有实例对该对象的操作都会其它实例。
   function Person(){}
   Person.prototype.name = 'Jack'; // 使用原型来添加属性
   Person.prototype.age = 29;
   Person.prototype.getName = function(){
       return this.name;
   }
   var person1 = new Person();
   alert(person1.getName());//Jack
   var person2 = new Person();
   alert(person1.getName === person2.getName);//true;共享一个原型对象的方法
   
   4、组合构造函数及原型模式（推荐使用方式）:既能初始化传参，也不会在执行getName方法时多次创建函数实例化，不变的属性写在构造函数内，方法写在原型链上
   function Person(name, age, job) {
       this.name = name;
       this.age = age;
       this.job = job;
       this.lessons = ['Math', 'Physics'];
   }
   Person.prototype = {
       constructor: Person, // 原型字面量方式会将对象的constructor变为Object，这里强制指回Person
       getName: function () {
           return this.name;
       }
   }
   var person1 = new Person('Jack', 19, 'SoftWare Engneer');
   person1.lessons.push('Biology');
   var person2 = new Person('Lily', 39, 'Mechanical Engneer');
   alert(person1.lessons);//Math,Physics,Biology
   alert(person2.lessons);//Math,Physics
   alert(person1.getName === person2.getName);//true,//共享原型中定义方法
   如果使用以下方式，那么person1的constructor还是指向Person，就没有必要再重新定义以下constructor的指向，但字面量方式简单直接方便查看;
   Person.prototype.getName = function(){
       return this.name;
   }
   ```

6. ##### js window对象

   ```js
   1、全屏操作：
   //全屏
   var de = document.documentElement;
   if (de.requestFullscreen) {
       de.requestFullscreen();
   } else if (de.mozRequestFullScreen) {
       de.mozRequestFullScreen();
   } else if (de.webkitRequestFullScreen) {
       de.webkitRequestFullScreen();
   }
   //退出全屏
   if (document.exitFullscreen) {
       document.exitFullscreen();
   }
   else if (document.webkitCancelFullScreen) {
       document.webkitCancelFullScreen();
   }
   else if (document.mozCancelFullScreen) {
       document.mozCancelFullScreen();
   }
   
   2、window.open() - 打开新窗口
   var myWin = window.open("url","窗口打开方式"，"参数字符串");
   url:打开窗口的网址或路径
   窗口打开方式：
   	_self:在当前窗口打开，类似window.location.href = "......";
       _blank:重新开一个窗口（此方法很多浏览器已经禁止弹出窗口）;
       _top:窗口在顶部打开;
   参数:  
       width:窗口的宽度；
       height：窗口的高度；
       top:窗口距离顶部的高度。
       left:窗口距离左边的宽度
       menubar:窗口是否有菜单栏。
       toolbar:窗口是否有工具栏。
       scrollbars:窗口是否有滚动条。
       status：窗口是否有状态栏。
   问题描述：在页面A中有链接点击用window.open打开一个弹窗页面B，页面B没有关闭，然后返回A页面再次点击那个链接，页面B的内容已经更新，但是不能自动回到最前，问题？
   解决思路：拿到window.open对象，
   var myWindow="";	//最好定为全局变量myWindow，或者放在function之外
   if (myWindow == "") {
       myWindow = window.open(url, "newWindow", "width=" + w + "px,height=" + h + "px,top=0, left=0, status=yes,toolbar=no,menubar=no,location=no,fullscreen=yes", true);
   } else {
       myWindow.close();
       myWindow = window.open(url, "newWindow", "width=" + w + "px,height=" + h + "px,top=0, left=0, status=yes,toolbar=no,menubar=no,location=no,fullscreen=yes", true);
       myWindow.focus();
   }
   
   3、window.close() - 关闭当前窗口
   
   4、window.moveTo() - 移动当前窗口:moveTo() 方法可把窗口的左上角移动到一个指定的坐标
   
   5、window.resizeTo() - 调整当前窗口的尺寸 window.onresize = function(){.......}
   
   6、window.frameElement 获取当前文档的宿主节点iframe元素
   
   7、窗口信息
   window.screen.width 返回当前屏幕宽度(分辨率值)
   window.screen.height 返回当前屏幕高度(分辨率值)
   window.screen.availWidth  可用的屏幕宽度(除去浏览器导航栏的宽度,但包含F12后调试界面的宽度)
   window.screen.availHeight 可用的屏幕高度(除去浏览器导航栏的高度)
   window.document.body.offsetHeight; 返回当前body的高度
   window.document.body.offsetWidth; 返回当前body的宽度
   const testEle = document.getElementById('test');
   testEle.offsetLeft / testEle.offsetTop 获取当前dom元素距离网页左/上的距离
   注意：此处可能获取到的是相对父元素的偏移距离（经测试使用环境不同，得到的值也不同，注意使用环境！）。解决办法
   let outMap = document.getElementById('outMap');
   let pos = outMap.getBoundingClientRect();
   console.log(pos.top); //width/height/top/right/bottom/lef,这里的偏移就是相对网页了
   Element.getBoundingClientRect()方法返回元素的大小及其相对于视口的位置。（有兼容性问题，慎用！）
   testEle.clientWidth / testEle.clientHeight 获取元素的宽高（包括padding的值，但不包括margin和border）
   
   8、history对象包含用户（在浏览器窗口中）访问过的 URL。可通过 window.history 属性对其进行访问
   length:返回浏览器历史列表中的 URL 数量。
   console.log(window.history.length) //window可以不用，直接history.length;
   back():	返回history 列表中的前一个 URL。
   	window.history.back() // 返回上一级
   forward():加载 history 列表中的下一个 URL，前提是已经访问过该地址。
   	window.history.forward()
   history.go(number/URL)：加载 history 列表中的某个具体页面。
       window.history.go(-2) //返回上上一级
       window.history.go("www.baidu.com") //返回访问过的百度
   
   9、 window.location 对象用于获得当前页面的地址 (URL),其中
   <p>location.hash 设置或返回从井号 (#) 开始的 URL（锚）。在angular中常用</p>
   <p>location.host 设置或返回主机名和当前 URL 的端口号。location:8081</p>
   <p>location.hostname 返回 web 主机的域名 location</p>
   <p>location.pathname 返回当前页面的路径和文件名部分</p>
   <p>location.port 返回 web 主机的端口 （80 或 443）</p>
   <p>location.protocol 返回所使用的 web 协议（http:// 或 https://）</p>
   <p>location.search 设置或返回从问号 (?) 开始的 URL（查询部分）界面通过get方式传参时使用。</p>
   <p>location.href 设置或返回完整的 URL。</p>
   <p>window.location.href= "http://www.xxxxxxxx.net" ; 直接跳转到指定路径且跳转后有后退功能</p>
   <p>window.location.replace("http://www.xxxxxxxx.net") ; 跳转后没有后退功能,即在window.history中不会产生记录</p>
   <p>window.location.reload( ); 刷新当前页面.</p>
   <p>window.location.assign("http://www.baidu.com") 类似于window.location.href = "http://www.baidu.com"</p>
   ```

7. ##### 锚点anchor的实现

   ```js
   1、利用A标签制作
   <!--a标签name属性-->
   <a href="#test1">跳转到一</a>
   <a href="#test2">跳转到二</a>
   <a href="#test3">跳转到三</a>
   <div style="width:100%;height:1100px;background:red;"></div>
   <a name="test1"></a>
   <div style="width:100%;height:1100px;background:blue;"></div>
   <a name="test2"></a>
   <div style="width:100%;height:1100px;background:green;"></div>
   <a name="test3"></a>
   <!--a标签 id属性-->
   <a href="#test1">跳转到一</a>
   <a href="#test2">跳转到二</a>
   <a href="#test3">跳转到三</a>
   <div style="width:100%;height:1100px;background:red;"></div>
   <div id="test1"></div>
   <div style="width:100%;height:1100px;background:blue;"></div>
   <div id="test2"></div>
   <div style="width:100%;height:1100px;background:green;"></div>
   <div id="test3"></div>
   
   2、js制作
   <span onclick="test1()">跳到第一个</span>
   <span onclick="test2()">跳到第二个</span>
   <span onclick="test3()">跳到第三个</span>
   <div id="test1" style="width:100%;height:1100px;background:red;"></div>
   <div id="test2" style="width:100%;height:1100px;background:blue;"></div>
   <div id="test3" style="width:100%;height:1100px;background:green;"></div>
   function test1(){
       document.getElementById('test1').scrollIntoView();
   }
   function test2(){
       document.getElementById('test2').scrollIntoView();
   }
   function test3(){
       document.getElementById('test3').scrollIntoView();
   }
   window.scrollTo(0, 0);
   window.document.body.scrollTop;
   ```

8. ##### 禁止粘贴网页内容

   ```js
   禁止页面内容被复制粘贴，参考：https://www.cnblogs.com/momo798/p/6797670.html
   1、css解决办法
   * {
       user-select: none;
   }
   *为全部内容，也可以指定内容区域
   注意：该方法是，用户无法选取页面内容
   
   2、js解决办法，可以选取内容，但是无法操作。
   // 禁止右键菜单
   document.oncontextmenu = function(){ return false; };
   // 禁止文字选择
   document.onselectstart = function(){ return false; };
   // 禁止复制
   document.oncopy = function(){ return false; };
   // 禁止剪切
   document.oncut = function(){ return false; };
   // 禁止粘贴
   document.onpaste = function(){ return false; };
   ```

9. 虚位以待！！！