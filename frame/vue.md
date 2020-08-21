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

5. ##### vue组件传参

   ```js
   1、父-子传递参数 props
   在father.vue中导入子组件child.vue后
   <child :data="data"></child>  // 可以绑定多个参数
   在child.vue中 定义一个props:['data']   //注意此处props是以数组的方式传递的，且为字符串。如果有多个则：['data','data1','data2']。
   props还可以以一个对象的方式接收，且可以调控
   props: {
       propA: Number,  //传递的参数propA只能为Number类型，如果不是number类型则抛错
       propB: [String, Number],  // 可以同为string和number
       propC: {                   // 参数propC必传且是字符串
           type: String,
   		required: true
       },
   	propD: {                 // 参数propD，可以不传（但默认值为100）
           type: Number,
           default: 100
       },
       propE: { // 数组/对象的默认值应当由一个工厂函数返回
           type: Object,
           default: function () {
               return { message: 'hello' }
           }
       },
   }
   child.vue中使用该data和数据一样使用。
   注意：通过props传参是单向的，即父-->子组件
   
   2、子-父传递参数，通过事件的传播方式传递参数
   在child.vue中自定义一个事件 this.$emit('childToFather',msg);
   在father.vue中监听事件 <child v-on:childToFather="listenToChild"></child>
   在methods中，添加监听到childToFather 的事件执行函数listenToChild(data),该方法中有一个data返回参数，里面就存放着msg对应的参数
   
   3、eventHub bus总线方式
   方法一：挂载data方式
   在入口main.js中添加数据模型
   new Vue({
       el: '#app',
       router,
       render: h => h(App),
       data: {    //添加的data数据模型
           eventHub: new Vue()
       }
   })
   组件中调用事件触发，通过this.$root.eventHub获取到此对象
   //调用$emit触发
   this.$root.eventHub.$emit('YOUR_EVENT_NAME', yourData)
   //调用$on监听
   this.$root.eventHub.$on('YOUR_EVENT_NAME',function(data){
       console.log(data)   //通过事件传递过来的参数
   })
   方法二：挂载prototype方式
   创建一个Bus.js文件
   import Vue from 'vue';
   export default new Vue();
   在main.js中绑定在vue原型上
   import Bus from 'Bus'
   Vue.prototype.Bus = Bus
   在界面上就可以使用
   this.Bus.$emit("doSearch", params); //触发事件
   this.Bus.$on("doSearch",function(params){......}) //监听事件，params为传递的参数
   注意：
   1、在组件中使用$on时，首先创建时会执行一次。如果在当前组件不销毁时，那么下次进入时就会执行2次，依次类推，不停的进入该组件、退出，就会不停的监听$on，解决办法是，在该组件销毁时取消监听组件销毁生命周期函数中取消监听，下次再进入时再创建监听。
   destroyed() {
   	this.$root.$bus.$off("reloadDetail")
   },
   2、以上两种方法都一样，只是书写方法不一致而已。适用于触发一次，从新渲请求接口等操作。Bus总线事件触发方法，建议少用，尤其是有大量数据需要传递时，建议使用vuex。（整个界面都是触发--监听很难维护,尤其是数据无法得到保证和确定）
                                 
   4、路由传参
   方法一：定义路由跳转传参
   <router-link :to="{ name:'game1', params: {num: 123} }">   
   //其中name:'game1',这个name属性就是定义路由中的name
   <router-link :to="{ path:'/hotel',params:{id:item.id} }">
   方法二：函数传参
   router.push({ name: 'user', params: { userId: 123 }})  
   // 记住此处只能用定义在路由index.js文件中，name属性对应的值。
   界面上通过this.$route就可以获取到这个params了
   方法三：
   this.$router.push({name:'parasetEdit',params:{pk_refinfo:'test',value:'test1'}}); // 传递
   this.$route.params.pk_refinfo // 接收
   方法四：
   this.$router.push({path:'/uapbd/paraset/edit',query:{pk_refinfo:'test',value:'test1'}}); // 传递
   this.$route.query.pk_refinfo  // 接收
   方法五：
   通过拼接路由地址+参数方式：http://localhost/isAttendMeeting?id=123
   然后在路由isAttendMeeting界面，使用this.$route.query.id来获取拼接的参数。
   两种方式的区别是query传参的参数会带在url后边展示在地址栏，params传参的参数不会展示到地址栏。
   需要注意的是接收参数的时候是route而不是router。两种方式一一对应，名字不能混用。
   
   5、$attrs和$listener方式（vue2.4版本以后）。
   描述：A-->B-->C组件传参（$attrs）
   组件A：
   <template>
       <div>
           <div>页面A</div>        
           <first-component :inputTex="inputTex" :content="content"></first-component>
       </div>
   </template>
   <script>
   import firstComponent from './page1/firstComponent.vue';
   export default {
       components:{
           firstComponent
       },
       data(){
           return{
               inputTex:'123',
               content:'1007',
           }
       }
   }
   </script>
   组件B：
   <template>
       <div>
           <div>组件B</div>
           <second-component v-bind="$attrs"></second-component>
       </div>
   </template>
   <script>
   import secondComponent from './secondComponent.vue';
   export default {
       props: {
           inputTex:{
               type:String,
               default:''
           }
       },
       components:{
           secondComponent
       },
       data(){
           return {
               name:'这是第一级组件'
           }
       },
       mounted:function(){
           console.log(this.inputTex);
       }
   }
   </script>
   组件C：
   <template>
       <div>
           组件C
       </div>
   </template>
   <script>
   export default {
       props:{
           content:{
               type:String,
               default:''
           }
       },
       data(){
           return {
               name:'这是第二级组件'
           }
       },
       mounted:function(){
           console.log(this.content)
       }
   }
   </script>
   说明：这种方式可以理解为漏勺的原理，A组件传递了inputTex和content两个参数，但B组件只用了inputTex参数，剩下的参数（content）直接给B组件中引用的C组件，当然C组件需要props去接收。注意点：A传递的inputTex和content，如果B组件用props全部接收了，则组件C一个值都拿不到（一个都没有漏下去），同理如果B组件一个都没取，则全部漏给了组件C。这种方式实际应用时根据需要所定。
                
   组件C-->B-->A 事件触发($listener)
   组件A：同时监听了event-first和event-second两个事件
   <template>
       <div>
           <first-component @event-first="eventFirst" @event-second="eventSecond"></first-component>
       </div>
   </template>
   <script>
   import firstComponent from './firstComponent.vue'
   export default {
       components:{
           firstComponent
       },
       methods:{
           eventFirst(data){
               console.log(data);
           },
           eventSecond(data){
               console.log(data);
           }
       }
   }
   </script>
   组件B：父子组件可以使用this.$emit()方法触发事件，但也可以使用this.$listeners()方式。重点来了如果是C组件想通过事件传值给A组件，怎么办？？？
   <template>
       <div>
           第一级组件
           <button @click="touchFirst">触发eventFirst</button>
           <button @click="touchSecond">触发eventSecond</button>
   
           <second-component v-on="$listeners"></second-component>
       </div>
   </template>
   
   <script>
   import secondComponent from './secondComponent.vue';
   export default {
       components:{
           secondComponent
       },
       mounted:function(){
           console.log(this.$listeners);
       },
       methods:{
           touchFirst(){
               // 这两种方法效果都是一样的
               this.$emit('event-first',1);
               this.$listeners['event-first']('1');
           },
           touchSecond(){
               this.$listeners['event-second']('2');
           }
       }
   }
   </script>
   组件C：我们看到B组件调用C组件使用了v-on="$listeners"，即把B组件所有的监听事件都传给了C组件，所以我们直在C组件中接触发事件，A组件就能收到！
   <template>
       <div>
           第二级组件
           <button @click="touchSecond">触发eventSecond</button>
       </div>
   </template>
   <script>
   export default {
       mounted:function(){
           console.log(this.$listeners)
       },
       methods:{
           touchSecond(){
               // 这里两种效果，都可以触发
               this.$emit('event-second',2);
               this.$listeners['event-second']('2');
           }
       }
   }
   </script>
   说明：这里的关键在于组件B，通过v-on="$listeners"把B组件所有的事件都传给了C组件，C组件就可以直接触发！！！
   
   6、自定义组件实现一个双向绑定 v-model
   问题描述：假如我们在书写一个组件的时候，只想得到里面的值。如<persona-component v-model="value"></persona-component>由于父--子组件props传参是单向的，即父组件修改值，子组件能拿到最新修改后的值。但是子组件将props传过来的值修改了，父组件却无法修改（双向绑定）。使用事件传递可以，但毕竟要$emit一次，并要监听。
   解决办法一：
   父组件：
   <template>
       <div>
       	<aa v-model="test"></aa>  // 组件中使用v-model
   	</div>
   </template>
   <script>
   import aa from './aa.vue'
   export default {
   	data () {
   		return {
   			test: ''
   		}
   	},
   	components:{
   		aa
   	}
   }
   </script>
   子组件：
   <template>
   	<div>
   		<ul>
   			<li>{{'里面的值：'+ msg}}</li>
               <button @click="fn2">里面改变外面</button>
   		</ul>
   	</div>
   </template>
   <script>
   export default {
   //使用model，这儿2个属性，prop属性说，我要将msg作为该组件被使用时（此处为aa组件被父组件调用）v-model能取到的值，
   //event说，我this.$emit('cc',修改的值)的时候，参数的值就是父组件v-model收到的值。
   	model: {
   		prop: 'msg',
   		event: 'cc'
   	},
       props: {
           msg: ''
       },
   	methods: {
           fn2 () {
               this.$emit('cc', this.msg+2)  // this.msg+2就是修改后的值
           }
       }
   }
   </script>
   方法二(推荐使用)：
   父组件与方法一的父组件一样
   子组件
   <template>
       <div>
       	<ul>
       	// 组件使用时有v-model属性，value初始传的‘what’ 不会被渲染，而是v-model绑定的test值被渲染，这儿value会被重新赋值为v-model绑定的test的值。
   			<li>{{'里面的值：'+ value}}</li>
   			<button @click="fn2">里面改变外面</button>
   		</ul>
   	</div>
   </template>
   <script>
   export default {
   	props: {
           value: {   // 必须要使用value，才能拿到数据
               default: '',
           },
       },
   	methods: {
           fn2 () {
               // 这儿必须用input发送数据，发送的数据会被父级v-model=“test”接受到，再被value=test传回来。
               this.$emit('input', this.value+2)
           }
       }
   }
   </script>
   注意：如果v-model是动态，随时可能变的情况下，应该用watch监听value的变化，在赋值！！！这样就能同步数据
   ```

6. ##### vue面试常见问题？

   ```js
   1、vue在组件中的data为什么推荐使用函数return的方式（直接对象data也可以）
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
   
   2、vue中为什么v-for循环时，建议加上key？
   首先看一下如果不加key会有什么影响？
   <ul>
       <li v-for="(item, i) in list">
           <input type="checkbox"> {{item.name}}
   	</li>
   </ul>
   list: [
       { id: 1, name: '李斯' },
       { id: 2, name: '吕不韦' },
       { id: 3, name: '嬴政' }
   ]
   第一步：checkbox选择，这个时候我们在界面上选中了第二条 ‘吕不韦’。
   第二步：list数据变更，
   list: [
       { id: 0, name: 'luoliang' },
       { id: 1, name: '李斯' },
       { id: 2, name: '吕不韦' },
       { id: 3, name: '嬴政' }
   ]
   就会发现视图变成四条，但是我们选中的checkbox变成了 ‘李斯’。
   解决办法：
   	就是在v-for循环的时候，加上唯一标识id
   	<ul>
           <li v-for="(item, i) in list" :key="item.id">
               <input type="checkbox"> {{item.name}}
           </li>
       </ul>
   总结：
       2.1、唯一性，为了vue 核心库中的diff算法能更高效的更新DOM。
       2.2、使得当前DOM内容和当前DOM状态相关联。
   
   3、vue的生命周期函数每个阶段干了什么事？以及父子组件生命周期加载的顺序？
   vue生命周期钩子函数：
   beforeCreate -> created -> beforeMount -> mounted -> beforeUpdate -> updated -> beforeDestroy -> destroyed
   beforeCreate：组件实例刚被创建，组件中的属性、data等都没有计算。
   created:组件实例创建完成，属性已经绑定，data等已经赋值，但DOM还未生产，所以$el还不存在
   beforeMount：虚拟DOM已经存在，但是正式DOM还未挂载。
   mounted：正式DOM已经生产，并挂载完成。
   beforeUpdate：组件更新之前
   updated：组件更新之后
   beforeDestroy：组件销毁之前
   destroyed：组件销毁之后
   activated,deactivated：这两个生命周期函数比较特别，是专为keep-alive组件使用的。
   activated：组件被激活时调用
   deactivated：组件被移除时调用
   
   4、vue父子组件加载顺序
   初次加载：
   父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
   数据修改渲染：
   beforeUpdate -> updated
   销毁过程：
   父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
   
   5、vue-router的hash、history模式的区别？
   hash模式在路由地址后面有一个‘#’，看着不太习惯。
   
   6、v-if和v-for为什么不建议在同意dom节点上使用？（虽然可以！）
   原因：v-for的执行顺序高于v-if，所以一般都是v-for循环后，再判断是否显示，存在一定的性能浪费（说实话，在我操作循环遍历时，几十几百条数据渲染，这点性能的浪费，可以忽略不计，这只是理论上的一种优化方式）。
   解决办法：可以在外层嵌套一个template或者其他dom标签，先v-if判定后，在v-for循环
   
   ```

7. ##### vue疑难杂症

   ```js
   1、setTnterval在vue中，clearInterval后，仍不停的触发？
   intervalTimeGetData(){
       clearInterval(this.intervalTime);
       this.intervalTime = setInterval(()=>{
           let params = {
               tenantCode:this.customerSelect,
           }
           this.getListData({
               pageIndex:this.currentPageIndex,
               startTime:this.timeSelect.startTime,
               endTime:this.timeSelect.endTime
           })
       },5000)
   },
   getListData(){
       ......
       this.intervalTimeGetData();
   }
   解决办法：将setInterval挂载到window对象上
   intervalTimeGetData(){
       clearInterval(window.intervalTime);
       window.intervalTime = setInterval(()=>{
           let params = {
               tenantCode:this.customerSelect,
           }
           this.getListData({
               workOrder:params,
               pageIndex:this.currentPageIndex,
               startTime:this.timeSelect.startTime,
               endTime:this.timeSelect.endTime
           })
       },5000)
   }
   ```

8. vue排序后，动画效果

   ```js
   官网上动画效果
   <transition-group name="flip-list" tag="ul">
       <li v-for="(item,index) in arr" :key="item">
           {{ item }}
   		<span @click="deleteItem(item,index)">删除该条</span>
   	</li>
   </transition-group>
   export default {
       data(){
           return {
               arr:[10,7,1,4,2,5,3],
               selectIndex:''
           }
       },
       methods:{
           // 排序
           arrReserve(){
               this.arr.sort(function(a,b){
                   return a-b;
               });
           },
           // 删除该条
           deleteItem(item,index){
               this.arr.splice(index,1);
           },
           // 根据输入排序
           inputSort(){
               if(!this.selectIndex){
                   return;
               }
               let currentNum = this.arr[this.selectIndex];
               this.arr.splice(this.selectIndex,1);
               this.arr.unshift(currentNum);
           }
       }
   }
   .flip-list-move {
       transition: transform 1s;
   }
   ```

9. 虚位以待！！！

