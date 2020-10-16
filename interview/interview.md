# 面试常见问题

1. ##### this指向问题

   ```js
   var name = "The Window";
   var object = {
       name: "My Object",
       getNameFunc: function () {
           var that = this;
           return function () {
               return that.name;
           };
       }
   };
   console.log(object.getNameFunc()());
   //输出结果:My Object
   
   (function Fn(){
       var user = "My Object";
       console.log(this.user);
       console.log(this);
   })();
   //输出结果:undefined   window
   
   var a = 1, b = 2;
   a = [b,b=a][0];
   console.log(a, b);
   //输出结果:2，1
   分析：
   第一步：把a的值赋给了b。但是数组[b,1][0],索引的第一个b还是原来的内存地址。
   a = [b,1][0]
   第二步：取数组的第一个值。b还是原来的2，赋值给a,所以a=2
   a = b
   ```

2. #####  数组去重

   ```
   详见js algorithm部分
   ```

3. #####  数组排序

   ```
   详见js algorithm部分
   ```

4. ##### 浏览器兼容问题

   ```js
   详见browser中的htmlCompatible和jsCompatible部分
   ```

5. ##### 前端安全策略 https://www.cnblogs.com/zhanghaiyu-Jade/p/11148530.html

   ```js
   1、XSS攻击
   xss攻击，全称跨站脚本攻击，XSS是一种在web应用中的计算机安全漏洞，它允许恶意web用户将代码植入到提供给其它用户使用的页面中。
   DOM-based型：利用DOM本身的缺陷，进行攻击
   <img src="{{img.src}}">
   //返回的 img.src=/xxxx"  onerror=xxx"
   //这样放到img的src中就成了这样<img src="/xxx"  onerror=xxx">
   //src肯定会加载失败，然后执行了onerror中注入的恶意代码，达到攻击效果
   反射型：恶意代码并没有保存在目标网站，通过引诱用户点击一个链接到目标网站的恶意链接来实施攻击的。（循序善诱，一步一步诱导用户点击操作）
   存储型：前端页面中表单提交的数据存在恶意代码被保存到目标网站的服务器中，这种攻击具有较强的稳定性和持久性。
   优化解决方案：
   对用户的输入进行过滤，移除用户输入的Style节点、Script节点、Iframe节点。
   将重要的cookie 设置成http only  这样就不能通过js获取到该cookie了
   
   2、CSRF(跨站请求伪造)
   要完成一次CSRF攻击，被攻击的网站必须满足两个必要的条件：
   登录受信任网站A，并在本地生成Cookie。（如果用户没有登录网站A，那么网站C在诱导的时候，请求网站A的api接口时，会提示你登录）
   在不登出A的情况下，访问危险网站C（其实是利用了网站A的漏洞）。
   温馨提示一下，cookie保证了用户可以处于登录状态，但网站C其实拿不到 cookie。
   优化解决方案：
   使用token:服务器发送给客户端一个token，客户端提交表单时带上该token，服务器验证token是否有效，有效就允许访问，否则拒绝访问。
   Referer验证：Referer指的是页面请求来源，意思是，只接受本站的请求，服务器才做响应；如果不是，就拦截。
   
   3、SQL注入攻击
   在编写SQL语句时，如果直接将用户传入的数据作为参数，使用字符串拼接的方式插入到SQL查询中，那么攻击者可以通过注入其他语句来执行攻击操作，这些攻击包括可以通过SQL语句做的任何事：获取敏感数据、修改数据、删除数据库表等。
   优化解决方案：
   验证输入类型：比如某个视图函数接收整形id来查询，那么就在URL规则中限制URL变量为整型。
   参数化查询：
   转义特殊字符：比如引号、分号和横线等。使用参数化查询时，各种接口库会为我们做转义工作
   
   4、点击劫持(ClickJacking)
   点击劫持就是利用视觉欺骗用户将一个危险网站设置透明，然后在其上方设置一个按钮，当你点击这个按钮的时候，就触发底部透明的危险网站的事件，从而欺骗用户点击一个按钮或者输入一个值。或者是将一个网站通过iframe进来，通过透明度设置不可见，诱导用户点击可见的一个按钮触发事件达到自己的一个目的等。
   优化解决方案：
   设置http响应头x-Frame-Options：X-Frame-Options HTTP 响应头是用来给浏览器指示允许一个页面可否在 <frame>, <iframe>或者 <object> 中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌到别人的网站中去。
   X-Frame-Options 有三个值：
   deny：表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
   sameorigin：表示该页面可以在相同域名页面的 frame 中展示。
   ALLOW-FROM uri：表示该页面可以在指定来源的 frame 中展示。
   换一句话说，如果设置为deny，不光在别人的网站frame嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为sameorigin，那么页面就可以在同域名页面的frame中嵌套。
   ```

6. ##### 设计模式 https://www.cnblogs.com/imwtr/p/9451129.html

   ```js
   什么是设计模式？
   假设有一个空房间，我们要日复一日地往里面放一些东西。最简单的办法当然是把这些东西直接扔进去，但是时间久了，就会发现很难从这个房子里找到自己想要的东西，要调整某几样东西的位置也不容易。所以在房间里做一些柜子也许是个更好的选择，虽然柜子会增加我们的成本，但它可以在维护阶段为我们带来好处。使用这些柜子存放东西的规则，或许就是一种模式。
   
   1、单例模式：保证一个类仅有一个实例对象，并提供一个能访问它的全局访问点。（dialog弹窗唯一，上传组件唯一）。
   es5实现方式：
   var Single = (function() {
       var instance = null;
       function Single(name) {
           this.name = name;
       }
       return function(name){
           if (!instance) {
               instance = new Single(name);
           }
           return instance;
       };
   })();
   var oA = new Single('hi');
   var oB = new Single('hello');
   console.log(oA===oB);
   说明：在第一次调用构造函数时利用闭包存储一个instance实例（不会被回收机制回收），以后的调用直接返回该instance实例。
   es6实现方式：
   class Single {
       constructor(name) {
           this.name = name;
           this.instance = null;
       }
       // 注意：这里的static只能是静态类的访问方式，实例化new之后的对象是无法访问的。
       static getInstance(name) {
           if(!this.instance) {
               this.instance = new Singleton(name);
           }
           return this.instance;
       }
   }
   var oA = Single.getInstance('hi');
   var oB = Single.getInstance('hisd');
   console.log(oA===oB);
   说明：这里只能是调用Single类的方法，才能实现一个instance。当然你也可以使用new Single创建多个实例（但他们都无法访问getInstance这个方法）
   另一种实现方式：
   let Single = (function(){
       let instance = null;
       return class {
           constructor(name){
               if(instance){
                   return instance
               }
               this.name = name;
               return instance = this;
           }
       }
   })()
   let oA = new Single('luo');
   let oB = new Single('liang');
   console.log(oA === oB);
   说明：对比上面的方式，很明显这种方法就比较具体化了（没有oop编程的思路）。
   总结：在oop编程的时候，我们更想把一个类给抽离出来，而不是做定制化开发，如果一个类只解决一个问题的话，那就是面向过程/流程开发了。所以我们在封装Single类的时候，既要考虑到公用的new多个实例操作，也要考虑单例模式。所以es6的第一种方法无疑是最好的选择。
   
   2、策略模式：定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。（也可理解为：为了解决一个问题，创建了一系列的解决办法，至于采用哪一种根据判断结果确定，与switch原理类似）
   商场满100减5、满200减15、满300减30、满400减50
   var countPrice = {
       // 定义活动算法
       returnPrice: {
           full100: function (price) {
               return price - 5
           },
           full200: function (price) {
               return price - 15
           },
           full300: function (price) {
               return price - 30
           },
           full400: function (price) {
               return price - 50
           }
       },
       // 统一调用入口
       getPrice: function (type, money) {
           return this.returnPrice[type] ? this.returnPrice[type](money) : money;
       },
       // 增加规则
       addRule: function (type, discount) {
           this.returnPrice[type] = function (price) {
               return price - discount;
           }
       }
   }
   console.log(countPrice.getPrice('full300',399)) // 369
   假如：新增一个满500减100的方案
   countPrice.addRule('full500', 100);
   console.log(countPrice.getPrice('full500',599))
   使用场景：表单验证，正则验证规则(手机号，密码)等。
   
   3、发布-订阅模式：又叫观察者模式（Observer Pattern），它定义了一种一对多的关系，让多个订阅者对象同时监听某一个发布者，或者叫主题对象，这个主题对象的状态发生变化时就会通知所有订阅自己的订阅者对象，使得它们能够自动更新自己。
   举例说明：
   当我们去adadis买鞋，发现看中的款式已经售罄了，售货员告诉你不久后这个款式会进货，到时候打电话通知你。于是你留了个电话，离开了商场，当下周某个时候adadis进货了，售货员拿出小本本，给所有关注这个款式的人打电话。这就是发布订阅模式。
   发布订阅模式的特点：
   3.1、买家（订阅者）只要声明对消息的一次订阅，就可以在未来的某个时候接受来自售货员（发布者）的消息，不用一直轮询消息的变化。
   3.2、售货员（发布者）持有一个小本本（订阅者列表），对这个本本上记录的订阅者的情况并不关心，只需要在消息发生时挨个去通知小本本上的订阅者，当订阅者增加或减少时，只需要在小本本上增删记录即可。
   3.3将上面的逻辑升级一下，售货员也可以有多个小本本，当不同款式的鞋进货了，发布者可以按照不同的小本本分别去通知订阅了不同类型消息的订阅者，这里有个消息类型的概念；
   参考：https://www.cnblogs.com/Nyan-Workflow-FC/p/13026629.html
   
   发布-订阅模式和观察者模式的区别：https://www.jianshu.com/p/a75b7917b850
   
   4、迭代器模式
   
   5、代理模式
   
   6、命令模式
   
   7、组合模式
   
   8、模板方法模式
   
   9、享元模式
   
   10、职责链模式
   
   11、中介者模式
   
   12、装饰者模式
   
   13、状态模式
   
   14、适配器模式
   
   15、外观模式
   ```

7. ##### eventloop事件循环，宏任务，微任务，执行上下文

   ```js
   EventLoop 事件循环
   Event Queue 事件队列
   Event Table 事件表
   macro-task 宏任务
   micro-task 微任务
   js执行时候，一个主线程里面都会有一个事件循环（消息循环|运行循环）和事件队列，存放各种要处理的事件信息，通过这个循环不断处理这些事件信息或消息。队列分为：宏任务队列和微任务队列
   
   js是一种单线程语言，顾名思义同一时间只能执行一个任务，代码是同步执行并且会形成阻塞。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推！
   优点：实现起来比较简单，执行环境相对单纯。
   缺点：只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。
   
   为了解决这个问题，js将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。
   js的异步函数：setTimeout，callback(回调不一定会形成异步，它只是可以使用异步，例如ajax的回调)，事件监听(on、bind、listen、addEventListener、observe)，发布/订阅(观察者模式)，promise。
   
   callback：回调函数是异步编程最基本的方法，其优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合（Coupling），流程会很混乱，而且每个任务只能指定一个回调函数。
   事件监听：任务的执行不取决代码的顺序，而取决于某一个事件是否发生。这种方法的优点：比较容易理解，可以绑定多个事件，每一个事件可以指定多个回调函数，而且可以去耦合，有利于实现模块化。这种方法的缺点：整个程序都要变成事件驱动型，运行流程会变得不清晰。
   发布/订阅：我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。这种方法的性质与"事件监听"类似，但是明显优于它。因为我们可以通过查看"消息中心"，了解存在多少信号、每个信号有多少订阅者，从而监控程序的运行。
   promise对象：async/await
   
   来看一个简单的例子：
   console.log(1)
   setTimeout(() => console.log(5), 0)
   new Promise((resolve, reject) => {
       console.log(2)
       resolve()
   }).then(() => {
       console.log(4)
   })
   console.log(3)
   1、整个js就是一个宏任务，主线程开始执行任务。
   2、先执行同步任务console.log(1)，然后遇到setTimeout异步的宏任务，把setTimeout注册到event table并分发到宏任务的事件队列中等待执行。
   3、然后遇到new Promise异步微任务，会立即执行promise函数console.log(2)以及resolve()函数，并将异步微任务的promise.then注册分发到微任务的事件队列中等待执行。
   4、执行同步任务 console.log(3)。至此第一轮事件轮询结束。
   5、第一轮事件轮询结束后，主线程开始检查异步任务，优先检查微任务的事件队列，发现promise.then并执行。console.log(4)
   6、微任务执行结束后，开始检查宏任务的事件队列，发现setTimeout并执行console.log(5)。
   
   再来看一个复杂点的情况：https://www.lagou.com/lgeduarticle/91871.html
   console.log(1);
   setTimeout(() => {
     console.log(2);
     Promise.resolve().then(() => {
       console.log(3)
     });
   });
   new Promise((resolve, reject) => {
     console.log(4)
     resolve(5)
   }).then((data) => {
     console.log(data);
     Promise.resolve().then(() => {
       console.log(6)
     }).then(() => {
       console.log(7)
       setTimeout(() => {
         console.log(8)
       }, 0);
     });
   })
   setTimeout(() => {
     console.log(9);
   })
   console.log(10);
    // 正确结果：1、4、10、5、6、7、2、3、9、8
   1、主线程开始执行任务console.log(1)。
   2、遇到setTimeout异步宏任务，放入宏任务队列等待执行（为了方便理解标记为：setTimeout1）。
   3、遇到promise异步微任务，先执行console.log(4)，并执行resolve(5)，注意：这里不会马上console.log(5)，因为then中的回调函数会放到微任务队列中，而不是马上执行console.log(data)。并将then回调函数放入微任务队列，等待执行（标记：promise,then1）！
   4、又遇到setTimeout异步宏任务，再次放入宏任务队列中等待执行（标记：setTimeout2）。
   5、执行console.log(10)。至此第一轮事件轮询结束。
   6、第一轮事件轮询结束后，主程序就开始检查微任务队列，发现promise.then1，执行resolve(5),即then后console.log(data)--》console.log(5)，又发现一个异步微任务promise.resolve.then，放入微任务队列（标记promise.then2）。至此微任务队列第一次执行完成。
   7、第一次微任务结束后，主程序继续检查微任务队列，发现promise.then2，执行console.log(6),console.log(7)，这两个都是在then中，都会执行。又发现异步宏任务setTimeout，放入宏任务队列等待执行(标记：setTimeout3)。至此微任务队列执行完成。
   8、主程序继续检查微任务队列（此时已执行完了），发现没有微任务队列了，才开始检查宏任务队列。
   9、宏任务队列，第一个就是setTimeout1，执行setTimeout1，console.log(2)。发现异步微任务Promise.resolve().then，放入微任务队列中等待执行（标记：promise.then3）。第三轮结束！
   10、主程序继续检查微任务队列，发现一个promise.then3，优先执行console.log(3)。至此微任务队列执行完了。
   11、主程序继续检查微任务队列（已执行完），执行宏任务队列，又发现一个setTimeout2，执行console.log(9)。
   12、主程序继续检查微任务队列（已执行完），执行宏任务队列，发现setTimeout3，执行console.log(8)。至此宏任务队列执行完成。
   13、js执行完成！
   
   当一个任务块代码执行时，遇到同步任务，就会进入主线程，遇到异步任务会进入Event Table中注册函数，之后将函数移入到 Event Queue。主线程内任务执行完毕，会去Event Queue 读取异步函数，按照顺序执行异步函数。待这个代码块的同步任务执行完，Event Table会执行Event Queue中的异步任务，也是按照顺序执行的。
   
   栈（堆栈）：栈是一种后进先出的数据结构，也就是说最新添加的项最早被移出；它是一种运算受限的线性表，只能在表头/栈顶进行插入和删除操作。例如：JS数组方法可以实现类似入栈出栈功能，入栈push()、出栈pop()
   队列：队列是一种先进先出的数据结构。队列在列表的末端增加项，在首端移除项。例如：JS数组提供了方法可以实现队列功能，入队unshift()、出队pop()
   js主程序执行，其实也是一个宏任务（单纯的说微任务执行顺序大于宏任务，是错误的！）。同一次事件循环中微任务永远在宏任务之前执行。因为每次事件循环都是先从微任务开始，只有当微任务执行完之后，才会执行宏任务。
   
   执行上下文：当js引擎在执行js代码时，会产生一个对应的执行环境，在这个环境中，所有变量会被事先提出来（变量提升），有的直接赋值，有的为默认值 undefined，代码从上往下开始执行，这就叫做执行上下文。
   
   js执行上下文具有以下特点：
   1.单线程，在主进程上运行
   2.同步执行，从上往下按顺序执行
   3.全局上下文只有一个，浏览器关闭时会被弹出栈
   4.函数的执行上下文没有数目限制
   5.函数每被调用一次，都会产生一个新的执行上下文环境
   
   js里有三种运行环境：
   1.全局环境：代码首先进入的环境。
   2.函数环境：函数被调用时执行的环境。
   3.eval函数：https://www.cnblogs.com/chaoguo1234/p/5384745.html（不常用）
   
   执行上下文的生命周期
   1.创建阶段
       (1).生成变量对象
       (2).建立作用域链
       (3).确定this指向
   2.执行阶段
       (1).变量赋值
       (2).函数引用
       (3).执行其他代码
   3.销毁阶段
   	执行完毕出栈，等待回收被销毁。
   ```

8. ##### 白屏问题

   ```js
   白屏主要是因为页面渲染阻塞导致的，导致的原因有：
   1：css文件加载需要一定的时间，在加载的过程中页面是空白的。
   解决：将css代码前置或者使用<style>书写，不建议直接使用嵌入html形式！
   2.可能是等待异步加载数据再渲染页面导致白屏,数据量大加载慢，导致数据没请求到阻塞页面渲染
   解决：在显示的首屏时同步渲染页面，后续的数据在页面滚动（滑屏时）时再采取异步请求渲染页面。
   3.首屏JS的执行会阻塞页面的渲染
   解决：尽量不要再首屏index.html代码中放置内联脚本。
   4、使用骨架屏技术
   ```

9. ##### 数据结构

   ```js
   详看：js中的dataStructure部分
   ```

10. ##### 算法

    ```js
    详看：js中的algorithm部分
    ```

11. ##### js中防抖、节流的方法

    ```js
    防抖节流：在进行窗口的resize、scroll、输入框内容校验、输入查询等操作时，如果事件处理函数调用的频率无限制（例如调用接口，增加服务器端压力），也会加重浏览器的负担，导致用户体验非常糟糕，而且也没有必要将浏览器的性能花费在这些地方，为了解决以上问题，便提出了防抖和节流。
    
    防抖例子：
    function debounce(fn, wait) {    
        var timeout = null;    
        return function() {        
            if(timeout !== null)  clearTimeout(timeout);        
            timeout = setTimeout(fn, wait);    
        }
    }
    function handle() {    
        console.log(Math.random()); 
    }
    window.addEventListener('scroll', debounce(handle, 1000));
    
    节流例子：
    //时间戳方式
    var throttle = function(func, delay) {            
    　　var prev = Date.now();            
    　　return function() {                
    　　　　var context = this;                
    　　　　var args = arguments;                
    　　　　var now = Date.now();                
    　　　　if (now - prev >= delay) {                    
    　　　　　　func.apply(context, args);                    
    　　　　　　prev = Date.now();                
    　　　　}            
    　　}        
    }        
    function handle() {            
    　　console.log(Math.random());        
    }        
    window.addEventListener('scroll', throttle(handle, 1000));
    // 定时器方式
    var throttle = function(func, delay) {            
        var timer = null;            
        return function() {                
            var context = this;               
            var args = arguments;                
            if (!timer) {                    
                timer = setTimeout(function() {                        
                    func.apply(context, args);                        
                    timer = null;                    
                }, delay);                
            }            
        }        
    }        
    function handle() {            
        console.log(Math.random());        
    }        
    window.addEventListener('scroll', throttle(handle, 1000));
    
    防抖：将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。
    节流：使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。
    总结： 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。 比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次Ajax请求，而不是在用户停下滚动页面操作时才去请求数据。这样的场景，就适合用节流技术来实现。
    ```

12. ##### 闭包

    ```js
    闭包：闭包就是能够读取其他函数内部变量的函数，由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
    
    闭包的形成：
    function outer(){
        var localVal = 1;
        return localVal;
    }
    outer(); // 1 
    这是一个一般的函数，每执行一次outer函数时，都会在outer作用域下var localVal = 1，当执行完成后，因为localVal没有被再使用，所以就会被垃圾回收而清除；下一次执行时再创建var localVal = 1，依次类推。
    function outer(){
        var localVal = 1;
        return function(){
            return localVal;
        }
    }
    var func = outer();
    func(); //1 
    这里形成了闭包，当定义var func = outer()时，执行outer函数，var localVal = 1并return一个function(){return localVal},但并未执行该function，重点来了，该function中会用到localVal，所以localVal不会被释放，而是常驻内存中，这样闭包形成了。
    
    闭包的作用：
    1、可以读取函数内部的变量
    function f1(){
        var n=999;
        function f2(){
            alert(n);
        }
        return f2;
    }
    var result=f1(); 
    // 赋值方法,如果是var result = f1; 那么只是给它一个方法的引用，并未执行该方法，便不能形成闭包,在执行result()()时相当于重新执行了一次f1方法而已，那里面的变量n就会被重置;
    result(); // 999
    
    2、让变量的值始终保持在内存中
    function f1(){
        var n=999;
        nAdd = function(){
            n+=1
        }
        function f2(){
            alert(n);
        }
        return f2;
    }
    var result=f1();
    result(); // 999
    nAdd();
    result(); // 1000
    nAdd();
    result(); // 1001
    说明：
    2.1、f1函数中的nAdd方法，因为没有用var nAdd，所以定义的nAdd为全局变量，而不是函数内的局部变量，虽然它存在函数内。可以在函数外部对函数内部的局部变量进行操作nAdd()直接使用。
    2.2、在var result = f1();时，此时就已经执行了f1函数，从而f2方法暴露在了全局作用域下。注意重点来了，重要事情说三遍，f1中var n = 999,因为在函数f2中会使用到n所以在执行完函数f1后，n不会被垃圾回收，而是常驻内存，这也就是闭包的关键。
    2.3、第一次执行result()时，就相当于f1()();那么执行的是return出来的f2函数，所以alert(n) //999，但内存中的n并没有被消除
    2.4、第一次nAdd()时，此时n+=1；使用的n是活动变量n（未被清除，常驻内存中的n），最终n变为1000，但依然没有被清除掉。
    2.5、第二次执行result()时，就相当于f1()();执行f2函数，但使用的n是内存中的n=1000;如果再执行f1()函数时，依然会使用n=1000；
    2.6、第二次nAdd()时，此时n+=1；使用的n依然是活动变量n（未被清除，常驻内存中的n），最终n变为1001，但依然没有被清除掉。
    
    闭包误区：
    var name = "The Window";
    var object = {
        name : "My Object",
        getNameFunc : function(){
            return function(){
                return this.name;
            };
        }
    };
    alert(object.getNameFunc()()); //The Window
    
    var name = "The Window";
    var object = {
        name : "My Object",
        getNameFunc : function(){
            var that = this;
            return function(){
                return that.name;
            };
        }
    };
    alert(object.getNameFunc()()); // My object
    
    闭包的注意事项:
    1、由于闭包会使得函数中的变量都被保存在内存中，滥用闭包的话，内存消耗很大，会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
    2、闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。
    ```

13. 虚位以待！