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
   
   
   ```

2. 虚位以待！