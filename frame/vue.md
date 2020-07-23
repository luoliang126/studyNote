# vue笔记

1. ##### vue数组更新（add,delete）等操作，但视图不刷新，以及对象属相添加，视图不刷新问题！

   ```js
   数组可以使用以下方法解决
   1、
   example1.items = example1.items.filter(function (item) {
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
   
   3、vue强制刷新界面
   this.$forceUpdate();
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
   vue的双向绑定原理：数据劫持 + 发布中-订阅者模式
   通过Object.defineProperty()来劫持各个属性的setter,getter（后面会讲到），在数据变动时发布消息给订阅者，触发相应的监听回调事件。
   ```
   
4. ##### vue实现一个面包屑

   ```js
   方法一：
   1、定义路由，与平时路由没什么不一样，但必须添加meta，以及title。
   let routers = [
       { 
           path:'/',
           redirect:"/dashboard" 
       },
       { 
           path: '/dashboard',
           name: 'dashboard',
           component: dashboard,
           meta: {
               title: "面板选择页"
           }
       },
       {
           path:'/loginConfig',
           name:'loginConfig',
           component:loginConfig,
           title:'登录配置',
           meta:{
               title:'登录配置'
           },
           children:[
               { 
                   path:'/loginConfig/loginRouterConfig',
                   name:'loginRouterConfig',
                   title:'登录路由配置',
                   component:loginRouterConfig,
                   meta:{
                       title:'登录路由配置'
                   }
               },
               { 
                   path:'/loginConfig/loginGuideConfig',
                   name:'loginGuideConfig',
                   title:'登录引导页配置',
                   component:loginGuideConfig,
                   meta:{
                       title:'登录引导页配置'
                   }
               },
           ]
       }
   ]
   2、获取路由地址，并截取出路由层级
   getBreadcrumb () {
       this.itemList = this.$route.matched.filter(item => item.name);
   },
   返回的是一个路由数组。（区分了一级路由，二级路由，三级路由......等）
   eg:[
       {
           ......
           path:'/loginConfig',
           meta:{
               title:'登录配置'
           }
       },
       {
           ......
           path:'/loginConfig/loginGuideConfig',
           meta:{
               title:'登录引导页配置'
           }
       }
   ]
   3、遍历itemList，并绑定视图
   <el-breadcrumb separator="/">
       <el-breadcrumb-item>
           <span @click="jumpRouter('/')">
               首页
           </span>
       </el-breadcrumb-item>
       <el-breadcrumb-item v-for="(item,index) in itemList" :key="'breadcrumb'+index">
           <span @click="jumpRouter(item.path)">
               {{ item.meta.title }}
           </span>
       </el-breadcrumb-item>
   </el-breadcrumb>
   jumpRouter(path){
       if(path == '/'){
   		......
       }
       this.$router.push(path);
   }
   4、注意，公用的面包屑，应该实时监听router的变化
   watch:{
       $route(){
           this.getBreadcrumb();
       }
   }
   批注：这种方法的关键在于this.$route.matched.filter(item => item.name) 直接返回的是一个数组格式，且根据路由嵌套的方式，一次存入数组，方便我们取值绑定。
   ```

5. ##### vue面试常见问题？

   ```js
   1、vue在组件中的data为什么推荐使用函数 return的方式（直接对象data也可以）
   data:{
   	a:1
   }
   data(){
   	return{
   		a:1
   	}
   }
   this指向问题，在写只有一个组件的vue项目时，this指向的就是当前vue实例，所以用data对象和data函数都可以。但是在多个组件或者多个.vue文件时，每一个组件的this都应该指向当前实例组件的this，而不是这个构造vue的this，使用函数return的方式可以避免全局数据的污染。
   eg：
   publicVueComponent.vue
   <template>
   	<div>
       	这是一个公用组件
       </div>    
   </template>
   <script>
   export default {
   	data:{
   		a:1
   	}
   }    
   </script>
   如果我们在a组件中调用这个publicVueComponent.vue，并修改了a这个值this.a=2;
   那么我们在b组件中调用这个publicVueComponent.vue时，a的值同样被修改了。因为他们指向的都是同一个a
   所以推荐使用函数return的方式，this指向的是每次实例化这个publicVueComponent组件的实例，而不是这个构造组件。
   ```

6. 虚位以待！！！
