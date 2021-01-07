# module模块化开发规范

1. ##### 模块化开发

   ```js
   背景历史：
   A,B两个开发人员，各自写各自的代码，但现在有一个需求（公用，因为A，B都会用到），那么这个公用的模块就必须先与A,B两人沟通好，采用统一个导入/导出规范，要不然就无法使用。
   当一个网站复杂度较高需要多人协作开发时，传统的非模块化编程模式容易导致代码冲突和依赖等问题，而模块化编程的诞生正是为了解决此类问题。然而，在ES6出来之前，原生JavaScript是不支持模块化的，因此就出现了一系列的JavaScript库来实现此功能，这些库主要遵循以下三种规范：CommonJS，AMD,CMD。
   AMD、CMD、CommonJs、es6新特性模块化，都是用于在模块化定义中使用的，AMD、CMD、CommonJs是ES5中提供的模块化编程的方案，import/export是ES6中定义新增的（也是前端为了规范而统一使用的）。
   
   1、AMD：与CommonJS不同，AMD规范下的模块调用是异步的，主要适用于浏览器客户端。
   试想一下，如果在客户端这样使用：
   var math = require('math');
   math.add(2, 3);
   在客户端，很明显math.add(...)这个方法会先执行，但我们需要的却是var math = require('math')执行完后才调用add方法。因为在客户端即便http再快，也无法满足。所以CommonJs规范不适合客户端，客户端应该使用异步的AMD（Asynchronous Module Definition）。
   AMD是RequireJS在推广过程中对模块定义的规范化产出，它是一个概念。是一个依赖前置、异步定义的AMD框架（在参数里面引入js文件），在定义的同时如果需要用到别的模块，在最前面定义好即在参数数组里面进行引入，在回调里面加载。require([module], callback);
   RequireJS：是一个AMD框架，可以异步加载JS文件，按照模块加载方法，通过数组定义所需要的加载内容，第一个参数是一个数组，里面定义一些需要依赖的包，第二个参数是一个回调函数，通过变量来引用模块里面的方法，最后通过return来输出。
   eg：
   require(['math'], function (math) {
        math.add(2, 3);
   });
   define(['./a','./b'],)
   
   2、CMD:是SeaJS在推广过程中对模块定义的规范化产出，是一个异步加载模块（与AMD类似，却又有不同），是SeaJS的一个标准，SeaJS是CMD概念的一个实现。
   通过define()定义，没有依赖前置，采用就近原则加载（CMD是依赖就近），在什么地方使用到插件就在什么地方require该插件，即用即返，这是一个同步的概念。define(function(require,exports,module){...});
   CMD(Common Module Definition)，通用模块定义，它解决的问题和AMD规范是一样的，只不过在模块定义方式和模块加载时机上不同，CMD也需要额外的引入第三方的库文件，SeaJS
   eg：
   需要引入sea.js
   // cmd2.js
   define(function(require,exports,module){    
       var cmd2 = require('./cmd1') 
       // cmd2.xxx 依赖就近书写
       module.exports={
           // ...
       }
   })                                                                                                                  // 、、、 
   // cmd1.js
   define(function(require,exports,module){
       // ...
       module.exports={
           // ..
       }
   })
   <script src="sea.js"></script>
   <script src="cmd2.js"></script>
   <script src="cmd1.js"></script>
   <script type="text/javascript">
       seajs.use(['cmd2.js','cmd1.js'],function(cmd2,cmd1){
       	// ....
   	})                                                                                                                   </script>
                                                                                                                             
   3、CommonJS：CommonJS规范下的模块调用是同步的，也就是说必须等模块加载完成之后，接下来的代码才能继续运行。因此，该规范主要适用于服务端，因为服务端可以直接从硬盘中调用所需要的模块，而这个过程速度是非常快的。相比之下，通过网络调用所需模块的客户端浏览器则不适合使用该规范。
   使用方式，是通过module.exports定义的，在前端浏览器里面并不支持module.exports,通过node.js后端使用的。Nodejs端是使用CommonJS规范的，前端浏览器一般使用AMD、CMD、ES6等定义模块化开发的。
   eg：
   3.1、定义一个模块a.js
   function foo(){
   	console.log('这里是a模块的一些内容，方法等')
   }
   // 此处提供模块对外接口（模块内的属性或方法，可以重命名。）
   module.exports = {
   	foo: foo,
   	func: foo,
   };
   3.2、引入/使用 a模块
   var module = require(‘./module.js’); // 加载module模块
   module.foo(); // 此处调用module模块下的foo方法，该方法名须与模块中对外接口方法名必须一致。
   module.func();
   
   4、ES6特性模块化：是通过export/import对模块进行导出导入的。在es6模块化出来之前，为了解决模块分离开发问题，前端可谓费尽心机，无论是requireJS还是seaJS都属于第三方的插件，ECMAscript根本就没有一个统一的规范。然而当es6的模块化开发出来之后，所有的AMD,CMD，CommonJS(虽然node还再坚持使用)都变得黯然失色。
   eg：
   在a.js
   let name = 'luoliang';
   let age = 31;
   let sayFun = function(){
       console.log(‘我叫’:+name);
   }
   export {
   	name,
       age as myage, //可以映射暴露出去
   	sayFun,                                                                                                                    }
   在b.js
   import { name,myage,sayFun } from './a.js'; // 注意引入的模块，必须是暴露的模块名，否则找不到。
   console.log(name,myage,sayFun());
   // 当然也可以使用全部引入的方式
   import * as info from './a.js'; // 全部引入，并使用info别名
   console.log(info.name,info.myage,info.sayFun())  
                                                                                                                                // 默认导出模式（默认export default在一个模块内只能是一个。但可以和export{...}混用）                                                     
   export default function(){
       return "默认导出一个方法"
   }
   import fun from './a.js'; // 这里的fun可以随便命名
   fun();
   
   // 重命名引入:如果导入的多个文件中，变量名字相同，即会产生命名冲突的问题。
   // 在test1.js中
   export let myName="我来自test1.js";
   // 在test2.js中
   export let myName="我来自test2.js";
   // 在test3.js中导入
   import {myName as name1} from "./test1.js";
   import {myName as name2} from "./test2.js";
   console.log(name1);//我来自test1.js
   console.log(name2);//我来自test1.js
                                                                                                                                
   // 混合导出
   export default function(){
       return "默认导出一个方法"
   }
   export var myName="laowang";
   import myFn,{myName} from "./a.js";
   console.log(myFn(),myName);
   
   ```

2. 虚位以待！