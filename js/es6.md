# es6语法笔记

参考：ECMAScript 6 入门 --阮一峰著 http://es6.ruanyifeng.com/

1. ##### 变量声明、使用

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
   
   5、const的使用
   const：申明一个只读的常量，一旦申明，常量的值就没法改变（只是引用地址不能改变，但可以往里面添加/删除数据，随后会详细解释）
   const的本质是：保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。
   5.1、不能修改引用地址的情况
   const a = 1;
   a = 2;  //抛错 Assignment to constant variable.
   5.2、修改引用地址中的值，但未修改引用地址时，正确
   const a = [];
   a.push(1);    //往数组中添加数据（可行），没有修改的值（引用地址未发生改变）
   console.log(a); //a=[1]
   a = [1];  //抛错，因为修改了引用地址
   5.3、const的作用域和let的使用方法相同
   
   6、变量赋值
   单个变量赋值 
   let a = 1;
   let b = 2;
   let c = 3;
   ES6语法(对号入座)
   let [a,b,c] = [1,2,3];  //和上面得到的一样效果
   逗号分隔
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
       //arguments和values，都包含了参数数组[2,5,3]，不同的是values更加简单明了
       console.log(arguments); 
       console.log(values);
       let sum = 0;
       for (var val of values) {
           sum += val;
       }
       return sum;
   }
   add(2, 5, 3) // 10
   //注意：这里提一下，箭头函数没有arguments，所以无法通过arguments获取参数。而是通过..values的方式传参。详见箭头函数部分
   
   3、函数对象传参
   function test({ sendData, handleMessage, title, style }){
       console.log(sendData,handleMessage,title,style)
   }
   test({sendData:123,handleMessage:'doSomething',title:'标题',style:{color:red,fontSize:12px}});
   这样假如我们不需要传递第2,4个参数时，test({sendData:123,title:'标题'});也不会出现任何问题。
   
   4、箭头函数
   4.1、箭头函数this为父作用域的this，不是调用时的this。任何方法都改变不了（包括call，apply，bind）
   let person = {
       name:'jike',
       init:function(){
           document.body.onclick = ()=>{
               alert(this.name);
           }
       }
   }
   person.init(); // jike
   如果是 
   document.body.onclick = function(){
       alert(this.name);
   }
   person.init(); //undefined
   4.2 箭头函数不能作为构造函数，不能使用new
   //构造函数如下：
   function Person(p){
       this.name = p.name;
   }
   //如果用箭头函数作为构造函数，则如下
   var Person = (p) => {
       this.name = p.name;
   }
   //Person is not a constructor
   4.3、箭头函数没有arguments，使用...rest方法传参
   let B = (b)=>{
     console.log(arguments);
   }
   B(2,92,32,32);   // Uncaught ReferenceError: arguments is not defined
   let C = (...c) => {
     console.log(c);
   }
   C(3,82,32,11323);  // [3, 82, 32, 11323]
   4.4、 箭头函数没有原型属性（没有原型链prototype）
   var a = ()=>{
     return 1;
   }
   function b(){
     return 2;
   }
   console.log(a.prototype);  // undefined
   console.log(b.prototype);   // {constructor: ƒ}
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
           // 这里可以做一些操作
           console.log('setter:'+value);
       }
   }
   let inst = new MyClass();
   inst.prop = 123; // 拦截存储行为，执行set prop(value){...}方法，并打印setter: 123
   inst.prop //'getter'
   
   4、class的静态方法。
   // 所有定义在类class中的方法，都会被new之后的实例所继承
   class Foo {
   	classMethod() {
           return 'hello';
       }
   }
   let fooTest = new Foo();
   fooTest.classMethod(); // hello
   // 但是有时候我们又不想让实例用到该构造函数的方法？
   class Foo {
       //声明一个静态方法，实例化无法调用！
       static classMethod() {
           return 'hello';
       }
   }
   let fooTest = new Foo();
   fooTest.classMethod(); // TypeError: foo.classMethod is not a function
   静态属性和方法 #
   class Foo {
       #name = '';
   	#classMethod() {
       	console.log(this.#name) //只有内部可以访问
       }
   }
   let fooTest = new Foo();
   fooTest.#classMethod();  // 抛错
   
   5、class继承
   es5方式：
   1、call继承方法:将父对象的构造函数绑定在子对象上，即在子对象构造函数中加一行Animal.call(this,arguments);
   // 动物类
   function Animal(arguments){
       this.name = arguments[0];
       this.age = arguments[1];
       this.height = arguments[2];
       this.color = arguments[3];
       this.say = function(){
           return "动物的名字叫：" + this.name + "。高度是：" + this.height + "。年龄是：" + this.age + "。颜色是：" + this.color
       }
   }
   // 猫科类
   function Cat(name,age,height,color){
       //arguments是将所有的参数(name,age,height,color)组成一个数组传递过去，所有使用时用arguments[0],arguments[1],arguments[2],arguments[3]来赋值
       Animal.call(this,arguments);
   }
   Cat.prototype.type = "猫科动物";
   var cat1 = new Cat("大毛",27,168,"黑色");
   console.log(cat1.type);
   2、apply继承方法:将父对象的构造函数绑定在子对象上，即在子对象构造函数中加一行Animal.apply(this,tips);
   // 动物类
   function Animal(name,age,height,color){
       this.name = name;
       this.age = age;
       this.height = height;
       this.color = color;
       this.say = function(){
           return "动物的名字叫：" + name + "。高度是：" + height + "。年龄是：" + age + "。颜色是：" + color
       }
   }
   //猫科类
   function Cat(name,age,height,color){
       //arguments是将所有参数组成一个对象传递过去，所以最后还是以name,age,height,color挨着赋值
       var tips = [name,age,height,color]; //可以封装数组，也可以封装对象
       Animal.apply(this,tips);
   }
   Cat.prototype.type = "猫科动物";
   var cat1 = new Cat("大毛",27,168,"黑色");
   console.log(cat1.type);
   3、prototype继承方法
   function Animal(name,age,height,color){
       this.name = name;
       this.age = age;
       this.height = height;
       this.color = color;
       this.say = function(){
           return "动物的名字叫：" + name + "。高度是：" + height + "。年龄是：" + age + "。颜色是：" + color
       }
   }
   function Cat(name,color){
       this.name = name;
       this.color = color;
   }
   Cat.prototype = new Animal();
   var cat1 = new Cat("大毛","黄色");
   console.log(cat1.constructor); //指向Animal，而不是Cat
   注：此时cat1虽然是由Cat构造器生成的，但Cat的prototype指向了Animal构造器的一个实例，所以Cat的prototype指向Animal，
   那生成的cat1也应该指向Animal。这显然会导致继承链的紊乱，所以需要手动加上Cat.prototype.constructor = Cat;
   //当继承的属性在prototype上时，可以直接继承(不用通过new一个新的继承实例)eg：
   function Animal(){}
   Animal.prototype.type = "动物类"
   function Cat(){}
   Cat.prototype = Animal.prototype;
   var cat1 = new Cat();
   Cat.prototype.constructor = Cat;
   console.log(cat1.type);
   4、利用空对象作为中介（推荐使用）
   function Animal(name,age,height,color){
       this.name = name;
       this.age = age;
       this.height = height;
       this.color = color;
       this.say = function(){
           return "动物的名字叫：" + name + "。高度是：" + height + "。年龄是：" + age + "。颜色是：" + color
       }
   }
   function Cat(name,color){
       this.name = name;
       this.color = color;
   }
   function extend(Child, Parent) {
       var F = function(){};	//空对象
       F.prototype = Parent.prototype;
       Child.prototype = new F();
       Child.prototype.constructor = Child;
       Child.uber = Parent.prototype;
   }
   extend(Cat,Animal);
   var cat1 = new Cat("大毛","黄色");
   console.log(cat1);
   5、拷贝继承：把父对象的所有属性和方法，拷贝进子对象。
   function Animal(){}
   Animal.prototype.style = "动物的公共属性";
   Animal.prototype.method = "动物的公共方法";
   Animal.prototype.do = function(){
       console.log("动物都可以运动");
   };
   function Cat(){}
   function extend2(Child, Parent) {
       var p = Parent.prototype;
       var c = Child.prototype;
       console.log(p);
       for (var i in p) {	//将父对象的prototype对象中的属性，一一拷贝给Child对象的prototype对象
           c[i] = p[i];
       }
       c.uber = p;
   }
   extend2(Cat, Animal);
   var cat1 = new Cat("大毛","黄色");
   6、浅拷贝继承.
   var Chinese = {
       nation:"Chinese",
       face:"yellow",
       birthPlace:["北京","上海","广州"]
   }
   function extendCopy(p) {
       var c = {};
       for (var i in p) {
           c[i] = p[i];
       }
       c.uber = p;//可不用
       return c;
   }
   var Doctor = extendCopy(Chinese);
   Doctor.career = '医生';
   Doctor.birthPlace.push("成都");
   console.log(Doctor);
   console.log(Chinese);
   //注：浅拷贝的弊端，如Chinese中有一个birthPlace属性(数组),当我们在生成的Doctor往birthPlace中添加“成都”时，
   此时的Chinese中也添加了“成都”(因为我们拷贝的birthPlace只是数组的一个引用，而不是真正的拷贝，最终指向的还是该数组)，
   子属性修改影响到了父属性,解决方法：深拷贝
   7、深拷贝继承(当copy的chinese属性是一个对象或者数组时，再次循环该数组或对象，达到更深的拷贝)
   var Chinese = {
       nation:"Chinese",
       face:"yellow",
       birthPlace:["北京","上海","广州"]
   }
   function deepCopy(p, c) {
       var c = c || {};
       for (var i in p) {
           if (typeof p[i] === 'object') {	//判断是否为数组或对象
               c[i] = (p[i].constructor === Array) ? [] : {};
               deepCopy(p[i], c[i]);
           } else {
               c[i] = p[i];
           }
       }
       return c;
   }
   var Doctor = deepCopy(Chinese);
   Doctor.career = '医生';
   Doctor.birthPlace.push("成都");
   console.log(Doctor);
   console.log(Chinese);
   //注：此时的Doctor中的birthPlace中有“成都”，但Chinese中没有“成都”，因为我们将birthPlace以及对应的["北京","上海","广州"]全部拷贝了(正因为如此，所以该方法很占内存)
   
   es6方式：使用extends和super
   // 父类Foo
   class Foo {
       constructor(){
           this.name = 'Foo';
       }
       static classMethod() {
           return 'hello';
       }
   }
   //子类Bar继承Foo
   class Bar extends Foo {
       //props是继承过来的属性，myAttribute是自己的属性
   	constructor(props,myAttribute){
           super(props) //相当于获得父类的this指向
       }
   }
   Bar.classMethod() //'hello'
   let barTest = new Bar();
   barTest.classMethod(); // 抛错，不存在
   注：extends继承，也包括父类的static方法继承，所以原型可以访问static方法，但是实例还是无法拿到static方法。
   
   多个继承实现方式: https://blog.csdn.net/weixin_43343144/article/details/92657964
   C同时继承A和B
   方法一：链式继承
   class A {
       ......
   }
   class B extends A {
       ......
   }
   class C extends B {
       ......
   }
   描述：这样链式继承很好的达到了多继承效果，而且追溯也很方便C类可以直接拿到A类的方法和属性。但是缺点也很明显，C类如果还想继承D类的话，就必须在A类和B类间再插入一个D类，这样在后期扩展时极为不方便。
   方法二：拷贝继承（将类中的属性、原型拷贝到当前类中，再使用super函数）
   function mix(...mixins) {
       class Mix {
           constructor() {
               for (let mixin of mixins) {
                   copyProperties(this, new mixin()); // 拷贝实例属性
               }
           }
       }
       for (let mixin of mixins) {
           copyProperties(Mix, mixin); // 拷贝静态属性
           copyProperties(Mix.prototype, mixin.prototype); // 拷贝原型属性
       }
       return Mix;
   }
   function copyProperties(target, source) {
       for (let key of Reflect.ownKeys(source)) {
           if ( key !== 'constructor' && key !== 'prototype' && key !== 'name') {
               let desc = Object.getOwnPropertyDescriptor(source, key);
               Object.defineProperty(target, key, desc);
           }
       }
   }
   class A {
       constructor(){
           this.a = 'a';
       }
       sayA (){
           console.log(this.a);
       }
   }
   class B {
       constructor(){
           this.b = 'b';
       }
       sayB (){
           console.log(this.b);
       }
   }
   class C extends mix(A,B){
       constructor(){
           super();
           this.c = 'c'
       }
       sayC(){
           console.log(this.a)
           console.log(this.b)
           console.log(this.c)
       }
   }
   let cInstance = new C();
   cInstance.sayC() //ａ、ｂ、ｃ
   描述：拷贝继承达到了预期效果，方便多个扩展继承，但是依然有弊端，例如:C继承A，A又继承B，但是C拿不到B类中的方法和属性，因为拷贝只能是当前类的方法和属性，不包括原型链上。
   总结：没有最好的办法，只有根据当前需求最合理的解决办法。
   ```

4. ##### Promise的用法。参考: https://blog.csdn.net/shan1991fei/article/details/78966297

   ```js
   1、什么是promise？
   简单来讲，就是能把原来的回调写法分离出来，在异步操作执行完后，用链式调用的方式执行回调函数，它是一种异步方法。
   基本使用：Promise是一个构造函数，允许一个回调方法，并接受两个参数resolve、reject，这两个参数都是function。这两个参数都可以修改promise的状态，resolve是成功状态，reject是失败状态，我们可以通过这两个方法将结果返回出去，在通过then方法接收。then方法也有两个回调function，第一个就是resove的成功回调，第二个就是reject的失败回调。除了这resolve和reject外，还有一个throw也可以修改promise的状态，但它只能走错误的回调
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
   },function(err){
       //error就是reject中传递过来的参数
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
   最后再返回数组
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
   
   3、当然也可以使用最暴力的方式，直接扔参数，将当前环境的this已参数的方式传入。
   ```

8. ##### async/await异步函数使用方法（其实是es7提出的）

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

10. ##### js扩展运算符...

    ```js
    1、函数中使用
    function myFunction(x, y, z) { 
      console.log(x+""+y+""+z);
    } 
    var args = [0, 1, 2]; 
    myFunction.apply(null, args);
    改进后
    function myFunction(x, y, z) { 
    	console.log(x+""+y+""+z); 
    } 
    var args = [0, 1, 2];
    myFunction(...args);
    注：...arr返回的并不是一个数组，而是各个数组的值。只有[...arr]才是一个数组，所以...arr可以用来对方法进行传值
    
    2、数组和对象的拷贝(深拷贝，不是简单的指向修改)。
    数组
    var arr1 = [1,2,3];
    var arr2 = [...arr1];
    arr2.push(4);
    console.log(arr1 === arr2);  // false
    console.log(arr1); // [1,2,3]
    console.log(arr2);// [1,2,3,4]
    对象
    var obj1 = {
        a:1,
        b:2
    };
    var obj2 = {...obj1};
    console.log(obj2); //{ a:1, b:2}
    console.log(obj1 === obj2);// false
    
    3、字符串转数组
    var demo = "hello"
    var str = [...demo];
    console.log(str);// ["h", "e", "l", "l", "o"]
    ```

11. ##### 解构赋值 https://www.cnblogs.com/xiaohuochai/p/7243166.html#anchor5

    ```js
    1、对象解构
    es5语法：
    let options = {
        repeat: true,
        save: false
    };
    let repeat = options.repeat,
    	save = options.save;
    es6语法：
    let options = {
        repeat: true,
        save: false
    };
    let { repeat, save, value=true } = options;
    console.log(repeat); // "Identifier"
    console.log(save); // "foo"
    console.log(value); // true,不存在为value的key，则赋值为undefined,如果给了默认值就用默认值
    // 这里要注意：声明的变量名为obj的key，如：repeat，save，当然也可以重命名。
    let { repeat:renameRepeat, save:renameSave, value=true } = options; 
    console.log(renameRepeat); // "Identifier"
    console.log(renameSave); // "foo"
    对象嵌套的解构
    
    2、数组解构	
    let colors = [ "red", "green", "blue" ];
    let [ firstColor,,thirdColor ] = colors; // 对号入座方式,占位符
    console.log(firstColor); // "red"
    console.log(thirdColor); // "blue"
    // 和对象一样，也有默认值
    let colors = [ "red" ];
    let [ firstColor, secondColor = "green" ] = colors;
    console.log(firstColor); // "red"
    console.log(secondColor); // "green"
    // 变量值的交换
    在es5中
    let a = 1,b = 2,tmp;
    tmp = a;
    a = b;
    b = tmp;
    console.log(a); // 2
    console.log(b); // 1
    在es6中
    let a = 1,b = 2;
    [ a, b ] = [ b, a ];
    console.log(a); // 2
    console.log(b); // 1
    数组嵌套解构
    
    3、字符串的解构，是转换为数组的方式
    
    4、混合解构
    ```

10. ##### 可选链操作符

    ```js
    ECMAScript机构还在审核阶段，各浏览器暂时不支持！ https://www.jianshu.com/p/2852b6efed8e
    ES11即ECMAScript2020已经正式通过！！！
    问题描述：有一个对象，多个嵌套属性，但有可能不存在
    let obj = {
        luoliang:{
            name:"luoliang",
            age:31
        }
    }
    let obj1 = {}
    let age = obj.luoliang.age // 已确定对象属性
    let age1 = obj1.luoliang.age // 抛错
    我们一般会这样：let age1 = obj1 && obj1.luoliang && obj1.luoliang.age || 0; 
    // 虽然实现了结果，但很不方便，如果有更深的嵌套就会很麻烦，解决办法：
    let age1 = obj1?.luoliang?.age
    ```

13. ##### 空位合并操作符

    ```js
    问题描述：当一个变量为空值时，就使用默认值
    let c = a ? a ： b; 
    let c = a || b;
    // 如果a存在就用a，不存在就用b
    这种方式有个弊端就是：如果a是0，‘’，false时，依然会用b，但a其实是存在的 0，‘’，false，解决办法：
    let c = a ?? b;
    当a不为undefined，null时，用a值，否则才用b值，这样就避免了a为0，''，false的尴尬情况
    let a = 0;
    let b = 10;
    let c = a ?? b; // c=0
    ```

14. ##### js strict模式

    ```js
    strict模式产生原因：JavaScript在设计之初，为了方便初学者学习，并不强制要求用var申明变量。这个设计错误带来了严重的后果：如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量：
    i = 10; // i现在是全局变量
    在同一个页面的不同的JavaScript文件中，如果都不用var申明，恰好都使用了变量i，将造成变量i互相影响，产生难以调试的错误结果，污染全局变量。
    为了修补JavaScript这一严重设计缺陷，ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误。
    
    使用方法：在JavaScript代码的第一行写上：
    'use strict';
    注意：这是一个字符串，不支持strict模式的浏览器会把它当做一个字符串语句执行，支持strict模式的浏览器才会开启strict模式。
    
    注意点;
    1、严格模式下, delete运算符后跟随非法标识符(即delete 不存在的标识符)，会抛出语法错误； 非严格模式下，会静默失败并返回false
    2、严格模式中，对象直接量中定义同名属性会抛出语法错误； 非严格模式不会报错
    3、严格模式中，函数形参存在同名的，抛出错误； 非严格模式不会
    4、严格模式不允许八进制整数直接量（如：023）
    5、严格模式中，arguments对象是传入函数内实参列表的静态副本；非严格模式下，arguments对象里的元素和对应的实参是指向同一个值的引用
    6、严格模式中 eval和arguments当做关键字，它们不能被赋值和用作变量声明
    7、严格模式会限制对调用栈的检测能力，访问arguments.callee.caller会抛出异常
    8、严格模式 变量必须先声明，直接给变量赋值，不会隐式创建全局变量，不能用with,
    9、严格模式中 call apply传入null undefined保持原样不被转换为window
    ```

13. ##### Map类 使用方法

    ```js
    Map结构提供了“键—值”的对应，是一种更完善的Hash结构实现。如果你需要“键值对”的数据结构，Map比Object更合适。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
    1、使用
    let map = new Map();
    let obj = {name:1};
    map.set(true,'111');
    map.set(obj,'111');
    map.set(1,1);
    map.set(undefined,undefined);
    map.set(null,null);
    map.set(NaN,NaN);
    map.set([1,2,3],1);
    console.log(map); 
    注意：只有对同一个对象的引用，Map结构才将其视为同一个键。
    let map = new Map();
    map.set(['a'], 555);
    map.get(['a']) // undefined
    let obj = {
        name:'luoliang'
    }
    map.set(obj,123)
    map.get(obj) // 123
    第一种，因为最开始的['a']它的内存地址和map.get(['a'])的内存地址不一样，所以无法取到！。
    第二种，obj的内存地址已经存在，且map.get(obj)指向同一地址，所以可以获取到！
    
    2、Map常用实例方法
    size、set、get、has、delete、clear
    
    3、Map常用遍历方法
    keys()、values()、entries()、forEach()
    
    4、Map与数组Array的对比
    let map = new Map();
    let arr = new Array();
    //增：
    map.set('a',1);
    arr.push({'a': 1});
    //查：
    map.has('a');
    arr.find(item=>item.a);
    //改：
    map.set('a',2);
    arr.forEach(item=>item.a?item.a=2:'');
    //删：
    map.delete('a');
    arr.splice(arr.findIndex(item=>item.a),1);
    console.log(map);
    console.log(arr);
    
    5、Map与对象object的对比
    let item = {a: 1};
    let set = new Set();
    let map = new Map();
    let obj = new Object();
    //增
    set.add(item);
    map.set('a', 1);
    obj['a'] = 1;
    //查
    set.has(item);// true
    map.has('a');// true
    'a' in obj;// true
    //改
    item.a = 2;
    map.set('a', 2);
    obj['a'] = 2;
    //删
    set.delete(item);
    map.delete('a');
    delete obj['a'];
    console.log(set);
    console.log(map);
    console.log(obj);
    
    总结：在开发过程中，涉及到数据结构，能使用Map不使用Array，尤其是复杂的数据结构，如果对于数组的存储考虑唯一性使用Set，如果要求数据储存的唯一性使用Set，放弃使用Array。
    Map 和 WeakMap 是一种[键,值]的集合，Map的键可以是任意类型，WeakMap的键只能是对象类型
    
    WeakMap后续补充
    ```
    
14. ##### Set 类的使用。参考：https://www.cnblogs.com/wjcoding/p/11690886.html

    ```js
    ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
    const set = new Set([1,2,1,2])
    console.log(set) // {1,2} 可以看出Set是可以去除数组中的重复元素
    
    Set的属性
    size: 返回集合中所包含的元素的数量
    const set = new Set([1,2,1,2])
    console.log(set.size) // 2
    
    Set的方法：
    add(value): 向集合中添加一个新的项，无返回值
    delete(value): 从集合中删除一个值，无返回值
    has(value): 判断值是否存在集合中，返回ture/false
    clear(): 移除集合中的所有项，无返回值
    let set = new Set()
    set.add(1)
    set.add(2)
    set.add(2)
    set.add(3)
    console.log(set) // {1,2,3} 这里第一次add的2，被第二次add的2覆盖掉了
    set.has(2) // true
    set.delete(2)
    set.has(2) // false
    set.clear() 
    
    遍历方法：
    keys(): 返回键名的遍历器
    values(): 返回键值的遍历器
    entries(): 返回键值对的遍历器
    forEach(): 使用回调函数遍历每个成员
    let set = new Set([1,2,3,4]);
    console.log(Array.from(set.keys())) // [1,2,3,4] console.log([...set.keys()]) 同样转换
    console.log(Array.from(set.values())) // [1,2,3,4]
    console.log(Array.from(set.entries())) //  [[1,1],[2,2],[3,3],[4,4]]
    set.forEach((item) => { console.log(item)}) // 1,2,3,4
    注意：由于set只有键值，没有键名，所以keys() values()行为完全一致
    
    应用场景：
    1、去重，因为Set结构的值是唯一的，所以可以用来去重
    let arr = [1, 1, 2, 3];
    let arr1 = new Set(arr);
    let arr2 = [...arr1];
    console.log(arr) // [1,1,2,3]
    console.log(arr1)
    // 返回的是一个类数组（不是数组，也不是对象）
    Set(3) {1, 2, 3}
    [[Entries]]
    0: 1
    1: 2
    2: 3
    size: 3
    __proto__: Set
    console.log(arr2) // [1,2,3] 将类数组转换为数组
    
    2、取并集
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
    let union = [...new Set([...a, ...b])]; // [1,2,3,4]
    注意：两个数组的合并，需要展开[..a,...b]
    
    3、取交集
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
    let intersect = [...new Set([...a].filter(x => b.has(x)))]; // [2,3]
    
    4、取差集（取的是，a数组中的值，没有包含在b数组中的值）
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
    let difference = Array.from(new Set([...a].filter(x => !b.has(x)))); // [1]
    
    5、取不同时在a,b两数组中的值
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
    let difference1 = Array.from(new Set([...a].filter(x => !b.has(x)))); // [1]
    let difference2 = Array.from(new Set([...b].filter(x => !a.has(x)))); // [4]
    let result = [...difference1,...difference2]; // [1,4]
    
    总结：Set、WeakSet 是[值,值]的集合，且具有唯一性。
    WeakSet后续补充
    ```

15. #####  Proxy代理，参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    ```js
    Proxy用于修改某些操作的默认行为，等同于在语言层面做出修改，即对编程语言进行编程。
    Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截。因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
    
    1、基本使用：
    var proxy = new Proxy(target, handler);
    // new Proxy()表示生成一个Proxy实例，target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为
    实例：
    var target = {
    	name: 'poetries'
    };
    var logHandler = {
        get: function(target, key) {
            console.log(`${key} 被读取`);
            return target[key];
        },
        set: function(target, key, value) {
            console.log(`${key} 被设置为 ${value}`);
            target[key] = value;
            return true;
        }
    }
    var targetWithLog = new Proxy(target, logHandler);
    targetWithLog.name; // 控制台输出：name 被读取
    targetWithLog.name = 'others'; // 控制台输出：name 被设置为 others
    console.log(target.name); // 控制台输出: others
    // 由实例可以看出，一个对象的拦截只有生成new Proxy()对象后，才有拦截效果，其原始对象仍然不变。
    注意：在设置对象代理的时候，代理器的get,set操作，最后一定要返回一个值，否则在严格模式下会抛错！！！，如果return false会阻塞进程！！！
    当然在定义一个空对象的时候，依然可以代理，这个根据实际使用情况而定！！！
    var proxy = new Proxy({}, {
        get: function(target, property) {
            return 35;
        }
    });
    
    2、Proxy所能代理的范围--handler
    实际上handler本身就是ES6所新设计的一个对象。它的作用就是用来：自定义代理对象的各种可代理操作。它本身一共有13中方法,每种方法都可以代理一种操作
    // 在读取代理对象的原型时触发该操作，比如在执行 Object.getPrototypeOf(proxy) 时。
    handler.getPrototypeOf()
    // 在设置代理对象的原型时触发该操作，比如在执行 Object.setPrototypeOf(proxy, null) 时。
    handler.setPrototypeOf()
    // 在判断一个代理对象是否是可扩展时触发该操作，比如在执行 Object.isExtensible(proxy) 时。
    handler.isExtensible()
    // 在让一个代理对象不可扩展时触发该操作，比如在执行 Object.preventExtensions(proxy) 时。
    handler.preventExtensions()
    // 在获取代理对象某个属性的属性描述时触发该操作，比如在执行 Object.getOwnPropertyDescriptor(proxy, "foo") 时。
    handler.getOwnPropertyDescriptor()
    // 在定义代理对象某个属性时的属性描述时触发该操作，比如在执行 Object.defineProperty(proxy, "foo", {}) 时。
    andler.defineProperty()
    // 在判断代理对象是否拥有某个属性时触发该操作，比如在执行 "foo" in proxy 时。
    handler.has()
    // 在读取代理对象的某个属性时触发该操作，比如在执行 proxy.foo 时。
    handler.get()
    // 在给代理对象的某个属性赋值时触发该操作，比如在执行 proxy.foo = 1 时。
    handler.set()
    // 在删除代理对象的某个属性时触发该操作，比如在执行 delete proxy.foo 时。
    handler.deleteProperty()
    // 在获取代理对象的所有属性键时触发该操作，比如在执行 Object.getOwnPropertyNames(proxy) 时。
    handler.ownKeys()
    // 在调用一个目标对象为函数的代理对象时触发该操作，比如在执行 proxy() 时。
    handler.apply()
    // 在给一个目标对象为构造函数的代理对象构造实例时触发该操作，比如在执行new proxy() 时。
    handler.construct()
    
    3、Proxy的作用
    3.1、拦截和监视外部对对象的访问
    3.2、降低函数或类的复杂度
    3.3、在复杂操作前对操作进行校验或对所需资源进行管理
    
    4、Proxy的应用场景
    4.1、实现私有变量
    var target = {
        name: 'poetries',
        _age: 22  //　以‘＿’开头的变量一般认为是私有变量
    }
    var logHandler = {
        get: function(target,key){
            if(key.startsWith('_')){
                console.log('私有变量age不能被访问')
                return false
            }
            return target[key];
        },
        set: function(target, key, value) {
            if(key.startsWith('_')){
                console.log('私有变量age不能被修改')
                return false
            }
            target[key] = value;
    		return true;
        }
    } 
    var targetWithLog = new Proxy(target, logHandler);
    // 私有变量age不能被访问
    targetWithLog.name; 
    // 私有变量age不能被修改
    targetWithLog.name = 'others';
    console.log(target._age) // 22
    
    4.2、抽离校验模块。
    一个简单的校验模块
    let numericDataStore = {  
        count: 0,
        amount: 1234,
        total: 14
    };
    let numericDataStoreProxy = new Proxy(numericDataStore, {  
        set(target, key, value, proxy) {
            if (typeof value !== 'number') {
                throw Error("Properties in numericDataStore can only be numbers");
            }
            return Reflect.set(target, key, value, proxy);
        }
    });
    numericDataStoreProxy.count = "foo"; // 抛错，因为"foo"不是number
    numericDataStoreProxy.count = 333; // 赋值成功
    
    如果要直接为对象的所有属性开发一个校验器可能很快就会让代码结构变得臃肿，使用Proxy则可以将校验器从核心逻辑分离出来自成一体。
    function createValidator(target, validator) {  
        return new Proxy(target, {
            _validator: validator,
            set(target, key, value, proxy) {
                if (target.hasOwnProperty(key)) {
                    let validator = this._validator[key];
                    if (!!validator(value)) {
                        return Reflect.set(target, key, value, proxy);
                    } else {
                        throw Error(`Cannot set ${key} to ${value}. Invalid.`);
                    }
                } else {
                    throw Error(`${key} is not a valid property`)
                }
            }
        });
    }
    const personValidators = {  
        name(val) {
            return typeof val === 'string';
        },
        age(val) {
            return typeof age === 'number' && age > 18;
        }
    }
    class Person {  
        constructor(name, age) {
            this.name = name;
            this.age = age;
            return createValidator(this, personValidators);
        }
    }
    const bill = new Person('Bill', 25);
    // 以下操作都会报错
    bill.name = 0;  
    bill.age = 'Bill';  
    bill.age = 15;
    
    通过校验器和主逻辑的分离，你可以无限扩展 personValidators 校验器的内容，而不会对相关的类或函数造成直接破坏。更复杂一点，我们还可以使用 Proxy 模拟类型检查，检查函数是否接收了类型和数量都正确的参数
    let obj = {  
        pickyMethodOne: function(obj, str, num) { /* ... */ },
        pickyMethodTwo: function(num, obj) { /*... */ }
    };
    const argTypes = {  
        pickyMethodOne: ["object", "string", "number"],
        pickyMethodTwo: ["number", "object"]
    };
    obj = new Proxy(obj, {  
        get: function(target, key, proxy) {
            var value = target[key];
            return function(...args) {
                var checkArgs = argChecker(key, args, argTypes[key]);
                return Reflect.apply(value, target, args);
            };
        }
    });
    function argChecker(name, args, checkers) {  
        for (var idx = 0; idx < args.length; idx++) {
            var arg = args[idx];
            var type = checkers[idx];
            if (!arg || typeof arg !== type) {
                console.warn(`You are incorrectly implementing the signature of ${name}. Check param ${idx + 1}`);
            }
        }
    }
    obj.pickyMethodOne();
    obj.pickyMethodTwo("wopdopadoo", {});  
    obj.pickyMethodOne({}, "a little string", 123);  
    obj.pickyMethodOne(123, {});
    
    4.3、访问日志
    对于那些调用频繁、运行缓慢或占用执行环境资源较多的属性或接口，开发者会希望记录它们的使用情况或性能表现，这个时候就可以使用 Proxy 充当中间件的角色，轻而易举实现日志功能（实例有问题，有待验证！！！）
    let api = {  
        _apiKey: '123abc456def',
        getUsers: function() { /* ... */ },
        getUser: function(userId) { /* ... */ },
        setUser: function(userId, config) { /* ... */ }
    };
    function logMethodAsync(timestamp, method) {  
        setTimeout(function() {
            console.log(`${timestamp} - Logging ${method} request asynchronously.`);
        }, 0)
    }
    api = new Proxy(api, {  
        get: function(target, key, proxy) {
            var value = target[key];
            return function(...arguments) {
                logMethodAsync(new Date(), key);
                return Reflect.apply(value, target, arguments);
            };
        }
    });
    api.getUsers();
    
    4.4、预警和拦截
    假设你不想让其他开发者删除 noDelete 属性，还想让调用 oldMethod 的开发者了解到这个方法已经被废弃了，或者告诉开发者不要修改 doNotChange 属性，那么就可以使用 Proxy 来实现
    let dataStore = {  
        noDelete: 1235,
        oldMethod: function() {/*...*/ },
        doNotChange: "tried and true"
    };
    const NODELETE = ['noDelete'];  
    const NOCHANGE = ['doNotChange'];
    const DEPRECATED = ['oldMethod'];  
    dataStore = new Proxy(dataStore, {  
        set(target, key, value, proxy) {
            if (NOCHANGE.includes(key)) {
                throw Error(`Error! ${key} is immutable.`);
            }
            return Reflect.set(target, key, value, proxy);
        },
        deleteProperty(target, key) {
            if (NODELETE.includes(key)) {
                throw Error(`Error! ${key} cannot be deleted.`);
            }
            return Reflect.deleteProperty(target, key);
    
        },
        get(target, key, proxy) {
            if (DEPRECATED.includes(key)) {
                console.warn(`Warning! ${key} is deprecated.`);
            }
            var val = target[key];
    
            return typeof val === 'function' ?
                function(...args) {
                    Reflect.apply(target[key], target, args);
                } : val;
        }
    });
    dataStore.doNotChange = "foo";  
    delete dataStore.noDelete;  
    dataStore.oldMethod();
    
    4.5、过滤操作
    某些操作会非常占用资源，比如传输大文件，这个时候如果文件已经在分块发送了，就不需要在对新的请求作出相应（非绝对），这个时候就可以使用 Proxy 对当请求进行特征检测，并根据特征过滤出哪些是不需要响应的，哪些是需要响应的。下面的代码简单演示了过滤特征的方式，并不是完整代码，相信大家会理解其中的妙处
    let obj = {  
        getGiantFile: function(fileId) {/*...*/ }
    };
    obj = new Proxy(obj, {  
        get(target, key, proxy) {
            return function(...args) {
                const id = args[0];
                let isEnroute = checkEnroute(id);
                let isDownloading = checkStatus(id);      
                let cached = getCached(id);
    
                if (isEnroute || isDownloading) {
                    return false;
                }
                if (cached) {
                    return cached;
                }
                return Reflect.apply(target[key], target, args);
            }
        }
    });
    
    4.6、中断代理
    Proxy支持随时取消对target的代理，这一操作常用于完全封闭对数据或接口的访问。使用了Proxy.revocable方法可以创建、撤销代理的代理对象
    let sensitiveData = { username: 'devbryce' };
    const {sensitiveData, revokeAccess} = Proxy.revocable(sensitiveData, handler);
    function handleSuspectedHack(){  
        revokeAccess();
    }
    console.log(sensitiveData.username);
    handleSuspectedHack();
    console.log(sensitiveData.username);
    
    5、在上面的实例中，我们常用到Reflect，它是什么呢？ 参考：https://blog.csdn.net/jianghao233/article/details/80175845
    
    
    提示：据说vue3.0版本的数据响应双向绑定就是使用的Proxy方式（有待验证！！！）
    ```

18. 虚位以待！