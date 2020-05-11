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
