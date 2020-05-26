# es6语法笔记

参考：ECMAScript 6 入门 --阮一峰著 http://es6.ruanyifeng.com/

1. ##### let 声明变量使用

   ```js
   1、es5之前声明一个变量用var，无论这个变量声明在哪？（花括号内，function内等）它都是一个全局的变量（变量提升），只是在执行上下文的时候，它才会被赋值。所以会造成一种现象，没有赋值过的变量，可以先使用。从严谨的方向说，显然不合理！所以引入了let声明。
   使用var
   console.log(foo); // 输出undefined（变量被提升了）
   var foo = 2;
   使用let
   console.log(bar); // 报错ReferenceError
   let bar = 2;
   
   2、作用域的限制
   {
       var a = 10;
       let b = 20;
   }
   console.log(a) // 10
   console.log(b) // ReferenceError: a is not defined.
   
   3、暂时性死区：只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）在这个区域，不再受外部的影响。
   var tmp = 123;
   if (true) {
       tmp = 'abc'; 
       // ReferenceError （此处抛错是因为，下一句let tmp，在该if作用域内定义前使用了变量tmp，即便有一个全局的tmp）
       let tmp;
   }
   
   4、不允许重复申明，在同一块级作用域下，不允许使用let重复申明一个变量，跨作用域可以。
   错误方法
   function test(){
       let a = 1;
       let a = 2; //抛错
   }
   
   正确方法
   let a = 1;
   function test(){
       let a = 2;
   }
   ```

2. ##### const的使用

   ```js
   const：申明一个只读的常量，一旦申明，常量的值就没法改变（只是引用地址不能改变，但可以往里面添加/删除数据，随后会详细解释）
   const的本质是：保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。
   1、不能修改引用地址的情况
   const a = 1;
   a = 2;  //抛错 Assignment to constant variable.
   
   2、修改引用地址中的值，但未修改引用地址时，正确
   const a = [];
   a.push(1);    //往数组中添加数据（可行），没有修改的值（引用地址未发生改变）
   console.log(a); //a=[1]
   a = [1];  //抛错，因为修改了引用地址
   
   3、const的作用域和let的使用方法相同
   ```

3. ##### 变量声明与赋值

   ```js
   1、单个变量赋值 
   let a = 1;
   let b = 2;
   let c = 3;
   
   2、ES6语法(对号入座)
   let [a,b,c] = [1,2,3];  //和上面得到的一样效果
   
   3、逗号分隔
   let a = 1,b = 2, c = 3;
   ```

4. ##### 函数的新方法

   ```js
   1、ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
   function log(x, y = 'World') {
       console.log(x, y);
   }
   log('Hello') // Hello World
   log('Hello', 'China') // Hello China
   log('Hello', '') // Hello
   
   2、rest 参数（形式为...变量名）,用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。
   function add(...values) {
       console.log(arguments); //arguments和values，都包含了参数数组[2,5,3]，不同的是values更加简洁
       console.log(values);
       let sum = 0;
       for (var val of values) {
           sum += val;
       }
       return sum;
   }
   add(2, 5, 3) // 10
   ```

5. ##### Class类

   ```js
   1、class的基本格式
   定义一个Person类
   class Person {
       constructor({name,age,sex}){
           this.name = name;
           this.age = age;
           this.sex = sex;
       }
       toSay(){
           console.log(this.name);
       }
       async toChangeName({name}){
           this.name = name;
       }
   }
   使用
   var luoliang = new Person({name:'luoliang',age:29,sex:'男'});
   
   转换成es5语法：
   类Person就是构造函数function Person{、、、}。
   toSay等方法，就类似绑定在原型prototype上的方法
   Person.prototype = {
       constructor() {
   
       },
       toSay(){
   
       },
       async toChangeName(){
   
       }
   }
   
   2、class的表达式方法
   const MyClass = class Me { //此时的Me可以省略
       getClassName() {
           return Me.name;
       }
   };
   类似命名函数，此时的Me其实没有任何含义
   let inst = new MyClass();
   inst.getClassName() // Me
   Me.name // ReferenceError: Me is not defined
   
   立即执行的class
   let person = new class {
       constructor(name) {
           this.name = name;
       }
       sayName() {
           console.log(this.name);
       }
   }('张三'); // 自定义传递一个张三进去，constructor方法中的name参数就是张三
   person.sayName(); // "张三"
   
   3、Class 的取值函数（getter）和存值函数（setter）
   在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。
   class MyClass {
       constructor() {
   
       }
       get prop() {
           return this.name;
       }
       set prop(value) {
           console.log('setter:'+value);
       }
   }
   let inst = new MyClass();
   inst.prop = 123; // 拦截存储行为，执行set prop(value){...}方法，并打印setter: 123
   inst.prop //'getter'
   
   4、class的静态方法。所有定义在类class中的方法，都会被new之后的实例所继承
   class Foo {
   	classMethod() {
           return 'hello';
       }
   }
   let fooTest = new Foo();
   fooTest.classMethod(); // hello
   但是有时候我们又不想让实例用到该构造函数的方法？
   class Foo {
       //声明一个静态方法，实例化无法调用！
       static classMethod() {
           return 'hello';
       }
   }
   let fooTest = new Foo();
   fooTest.classMethod(); // TypeError: foo.classMethod is not a function
   
   5、class继承 extends
   // 父类Foo
   class Foo {
       static classMethod() {
           return 'hello';
       }
   }
   //子类Bar继承Foo
   class Bar extends Foo {
   	
   }
   Bar.classMethod() //'hello'
   注：extends继承，也包括父类的static方法继承。但是实例还是无法拿到static方法。
   ```

6. ##### Promise的用法。参考: https://blog.csdn.net/shan1991fei/article/details/78966297

   ```js
   1、什么是promise？
   简单来讲，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数。
   function runAsync(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作(这里用setTimeout模拟ajax)
           setTimeout(function(){
               console.log('执行完成');
               resolve('随便什么数据');
           }, 2000);
       });
       return p;
   }
   runAsync().then(function(data){
       //data就是resolve中传递过来的参数
       console.log(data);
   });
   //上面的方法，用callback回调的办法依然可以解决异步执行方法，但promise的强大之处在于可以链式调用。
   
   2、promise链式调用
   function runAsync1(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务1执行完成');
               resolve('随便什么数据1');
           }, 1000);
       });
       return p;
   }
   function runAsync2(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务2执行完成');
               resolve('随便什么数据2');
           }, 2000);
       });
       return p;
   }
   function runAsync3(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务3执行完成');
               resolve('随便什么数据3');
           }, 2000);
       });
       return p;
   }
   runAsync1()
   .then(function(data){
       console.log(data);
       return runAsync2();
   })
   .then(function(data){
       console.log(data);
       return runAsync3();
   })
   .then(function(data){
       console.log(data);
   });
   异步任务1执行完成
   随便什么数据1
   异步任务2执行完成
   随便什么数据2
   异步任务3执行完成
   随便什么数据3
   
   3、promise的 reject 方法。可以理解为错误的回调方法
   function getNumber(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               var num = Math.ceil(Math.random()*10); //生成1-10的随机数
               if(num<=5){
                   resolve(num);
               }
               else{
                   reject('数字太大了');
               }
           }, 2000);
       });
       return p;
   }
   getNumber()
   .then(
       function(data){
           console.log('resolved');
           console.log(data);
       },
       function(reason, data){
           console.log('rejected');
           console.log(reason);
       }
   );
   注意：这里then的回调方法有两个，第一个为resolve的回调，第二个为reject的回调
   
   4、promise的catch方法。跟promise的reject回调方法类似，接收抛错。（推荐使用这种方式，接受reject操作，因为catch后，不会抛错js依然可以执行，不会阻塞后面的进程！这点和try...catch类似）
   function getNumber(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               var num = Math.ceil(Math.random()*10); //生成1-10的随机数
               if(num<=5){
                   resolve(num);
               }
               else{
                   reject('数字太大了');
               }
           }, 2000);
       });
       return p;
   }
   getNumber()
   .then(function(data){
       console.log('resolved');
       console.log(data);
   })
   .catch(function(reason){
       console.log('rejected');
       console.log(reason);
   });
   
   5、promise的all方法。（all接受一个数组，需要等待数组中所有的方法执行完成后，再执行then函数的回调。注意与race对比）
   function runAsync1(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务1执行完成');
               resolve('随便什么数据1');
           }, 1000);
       });
       return p;
   }
   function runAsync2(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务2执行完成');
               resolve('随便什么数据2');
           }, 2000);
       });
       return p;
   }
   function runAsync3(){
       var p = new Promise(function(resolve, reject){
           //做一些异步操作
           setTimeout(function(){
               console.log('异步任务3执行完成');
               resolve('随便什么数据3');
           }, 2000);
       });
       return p;
   }
   Promise
   .all([runAsync1(), runAsync2(), runAsync3()])
   .then(function(results){
       console.log(results);
   });
   这里先打印
   异步任务1执行完成
   异步任务2执行完成
   异步任务3执行完成
   最后再返回
   ["随便什么数据1", "随便什么数据2", "随便什么数据3"]
   
   6、promise的race方法。（和all方法不同点，数组中谁先执行完，谁就先返回并调用then方法。所以then方法中只可能有数组函数中一个的返回值，但是数组中的函数都会执行）
   Promise
   .all([runAsync1(), runAsync2(), runAsync3()])
   .then(function(results){
       console.log(results);
   });
   异步任务1执行完成
   随便什么数据1
   异步任务2执行完成
   异步任务3执行完成
   注意：这里  随便什么数据2，随便什么数据3都返回了，但是then函数已经执行了，所以只会打印一次 随便什么数据1。
   ```

7. ##### 关于回调函数，this的指向问题！参考：https://www.cnblogs.com/jiajialove/p/10779655.html

   ```js
   问题描述点：回调函数在js中经常会用到，但是在执行回调函数的时候，this不是指向当前定义回调函数的this
   class Person{
       person1 = {
           name:'luoliang',
           age:31
       }
       mainFunction = function(callback){
           callback();
       }
       child1Function = function(){
           console.log(this);
       }
   }
   let person1 = new Person();
   person1.mainFunction(person1.child1Function); 
   //你以为这里console的this，是Class Person这个对象吗？ 结果它是undefined！
   解决办法：
   1、使用箭头函数，箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
   class Person{
       person1 = {
           name:'luoliang',
           age:31
       }
       mainFunction = function(callback){
           callback();
       }
       child1Function = () =>{
           console.log(this);
       }
   }
   let person1 = new Person();
   person1.mainFunction(person1.child1Function); 
   // 这时的this，才是Person这个类对象
   
   2、使用bind()方法，修改this指向。
   class Person{
       person1 = {
           name:'luoliang',
           age:31
       }
       mainFunction = function(callback){
           callback();
       }
       child1Function = function(){
           console.log(this);
       }.bind(this);
   }
   let person1 = new Person();
   person1.mainFunction(person1.child1Function); 
   // 这时的this，才是Person这个类对象
   注意：bind和call、apply都是一样的效果可以修改当前函数运行环境this的指向，但是call和apply会先执行（在这里不适用）！
   
   3、当然也可以使用最暴力的方式，直接扔参数。
   ```

8. ##### async/await异步函数使用方法

   ```js
   1、首先申明一个异步函数，和一般的命名函数没多大区别，在前面加一个async即可（切记await必须要写在申明了async的函数内部，否则无法实现异步功能）
   let getBlogs = async function(){
       let db = await MongoClient.connect(url + db_name);
       //调用接口时使用await，即表示要等到await后面的执行完毕后才会执行后面的操作。
       let coll = db.collection('blogs');
       let blogs = await coll.find().toArray();
       db.close();
       return blogs;
   };
   getBlogs().then(result=> {
       console.log(result.length);
   }).catch(err=> {
       console.log(err);
   })
   
   2、实际项目中的应用（基于Vue框架）
   loginStatus: async function () {
       let params = {
           client_id: "apps.tenant",
           client_secret: "smartx",
           grant_type: "password",
           userName: this.username,
           password: this.password
       }
       if(!params.userName||!params.password){
           this.modal.tip(myApp,'账号密码不能为空');
           return;
       }
       this.myApp.showIndicator();
       this.myApp.showPreloader('登录中...')
       try{
           var res = await this.axios.post(this.api.login, params,{ headers: { 'Content-Type': 'application/x-www-form-urlencoded' }});
           if (res.access_token) {
               this.myApp.hidePreloader()
               this.myApp.hideIndicator();
               localStorage.setItem("token", res.access_token)
               var res=await this.axios.get(this.api.personInfomation);
               if(res.content){
                   this.localStorageService.set("me",res.content)
                   var _this=this;
                   this.alipayAuthCheck(function(){
                       _this.$emit('IsLogin', false)
                   });
               }
           }
       } catch (err) {
           console.log("res error->" + res);
       }
   }
   ```

9. ##### js执行异常抛错，阻塞进程，解决办法！

   ```js
   问题描述：有这么一种场景，在执行函数时，由于函数内部（代码书写原因，接口返回等原因）可能出现js错误，阻塞进程，后面的代码不会执行，但我们又想让程序继续跑下去。
   let fun1 = function(){
       let data1 = {
           name:'luoliang'
       }
       console.log(data2);
       console.log(data1);
   }
   let fun2 = function(){
       console.log('fun2')
   }
   fun1();
   fun2();
   // 抛错Uncaught ReferenceError: data2 is not defined，并阻塞了进程，后面的console都不会执行
   
   解决办法：
   1、使用try。。。catch方法
   let fun1 = function(){
       let data1 = {
           name:'luoliang'
       }
       console.log(data2);
       console.log(data1);
   }
   let fun2 = function(){
       console.log('fun2')
   }
   try {
       fun1();
       console.log(123);
   } catch (error) {
       console.log(error);
       //这里抛错，可以做一些统一处理
   }
   fun2();
   // ReferenceError: data2 is not defined
   // fun2
   // 我们可以发现同时在try中的后面语句都被阻塞了data1和123都未打印。但是try之外的语句fun2却执行了，所以只阻塞了try中的程序，而未阻塞try之外的语句。
   
   2、自定义错误内容，并用throw抛出。throw也会阻塞当前执行的function，但function之外的代码依然会执行。
   let fun1 = function(){
       let data1 = {
           name:'luoliang'
       }
       console.log(data2);
       console.log(data1);
   }
   let fun2 = function(){
       console.log('fun2')
   }
   try {
       throw '太多了'
       fun1();
       console.log(123);
   } catch (error) {
       console.log(error);
   }
   fun2();
   // 太多了
   // fun2
   ```

10. 虚位以待！