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

6. ##### 前端安全策略

7. ##### 设计模式

   ```
   js常见的设计模式：https://www.cnblogs.com/imwtr/p/9451129.html
   ```

7. ##### eventloop事件循环，宏任务，微任务，执行上下文

   ```js
   EventLoop 事件循环
   Event Queue 事件队列
   Event Table 事件表
   macro-task 宏任务
   micro-task 微任务
   js执行时候，一个主线程里面都会有一个事件循环（消息循环|运行循环）和事件队列，存放各种要处理的事件信息，通过这个循环不断处理这些事件信息或消息。队列分为：宏任务和微任务
   
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
   3、然后遇到new Promise异步微任务，会立即执行promise函数console.log(2)，并将异步微任务的promise.then注册分发到微任务的事件队列中等待执行。
   4、执行同步任务 console.log(3)。至此第一轮事件轮询结束。
   5、第一轮事件轮询结束后，主线程开始检查异步任务，优先检查微任务的事件队列，发现promise.then并执行。console.log(4)
   6、微任务执行结束后，开始检查宏任务的事件队列，发现setTimeout并执行console.log(5)。
   
   再来看一个复杂点的面试题：https://www.lagou.com/lgeduarticle/91871.html
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

9. 虚位以待！