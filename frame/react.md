# react笔记

1. ##### react安装运行

   ```jsx
   1、Node >= 8.10
   2、npm >= 5.6
   3、cmd到文件目录下，执行
   npx create-react-app my-app // npx是npm 5.2+ 附带的 package 运行工具。
   cd my-app
   npm start
   ```

2. ##### react的渲染机制

   ```
   一般操作dom的渲染，有以下几种方式
   1、拼接html片段，然后直接append到dom树上
   eg: let str = '<div>内容123</div>'
   	document.body.appendChild(str);
   2、使用dom的api方法，直接操作dom
   eg: let testEle = document.getElementById('test');
   	testEle.innerHTML = '内容123'
   这两种方式都能实现dom的渲染、更新数据后再重绘，但有一个很大的弊端，假如是一个循环的渲染，就会操作dom做很多重绘的功能，我们都知道浏览器端最浪费性能的往往是dom的回流和重绘，所以我们应当尽量避免回流重绘。所以react提出了一种虚拟dom的方式来渲染，更新数据后，虚拟的dom只重绘变更的数据，没有变更的数据则不需要重绘，大大的减少了浏览器渲染时间。
   
   diff算法
   ```
   
3. ##### react使用介绍

   ```jsx
   1、导入模块
   import React from 'react'; // 创建组件、虚拟DOM元素，生命周期
   import ReactDOM from  react-dom; // 把创建好的组件和虚拟DOM 放到界面上展示
   
   2、创建虚拟DOM元素
   const h1Ele = React.createElement(
       'h1',
       {
           id:'test',
           title:'dom标题'
       },
       '这是h1的内容部分'
   );
   参数1：创建元素节点
   参数2：是一个对象，当前dom元素的属性，默认为null
   参数3：子节点
   参数4：嵌套的子节点
   
   3、渲染虚拟DOM到页面上
   ReactDOM.render(
       h1Ele,
   	document.getElementById('app')
   )
   参数1：需要渲染的元素
   参数2：挂载真实的DOM节点
   
   以上是创建一个虚拟dom的最简单方式，但很明显这样创建项目不可取（不方便，当需要创建大量html元素时，会显得很吃力），所以jsx语法就运运而生。
   ```

4. ##### jsx语法

   ```jsx
   JSX是用来声明React当中的元素，React使用JSX来描述用户界面。
   1、jsx绑定值 {}
   const a = 123;
   <div>
   	{ a }
   </div>
   
   2、绑定标签属性
   const a = '这是标签的title'
   <p title={ a }></p>
       
   3、数组循环
   方法一：数组对象直接渲染
   const a = [
     <div className='test'>123</div>,
     <span>456</span>
   ]
   <header className="App-header">
       { a }
   </header>
   
   方法二：我们在拿到一个列表数据时，一般是一个json数组对象
   const a = [
       { name:'罗亮',age:31,id:1 },
       { name:'代倩',age:27,id:2 },
   ]
   let Arr = a.map(item => {
     return <div>{item.name }</div>
   })
   <header className="App-header">
       { Arr }
   </header>
   
   方法三：直接js渲染
   const a = [
       { name:'罗亮',age:31,id:1 },
       { name:'代倩',age:27,id:2 },
   ]
   <header className="App-header">
   	{ a.map(item => <div>{ item.name }</div>) }
   </header>
   
   注意：以上数组循环方法会提示一个错误--缺少key，因为react语法要求高效渲染dom，所以key必须（diff算法与vue类似，详情查看vue中关于v-for循环为什么必须要key部分）
   const a = [
       { name:'罗亮',age:31,id:1 },
       { name:'代倩',age:27,id:2 },
   ]
   <header className="App-header">
   	{ a.map(item => <div key={ item.id }><span>{ item.name }</span></div>) }
   </header>
   注意：key必须加在循环渲染的根节点dom上，例如上面加在span标签上，则依然会抛错！！！
   
   4、绑定class
   <div class="test"></div> // 错误，jsx无法识别
   <div className="test"></div> // 正确
       
   5、label for绑定
   <label for="foo"></label> // 错误，jsx无法识别
   <label htmlFor="foo"></label> // 正确
       
   6、使用JSX创建DOM元素的时候，所有节点必须包裹在一个根节点内（与vue的组件中，template元素有且只有一个根节点类似）
   
   7、使用jsx创建dom节点时，标签必须有闭环。eg: <div></div>或者<input type='test'/>
   ```

5. ##### react创建组件，以及组件传参

   ```jsx
   方法一：
   创建一个Test组件
   function Test(props){
     const list = props.list;
     return <div>{ list.map(item => <h1 key={ item.id }>{ item.name }</h1>) }</div>
   }
   function App() {
     const a = [
         { name:'罗亮',age:31,id:1 },
         { name:'代倩',age:27,id:2 },
     ]
     return (
       <div className="App">
         <header className="App-header">
           { a.map(item => <div key={ item.id }><span>{ item.name }</span></div>) }
         </header>
         <Test list={a}></Test>
       </div>
     );
   }
   注意：
   1、组件名称，首字母必须大写！！！
   2、父组件通过属性传递给子组件，子组件通过props参数接收。但参数为只读模式,意思是子组件无法修改父组件传递过来的值！
   function Test(props){
     const list = props.list;
     list = [] // 抛错，因为list = props.list是通过父组件传递过来的，子组件无法修改！
     return <div>{ list.map(item => <h1 key={ item.id }>{ item.name }</h1>) }</div>
   }
   
   方法二：
   方法一用于单个或者少量参数传递还行，大量数据使用就不太方便了，该方法提供一个对象传递
   function Test(props){
     console.log(props);
     return <div>123</div>
   }
   function App() {
     const a = { name:'罗亮',age:31,id:1 }
     const b = { hobby:'football' }
     let c = Object.assign(a,b);
     return (
       <div className="App">
         <Test {...c}></Test>
       </div>
     );
   }
   
   方法三：组件抽离成一个单独的jsx文件
   创建一个组件Test.jsx
   import React from 'react';
   function Test(props){
       console.log(props);
       return <div>这是一个单独的组件</div>
   }
   export default Test;
   在父组件中引入Test.jsx组件
   import Test from './Test.jsx';
   function App() {
     const a = { name:'罗亮',age:31,id:1 }
     const b = { hobby:'football' }
     let c = Object.assign(a,b);
     return (
       <div className="App">
         <Test {...c}></Test>
       </div>
     );
   }
   
   方法四：使用class创建
   创建一个Test.jsx组件
   import React from 'react';
   class Test extends React.Component{    
       render(){
           console.log(this.props);
           return <div>这是一个class组件</div>
       }
   }
   export default Test;
   在父组件中引入Test.jsx组件
   import Test from './Test.jsx';
   function App() {
     const a = { name:'罗亮',age:31,id:1 }
     const b = { hobby:'football' }
     let c = Object.assign(a,b);
     return (
       <div className="App">
         <Test {...c}></Test>
       </div>
     );
   }
   1、使用class创建的组件，必须要继承于React.Component
   2、组件内部必须要有一个render函数，且符合jsx语法的返回虚拟DOM
   3、使用class创建的组件，不需要接收传递过来参数，直接使用当前实例的this.props即可拿到参数
   4、通过class创建的组件，参数的readOnly为false，但是仍然无法修改组件传递过来的参数
   5、使用class创建组件，必须使用super()函数，因为该组件继承于React.Component
   
   对比function和class创建组件的优缺点：
   1、使用class创建的组件，有自己私有的数据。使用function创建的组件，只有props参数。没有自己的私有数据和生命周期函数。
   2、使用function创建的组件叫‘无状态组件’。使用class创建的组件叫‘有状态组件’（常用），他们之间最本质的区别就是：有无state、有无生命周期函数。
   无论哪一种方式传递的参数，都无法修改。
   
   组件的全局变量state:在组件中，如果需要一些组件私有的数据，以及做一些处理，使用state（类似vue的data）
   import React from 'react';
   class Test extends React.Component{
       constructor(){
           super();
           this.state = {
               name:'luoliang'
           }
       }
       render(){
           console.log(this.props);
           return <div>这是一个class组件，名称是 { this.state.name }</div>
       }
   }
   export default Test;
   注意：
   1、使用constructor函数时，必须使用super()函数，才能使用this。
   2、申明在state中的数据，都是可以读写的。
   ```

6. ##### 组件中的数据

7. 虚位以

