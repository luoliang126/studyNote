# vue笔记

1. ##### vue数组更新（add,delete）等操作，但视图不刷新，以及对象属相添加，视图不刷新问题！

   ```js
   数组可以使用以下方法解决
   1、example1.items = example1.items.filter(function (item) {
    return item.message.match(/Foo/)
   })
   2、使用深拷贝方式，最简单直接的JSON.stringify和JSON.parse转换一次，实现深拷贝
   
   对象可以使用以下方法解决
   var vm = new Vue({
        data:{
   	     a:1
        }
   })
   // `vm.a` 是响应的
   vm.b = 2
   // `vm.b` 是非响应的
   解决办法：
   1、使用深拷贝方式，最简单直接的JSON.stringify和JSON.parse转换一次，实现深拷贝
   2、Vue.set(vm.someObject, 'b', 2) 或者 this.$set(this.someObject,'b',2)
   3、this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
   ```

2. ##### vue-cli3发布一个npm包

3. ##### vue源码笔记 

   ```js
   1、双向绑定
   问：什么叫双向绑定？
   答：我们知道单向绑定，就是将一个数据
   data = {
       name:'luoliang',
       age:31
   }
   data.name 直接渲染到DOM界面上，这就叫单向绑定数据，或者就叫数据渲染。
   但是，当我们想修改数据 data.name='luoliang126'时，怎么办？
   第一种直接用js操作data.name='luoliang126',然后再操作DOM，document.getElementById('name').innerHTML = data.name。再次渲染。
   第二种放一个input标签在界面上，<input id="inputVal" type="text"/>监听change事件
   document.getElementById('inputVal').onchange = function(){
       let val = document.getElementById('inputVal').value();
       data.name = val;
   }
   上面两种方法都实现了效果，但是很明显这种操作适合一两个值的修改，当有大量需要编辑的时候就会显得臃肿，难以维护。所以这个时候我们就需要 数据<-->视图 间的双向绑定
   
   2、vue的双向绑定原理：数据劫持 + 发布中-订阅者模式
   通过Object.defineProperty()来劫持各个属性的setter,getter，在数据变动时发布消息给订阅者，触发相应的监听回调事件。
   ```

4. 虚位以待！！！
