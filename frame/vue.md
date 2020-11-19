# vue笔记

1. ##### vue基础配置项

   ```js
   1、关于vue-cli搭建的项目，启动后ip访问，修改端口，自动运行？
       修改项目中config文件夹下的index.js文件
       1、修改host：'0.0.0.0'。 // 就可以实现192.168.1.102:8080访问（192.168.1.102为本机的ip地址）
       2、修改port：9090   // 就可以实现9090端口访问
       npm run dev的时候自动启动项目，不需要手动打开网页方法。在config文件夹下的index.js
       1、引入ip模块 const ip = require('ip').address()
       2、配置host:ip
       3、修改 autoOpenBrowser:true
   ```

2. ##### vue数组更新（add,delete）等操作、对象属相添加等，视图不刷新问题！

   ```js
   数组可以使用以下方法解决
   1、
   example1.items = example1.items.filter(function (item) {
   	return item.message.match(/Foo/)
   })
   2、使用深拷贝方式，最简单直接的JSON.stringify和JSON.parse转换一次，实现深拷贝
   
   3、对象可以使用以下方法解决
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
   4、vue强制刷新界面 this.$forceUpdate();
   
   4、在data中定义数据的时候userObject:{}，是一个空对象。在通过http拿到数据后，将返回结果绑定到userObject.userInfo = { name:luoiang },但视图上第一次绑值会出错 {{ userObject.userInfo.name }}.
   解决办法： this.$set(userObject,'userInfo',response)
   ```

4. ##### vue源码笔记 

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

5. ##### vue实现一个面包屑

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

5. ##### vue组件

   ```js
   vue组件扩展（公用方法抽离），混合mixins和继承extends 参考：http://www.jb51.net/article/131008.htm
   在一个单独的js文件
   1、在extends.js中
   let apiUrs = {
       query: '/api/tenants/roche/Disclaimer/GetMyLastUnread',
       read: '/api/tenants/roche/Disclaimer/Read'
   }
   export default {
       data() {
           return {
               checked:false,
               model: {
                   disclaimerId: "",
                   content: ""
               }
           }
       },
       created: async function() {
           this.queryAsync();
       },
       methods: {
           queryAsync: async function() {
               var self = this;
               var response = await this.api.post(apiUrs.query);
               if (!response.content) {
                   this.next();
                   return;
               }
               this.model = response.content;
               this.checked = true;
           },
           readAsync: async function(opinion) {
               if (!opinion) {
                   this.messageBox.alert("感谢您的反馈!").then(action => {
                       this.back();
                   });
                   return;
               }
               var response = await this.api
               .post(apiUrs.read, this.model);
               if (response) {
                   this.next();
               }
           },
           back: function() {
               this.$router.replace({
                   path: "/"
               });
           },
       }
   }
   2、在test.vue组件中
   import extends from "../controllers/extends.js"; //这里为了演示，实际中最好不用关键字
   export default {
       extends: extends, // mixins:[extends，......]多个
       components: {
       	appHeader
   	},
       data(){
           return{
               name:'luoliang'
           }
   	},
       ......
   };
   批注：
   1、这里以extends为例，mixins类似。
   2、当然也可以extends一个单独的.vue文件，里面的内容和js一样（这也是写服务组件的方式）。
   3、使用了mixins和extends继承之后，同一生命周期函数，会优先调用mixins和extends里面的生命周期钩子函数，组件本身的钩子函数也会对应调用。mixins和extends相比，extends触发的优先级更高（最优先执行）。
   4、同一data里面的值，mixins和extentd也会优先解析赋值，所以最后取值的时候，一般都是组件本身的data值（因为本身最后执行，把data里面原有的值覆盖了）
   
   vue中动态切换组件的方法
   <template>
       <div>
   	    <button @click="changeComponent">切换组件</button>
   	    <component v-bind:is="componentName"></component>
       </div>
   </template>
   <scirpt>
   import component1 from './component1'
   import component2 from './component2'
   export default{
       data(){
           return{
               componentName:'component1'
           }
       },
       components:{
           component1,
           component2
       },
       methods:{
           changeComponent(){
               this.componentName = 'component2'
           }
       }
   }
   </scirpt>
   一般，我们通过v-if,v-show等可以控制组件的隐藏和显示，就可以达到渲染的效果，但这样无疑会增加代码量，而这样操作可以提高代码效率，传递参数也可以通用一个componentParams，以及回调事件componentCallback，再分发事件。
   
   全局组件 创建和使用方法。
   <a href="https://www.cnblogs.com/670074760-zsx/p/7049806.html" target="_blank">参考文档1</a>
   <a href="https://blog.csdn.net/runonway/article/details/78998631" target="_blank">参考文档2</a>
   问题描述：我们创建好了组件indicator.vue,然后在需要引用的地方
   import Indicator from '../components/indicator.vue'  // 引入组件
   components:{    // 注册组件
       Indicator
   }
   <Indicator></Indicator>   // 使用组件。
   假如有10处的话，我们就需要import 10次，但如果我们像element组件一样，在全局main.js中引入一次。
   方法一：
   1、在components目录下，定义单独的组件文件夹 outComponent，并创建组件outComponent.vue 和index.js
   2、outComponent.vue 定义的组件（和平时定义组件一样）
   <template>
       <div>
           这是挂载的全局组件
   	</div>
   </template>
   3、index.js 中通过 install方法，将outComponent组件暴露出去
   import outComponent from './outComponent.vue'
   const allComponent = {
       // allComponent这就是后面可以使用的组件的名字，install是默认的一个方法
       // 只要在index.js中规定了install方法，就可以像一些公共的插件一样在main.js中使用Vue.use()
       install:function(Vue){
           Vue.component('allComponent',outComponent)
       }
   };
   export default allComponent;
   4、挂载全局 到main.js中
   import allComponent from './components/outComponent/index.js' //默认会去找index.js
   Vue.use(allComponent);
   5、这样在组件中就可以直接使用 <all-component></all-component>，而不需要import和注册了
   
   方法二：
   1、在components下创建一个allComponent.js文件
   import eg from '../components/eg.vue';  //引入需要注册的全局组件，并暴露出去
   ......
   export default (Vue)=>{
       Vue.component("Eg",eg);
       ......
   }
   另外一种写法：
   import eg from '../components/eg.vue';  //引入需要注册的全局组件，并暴露出去
   ......
   const allComponent = {
       install:function(Vue){
           Vue.component("Eg",eg);
           ......
       }
   };
   export default allComponent
   2、在main.js中
   import components from './components/allComponent.js';
   Vue.use(components);
   
   组件提供给使用者自定义的方法（slot插槽方式）。 参考：https://blog.csdn.net/kingov/article/details/78293384
   问题描述：
   我们写了一个head.vue的组件，样式已经固定了，但是用户在使用的时候，又想自己修改怎么办？
   解决办法：使用slot和name属性搭配可以做到，slot分发插槽，可以让组件的可扩展性更强。
   单个插槽
   // 父组件
   <template>
       <div id="app">
           <test-slot>
           	<span>我是父组件里的文字，但是我要被放到子组件里</span>
   		</test-slot>
   	</div>
   </template>
   <script>
   import testSlot from './components/testSlot'
   export default {
       components:{
           testSlot
       }
   }
   </script>
   //子组件
   <template>
       <div>
       	<h3>test-slot</h3>
           //父组件里的span会替换掉slot所以这里的123是看不见的
           //如果父组件在使用子组件testSlot的时候不在里面加内容则这里的slot会显示出来
   		<slot>123</slot>
   	</div>
   </template>
   
   多个插槽（根据name属性来区别插槽位置）
   // 父组件
   <template>
       <div id="app">
           <test-slot>
               // slot=one这个div会替换掉子组件里name="one"的slot标签
               <div slot="one">
                   <span>one</span>
                   <span>第一个</span>
               </div>
               //这个div没有用slot指定名字所以会替换掉子组件里没有name属性的slot标签
               <div>
                   <span>此div没有slot</span>
               </div>
               //slot=two这个div会替换掉子组件里name="two"的slot标签
               <div slot="two">
                   <span>two</span>
                   <span>第二个</span>
               </div>
   		</test-slot>
   	</div>
   </template>
   <script>
   import testSlot from './components/testSlot'
   export default {
       components:{
           testSlot
       }
   }
   </script>
   // 子组件
   <template>
       <div class="testSlot">
           <div class="noneSlot">
               <slot></slot>   // 没命名的slot插槽
   		</div>
           <div class="test-two">
               <slot name="two"></slot>  // 命名为name=two的插槽
           </div>
           <div class="test-one">
               <slot name="one"></slot> // 命名为name=one的插槽
           </div>
   	</div>
   </template>
   
   slot的作用域，父子通过slot-scope传递参数，参考：https://segmentfault.com/a/1190000012996217?utm_source=tag-newest
   描述：组件的内容渲染，都是通过数据传递的，即子组件把样式写好，父组件传递数据给子组件，从而渲染。但有一种情况，数组的渲染，父组件传递一个数组给子组件，但是子组件渲染的时候，每一条数据的样式是固定的，但如果用户需要在父组件自定义？子组件怎样把数据再扔给父组件呢？slot-scope就派上用场了，类似element的table表格中的<template slot-scope="row">......</template>
   // 子组件中，在插槽slot上使用 :data="data"传递数据给父组件，父组件再拿到数据，自定义渲染样式
   <div class="swiper colorF">
       <div class="swiper-left cursor-pointer" @click="goPre" v-if="isShowMoreIcon">
           <slot name="swiper-left">
               <i class="el-icon-caret-left font20"></i>
   	    </slot>
       </div>
       <div class="swiper-content">
           <div class="swiper-content-each" v-for="(item,index) in listData" :key="'swiper' + index">
               <slot name="swiper-content" :data="item">
                   <span>
   	                {{ item.name }}
                   </span>
   			</slot>
   		</div>
   	</div>
       <div class="swiper-right cursor-pointer" @click="goNext" v-if="isShowMoreIcon">
           <slot name="swiper-right">
               <i class="el-icon-caret-right font20"></i>
   	    </slot>
   	</div>
   </div>
   // 父组件中
   <swiper :listData="secondToolList" class="secondTool">
       <div slot="swiper-left">
           <i class="el-icon-caret-left font20 color3"></i>
   	</div>
   	// 这里的slot-scope="each"就是子组件返回的每一条数据
       <div class="font12 color3 cursor-pointer" slot="swiper-content" slot-scope="each">
           <div class="font20 text-align-center">
               <i :class="each.data.icon"></i>
   	    </div>
   		<div>
               {{ each.data.name }}
   		</div>
   	</div>
       <div slot="swiper-right">
           <i class="el-icon-caret-right font20 color3"></i>
       </div>
   </swiper>
   
   组件间的数据传递
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
   
   6、自定义组件实现一个双向绑定v-model，双向绑定还可以使用sync修饰符，详细内容查看sync部分
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
   
   函数式组件，vue没有类似react那样的jsx语法，想用函数式编程就会变得很鸡肋，因为数据不是双向的。
   什么叫函数式组件？
   我们可以把函数式组件想像成组件里的一个函数，入参是渲染上下文(render context)，返回值是渲染好的HTML片段。
   对于函数式组件，可以这样定义：
       Stateless(无状态)：组件自身是没有状态的
       Instanceless(无实例)：组件自身没有实例，也就是没有this
       内部没有任何生命周期处理函数
       轻量,渲染性能高,适合只依赖于外部数据传递而变化的组件(展示组件，无逻辑和状态修改)
       只接受props值
   由于函数式组件拥有的这两个特性，我们就可以把它用作高阶组件(High order components)，所谓高阶，就是可以生成其它组件的组件。
   新建一个functionComponent.vue
   export default {
       name: 'functional-button',
       functional: true,
       render(createElement, context) {
           return createElement('button', 'click me')
       }
   }
   注意：声明的functional:true // 必须！！！
   当然也可以使用模板template functional  // 与上面方法相同
   <template functional>
     <div class="cell">
       <div v-if="props.value" class="on"></div>
       <section v-else class="off"></section>
     </div>
   </template>
   <script>
   export default {
     props: ['value']
   }
   </script>
   ```

6. ##### vuex的使用

   ```js
   为什么要使用vuex？
   组件之间的作用域独立，而组件之间经常又需要传递数据。A为父组件，下面有子组件 B 和 C。A的数据可以通过 :totalNum="totalNum" 传递给 B 和 C，B和C通过props:['data'],获取数据。A可以通过v-on:event监听子组件的事件，获取到B和C返回的参数this.$emit('event',参数)。但当 B 需要操作 C 的数据就会比较麻烦，需要先传到 A，再通过A传到 C。如果项目比较小的话还好，越大的项目，涉及的组件通信就越多、越频繁，此时管理起来就会非常累，而且容易出错。在vue2中去掉了 $dispatch 和 $broadcast，传递参数就只有props和事件两种了。这就是 Vuex 的意义所在。它可以将数据置于单独的一层，并提供给外部操作内部数据的方法。
   注意：不是所有项目都适用vuex，比如小型项目或者组件交互较少的项目，使用bus总线的方式也是一种不错的选择。
   // vuex的基本使用（中型的项目推荐 单独建立一个store.js存放状态管理）
   1、在main.js中定义全局的store
   const store = new Vuex.Store({
       state: {
           count: 1
       },
       mutations: {
           increment (state) {   //将state传递进去，不然无法操作state
               state.count++
           }
       }
   })
   2、在全局vue中注入store
   new Vue({
       el: '#app',
       router,
       store,      //const 定义的store一定要注入全局的Vue中，这样每个单独的子组件才能获取到store
       template: '<App/>',
       components: { App }
   })
   3、在子组件中访问该状态（记住一定要在computed中通过计算属性获取，因为created和mounted只在组件加载的时候获取一次，如果是异步通过接口获取的话，还没获取到，组件就渲染了，随后也不会再获取。而用computed即可计算属性，从而实时获取）。
   computed:{
       count () {
           return this.$store.state.count
       }
   },
   注意：在获取store中的数据时，可以通过上面的方法，能实时获取，但是在获取本地存储数据时就不一定了。在获取本地存储的时候，用get方法才能实现实时。
   computed: {
       componentName: {
           get: function () {
               var componentId = this.iStorage.get("tenant") || 'simple';
               return (this.prefixComponent +"_"+ componentId);
           }
       }
   },
   4、修改全局store下state中count的值。既然可以用this.$store.state.count访问，那也可以用该方法修改？？？这是一种错误的方法。修改只能通过定义在mutations对象中的方法，如上面的increment方法，那子页面如何调用该方法？
   // 在子组件中：
   created:function(){
       this.$store.commit('increment', 10) //修改store中的值
   }
   // 在store中
   mutations: {
       increment (state, n) {
           state.count += n
       }
   }
   
   在建立大型项目的时候，这样使用vuex（需要引入模块modules）
   1、建立需要挂载vuex的总store.js文件
   // 引入各个子存储 js文件
   import identity from './modules/identity'
   import event from './modules/event'
   ......
   import Vue from 'vue'
   import Vuex from 'vuex'
   Vue.use(Vuex)
   export default new Vuex.Store({
       //建立模块管理
       modules: {
           identity: identity,
           event: event,
           ......
       }
   })
   2、在每个子存储模块中
   const identity = {
       state: {
           token: null,
           userId: null,
           grant: null,
       },
       mutations: {
           setGrantResult(state, value) {
               localStorage.setItem("smartx_token", JSON.stringify(value));
               this.token = value.access_token;
               this.userId = value.userId;
               this.grant = value;
               localStorage.setItem("token", this.token);
           }
       },
   }
   export default identity;
   3、在main.js中引入总的store.js即可
   new Vue({
       el: '#app',
       store,
       ......
   })
   4、调用方法时，直接this.setGrantResult(response);
   注意：获取store中的值，用this.$store.state.supplierId。使用store中的方法，用this.$store.commit("changeSupplierId",params);但这种获取方法和使用method方法，不是太方便。下面这种方法可以试试。mapMutations和mapGetters
   
   优化vuex使用方法
   1、在store的index.js中，引入getters
   import Vue from 'vue'
   import Vuex from 'vuex'
   import getters from 'store/getters'
   import permission from './modules/permission'
   import user from './modules/user'
   import venue from './modules/venue'
   import supplier from './modules/supplier'
   Vue.use(Vuex)
   export default new Vuex.Store ({
       modules: {
           permission,
           venue,
           user,
           supplier
       },
       getters,
   })
   2、定义getters.js(暴露出去的，就是我们要在界面上直接使用的store中的值。注意一定是getters.js这个文件，引入也是getters)
   export default {
       username: state => state.user.username, // state就是状态管理，user就是创建在modules中的user模块，username就是user模块中具体的一个值
       ......
   }
   3、界面使用
   import { mapMutations, mapGetters } from 'vuex'  // 引入mapMutations和mapGetters
   computed:{
       //state中的值
       ...mapGetters([
           'username'
       ])
   },
   methods:{
       // mutations中的方法
       ...mapMutations([
           'setUser'
       ]),
   }
   this.username //直接使用取值
   this.setUser({name:'luoliang',age:28}) // 直接使用修改user的值
   // 如果我们从接口获取到的数据需要存入store的话，那么建议将接口写在vuex的actions中。
   
   vuex状态持久化
   vuex虽然可以实现状态管理，但它毕竟是以定义一个全局变量实现，在app端还好，但在web端用户按F5或者刷新页面后，vuex中的数据会全部清空为初始值。解决办法：使用vuex-persistedstate 参考：https://www.cnblogs.com/lemoncool/p/9645587.html
   1、安装：npm install vuex-persistedstate  --save
   2、使用：在new Vuex.Store(......)中添加
   import createPersistedState from "vuex-persistedstate" // 引入vuex持久化
   export default new Vuex.Store ({
   	modules: {
   		user
   	},
       plugins: [createPersistedState({  // 配置持久化，以sessionStorage为例，localStorage和cookie同理
           storage:window.sessionStorage
       })],
   	getters
   })
   3、那么在调用user中的方法，修改值后，就会在sessionStorage中存储一个值，如果刷新的话，会自动去取sessonStorage中对应的值。如果想持久化一个 modules中的模块，请参考文档详细说明
   
   在一个独立的js文件中，怎样引用store？怎样获取store中的值？怎样修改store中的值？
   import Index from '@/store/index.js' // 引入store中的index.js（就是vuex的主文件）
   Index.orderId // 直接使用
   console.log(Index)中去寻找查询、修改的方法
   记住：引入的index.js一定要是export default new Vuex.Store({。。。。。。}) 挂载全局的那一个！
   
   vuex中触发事件修改state值，dispatch和commit的区别
   dispatch是异步操作
   存值
   this.$store.dispatch('listData',value);
   取值
   this.$store.getters.listData
   commit是同步操作
   存值
   this.$store.commit('listData',value);
   取值
   this.$store.state.listData
   ```

8. ##### vue路由

   ```js
   关于路由
   路由跳转的常见方式：
   1、常见用 <router-link to="/form">Form</router-link> 绑定跳转 "/form" 是定义路由时的path。等同于指令中的 <a v-link="{ path: '/home'}">Home</a>
   2、还有一种为函数式跳转 this.$router.push("/form") 来修改 url，完成跳转。
   3、replace属性：页面切换时不会留下历史纪录 <router-link :to="/home" replace></router-link>
   4、tag属性：有tag属性的router-link会被渲染成相应的标签
   <router-link :to="/home" tag="li">Home</router-link>
   渲染结果：<li>Home</li>
   5、路由的嵌套：参考：http://blog.csdn.net/github_26672553/article/details/54861174
   在当前路由Login下，再定义子路由。（可以再定义children继续嵌套下去）
   	path: '/Login',
       name: 'Login',
   	component: Login,
   	children:[
           { path: '/Login/Hello', component: Hello},
           { path: '/Login/Hello', component: Hello}
       ]
   6、路由链接的激活状态
   //注意，路由默认的激活class为：router-link-exact-active。此处我们单独再添加一个headItemActive的class。
   linkActiveClass:'headItemActive',
   routes: [
       {
           path: '/',
           redirect: '/Login'   //初次加载的路径
       },{
           path: '/Login',
           name: 'Login',
           component: Login
       },{
           path: '/Hello',
           name: 'Hello',
           component: Hello,
           redirect: '/page3/page3_1',
           children:[{
               path: '/page3/page3_1',
               name:'page3_1',
               component: page3_1,
           },{
               path: '/page3/page3_2',
               name:'page3_2',
               component: page3_2,
           }]
       }
   ]
   界面使用时：
   <router-link to="/Login">登陆界面</router-link>
   <router-link to="/Hello">首页</router-link>
   渲染出来是这样的：
   <a data-v-6a80d930 href="#/Login" class="router-link-exact-active headItemActive">登陆界面</a> 
   所以一般定义当前路由激活状态用router-link-exact-active写css样式就可以了，这里只是为了说明可以单独定义headItemActive。
   注意：如果是嵌套路由的话，就有一级二级同时激活状态，此时二级路由激活状态变成了class="router-link-exact-active headItemActive"，
   但是一级路由状态，依然只有class="headItemActive"。所以我们需要重新定义一个激活状态的active。更多层的嵌套待续......
   说明：如果是使用this.$router.push()方法进入的路由跳转，那么可以通过设置 activeIndex，即点击哪一个来区别，首次加载可以匹配路由地址，
   包含某个字符，从而实现activeIndex对应的值，然后再让标题栏成选中状态。
   7、路由的拆分（大型项目时，根据需要使用）
   //在router下的index.js中，引入配置路由list
   import routerConfig from './routerConfig'
   const router = {
       mode: 'history',
       base: __dirname ,
       routes: routerConfit,   //通过routes导入路由文件
   }
   //在routerConfig.js文件中建立各种路由
   const routerConfig = [
       {
           path: '/login',
           component: resolve => require(['登录路径.vue'],resolve),
           meta: {noCheckSession: true}
       },{
           path: '/',
           component: resolve => require(['通用的主框架.vue'],resolve),
           children: [{
               path: '/team',
               name: 'team',
               component: resolve => require(['对应的路径.vue'],resolve)
           }]
       },{
           path: '/Home',
           name: 'Home',
           component: Home,
       }
   ]
   export default routerConfig;
   8、路由守卫
   方法一：全局监听
   监听路由切换beforeEach切换前执行，afterEach切换成功后执行（此方法可以监听路由跳转时的函数，例如loadingBar加载条等）
   const router = new Router({
       mode: "history",
       routes: [{
           path: '/',
           redirect: '/Home' //初次加载的路径
       },{
           ......
       }]
   })
   router.beforeEach((to, from, next) => {  //to,from,next自行console看，根据需要调整
       iView.LoadingBar.start();
       ......
   	next() // 注意一定要next，不然路由跳转过去获取不到router，传一个next(false)就打断跳转。
   });
   router.afterEach((to, from, next) => {
       iView.LoadingBar.finish();
       ......
   });
   方法二：路由组件中，单个监听。(该方法的执行顺序大于 方法三)
   const router = new VueRouter({
       routes: [
           {
               path: '/foo',
               component: Foo,
               beforeEnter: (to, from, next) => {
                   <!--注意：必须使用next 函数，否则不会进入该组件-->
                       next()
               }
           }
       ]
   })
   方法三：单个组件监听
   在某个具体---路由组件 内部
   beforeRouteEnter (to, from, next) {
       // 在渲染该组件的对应路由被 confirm 前调用
       // 不！能！获！取！组件实例 `this`
       // 因为当守卫执行前，组件实例还没被创建
       如果要在这里向当前路由组件传值的话使用
       方法一：next回调函数，其实是在当前vue组件mounted之后才会执行，所以如果在mounted里定义 this.age = 28 时，最后依然会被修改为this.age = 18
       next(vm => {
           console.log(vm); // vm 就是当前组件的实例相当于组件里的this
           vm.name = 'luoliang';
           vm.age = 18;
       })
       方法二：往路由地址中添加参数，因为这时路由地址还未跳转，接收参数就直接在路由地址去拿
       to.query.content = result.content; // 像路由地址中添加content
       next();
       获取时，只需要在created或者mounted之后，通过this.$route.query.content获取
       方法三：也可以使用sessionStorage等本地存储，这里就不举例了
   },
   beforeRouteUpdate (to, from, next) {
       // 在当前路由改变，但是该组件被复用时调用
       // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
       // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
       // 可以访问组件实例 `this`
   },
   beforeRouteLeave (to, from, next) {
       // 导航离开该组件的对应路由时调用
       // 可以访问组件实例 `this`
   }
   注意：使用beforeRouteEnter时，如果加上async 函数，还可以实现阻塞，我们可以在这里操作 首页最先加载的内容，后面的内容会在此等待这个操作结束
   9、修改vue-router地址栏中的 “#”
   在vue-router中，地址栏一般是这样的：http://localhost:8080/#/Home/HomePage
   在创建路由时添加 mode:"history"，属性就可以取消“#”
   10、路由传递参数 ：
   参考文档：
   https://segmentfault.com/q/1010000009749320/a-1020000009749399
   https://blog.csdn.net/wojiaomaxiaoqi/article/details/80688911
   <router-link :to="{ name:'game1', params: {num: 123} }">   //其中name:'game1',这个name属性就是定义路由中的name
   <router-link :to="{ path:'/hotel',params:{id:item.id} }">
   函数传参：
   this.$router.push({ name: 'user', params: { userId: 123 }}) 
   界面上通过this.$route就可以获取到这个params了
   方法一：
   // 记住，此处的name是，定义在路由index.js文件中，某个具体路由的name值，而不是路由组件中的name值
   this.$router.push({name:'parasetEdit',params:{ pk_refinfo:'test',value:'test1' }}); // 传递
   this.$route.params.pk_refinfo // 接收
   方法二：
   this.$router.push({path:'/uapbd/paraset/edit',query:{pk_refinfo:'test',value:'test1'}}); // 传递
   this.$route.query.pk_refinfo  // 接收
   通过拼接路由地址+参数方式：http://localhost/isAttendMeeting?id=123,然后在路由isAttendMeeting界面，使用this.$route.query.id来获取拼接的参数。
   两种方式的区别是query传参的参数会带在url后边展示在地址栏，params传参的参数不会展示到地址栏。需要注意的是接收参数的时候是route而不是router。两种方式一一对应，名字不能混用。
   11、关于用户权限，加载不同路由组件，addRouter方法。（查看smartx-webconsole-supplier站点）
   问题描述：一个应用有A,B两种权限操作，A和B不能访问对方的路由页面。在定义路由时可以做到，但如果在浏览器中手动输入路由地址依然可以访问？
   解决办法：使用vue-router中的addRouter()动态添加路由方法。参考：https://www.jb51.net/article/139303.htm
   第一步：定义路由，跟平时一样，不过基础路由很少，只有登陆注册报错等几个简单的路由。（因为登陆成功后，会根据一个返回值来判断动态添加的路由）
   import Vue from 'vue'
   import Router from 'vue-router'
   import index from '@/pages/index.vue'
   Vue.use(Router)
   const router = new Router({
       mode:'history',
       routes: [
           {
               path: '/',
               name: 'index',
               component: index
           },
           {
               path:'*',  // 匹配路由，当手动输入地址栏中的路由没有时，统一跳转到指定component
               component: (resolve) => require(['@/pages/error.vue'], resolve),
           }
       ]
   })
   router.beforeEach((to, from, next) => {
       if (to.meta.title) {  // 修改网页title
           document.title = to.meta.title
       }
       next();
   });
   router.afterEach((to, from, next) => {
       // console.log('after',router);
   });
   export default router;
   第二步，定义一个，拥有多个路由文件（各种操作权限的所有路由）的routerList.js
   // V2：场地供应商
   // V4：车辆、人员供应商
   // V16：制作物、设备供应商
   const routes = {
       'V2':[{
           path: '/supplier',
           name: 'supplier',
           component: (resolve) => require(['@/pages/supplier.vue'], resolve),
           children: [
               {
                   path: '/supplier',
                   redirect: '/supplier/supplier-list'
               },{
                   path: '/supplier/supplier-list',
                   name: 'supplier-list',
                   component: (resolve) => require(['@/pages/supplier/supplierList.vue'], resolve),
                   meta: {
                       title: '招标大厅'
                   }
               }
           ]
       },{
           path:'*',  // 匹配路由，当手动输入地址栏中的路由没有时，统一跳转到指定component或者redirect重定向
           redirect: '/supplier'
       }],
       'V4':[{
           path: '/supplier',
           name: 'supplier',
           component: (resolve) => require(['@/pages/supplier.vue'], resolve),
           children: [
               {
                   path: '/supplier',
                   redirect: '/supplier/supplier-list'
               },{
                   path: '/supplier/supplier-list',
                   name: 'supplier-list',
                   component: (resolve) => require(['@/pages/supplier/supplierList.vue'], resolve),
                   meta: {
                       title: '招标大厅'
                   }
               }
           ]
       },{
           path:'*',  // 匹配路由，当手动输入地址栏中的路由没有时，统一跳转到指定component或者redirect重定向
           redirect: '/supplier'
       }],
       'V16':[{
           path: '/production',
           name: 'production',
           component: (resolve) => require(['@/pages/production.vue'], resolve),
           children: [
               {
                   path: '/production',
                   redirect: '/production/order-manage'
               },{
                   path: '/production/order-manage',
                   name: 'order-manage',
                   component: (resolve) => require(['@/pages/production/productionManage.vue'], resolve),
                   meta: {
                       title: '订单管理',
                   }
               }
           ]
       },{
           path: '/add-order',
           name: 'add-order',
           component: (resolve) => require(['@/pages/production/addOrder.vue'], resolve),
           meta: {
               title: '增补订单',
           }
       },{
           path:'*',  // 匹配路由，当手动输入地址栏中的路由没有时，统一跳转到指定component或者redirect重定向
           redirect: '/production'
       }]
   }
   export default routes;
   第三步:根据login登陆后，有不同的类型或者id等来区别，然后直接往路由中添加对应的V2,V4或V16。
   this.$router.addRoutes(routerList['V16']); 此时的路由就变成了V16对应的路由。
   注意：1、该方法要在路由跳转前就使用。
   	2、在切换账号权限的时候，需要路由重定向，清除不同账号权限对应的cookie，storage等
       
   12、路由地址错误或者丢失：当用户输入地址栏的路由，在我们路由配置中找不到时，那么直接强行跳转到我们指定的路由地址
   routes: [{
       path: '/',
       redirect: '/Login'   //初次加载的路径
   },{
       path: '/Login',
       name: 'Login',
       component: Login,
   },{
       path: '/Hello',
       name: 'Hello',
       component: Hello,
   },{
       path: '/regist',
       name: 'Regist',
       component: Regist,
   },{
       path:'*', // 匹配路由，当手动输入地址栏中的路由没有时，统一跳转到 '/regist'
       redirect:'/regist'
   }]
   
   13、在组件中监听路由用$route
   watch:{
       '$route':function(newVal,oldVal){
           console.log(newVal);  //路由信息
           this.routerList = newVal.matched;
       }
   },
   
   14、关于路由的懒加载：当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了
   参考：https://router.vuejs.org/zh/guide/advanced/lazy-loading.html
   	https://blog.csdn.net/wp_boom/article/details/78799237
   方法一：
   const routers = [
       {
           path: '/',
           name: 'index',
           component: (resolve) => require(['./views/index.vue'], resolve)
       }
   ]
   方法二：
   const Index = () => import(/* webpackChunkName: "group-home" */  '@/views/index')
   const routers = [
       {
           path: '/',
           name: 'index',
           component: Index
       }
   ]
   方法三：
   const Index = r => require.ensure([], () => r(require('./views/index')), 'group-home');
   const routers = [
       {
           path: '/',
           name: 'index',
           component: Index
       }
   ]
   注意：
   1、方法一常用，单独定义直接应用，和平时定义组件一样
   2、方法二，三中的‘group-home’ 是把组件按组分块打包, 可以将多个组件放入这个组中
   
   15、路由切换动画 参考：https://www.cnblogs.com/szyblogs/p/7069577.html
   在做app路由跳转的时候，右侧出来前进，左侧出来后退
   在app.vue中，或者路由绑定界面上
   <transition :name="transitionName">
       <router-view class="child-view"/>
   </transition>
   .child-view {
       position: absolute;
       width: 100%;
       transition: all 0.4s;
   }
   .slide-left-enter,.slide-right-leave-active {
       opacity: 0;
       -webkit-transform: translate(100%, 0);
       transform: translate(100%, 0);
   }
   .slide-left-leave-active,.slide-right-enter {
       opacity: 0;
       -webkit-transform: translate(-100%, 0);
       transform: translate(-100%, 0);
   }
   <!--监听路由变化，根据isBack来判断是slide-left还是slide-right,从而执行相应的动画-->
   watch: {
       $route(to, from) {
           let isBack = this.$router.isBack;
           if (isBack) {
               this.transitionName = "slide-right";
           } else {
               this.transitionName = "slide-left";
           }
           this.$router.isBack = false;
       }
   },
   <!--重写 vue-router中的back方法-->
   Router.prototype.back = function (params) {
       this.isBack = params || true;
       window.history.go(-1)
   }
   
   16、路由界面持久化
   参考文档：
   <a href="https://segmentfault.com/a/1190000012083511?tdsourcetag=s_pcqq_aiomsg" target="_blank">文档一</a>
   <a href="https://www.jianshu.com/p/e565cfef48e7" target="_blank">文档二</a>
   <a href="https://www.cnblogs.com/goloving/p/9256212.html" target="_blank">文档三</a>
   问题一：
   问题描述：我们在做界面的时候，比如tab界面，我们切换tab标签的时候，如果该tab还未加载，那么进入时在created里添加http请求数据，然后渲染，但是如果该tab标签加载过，我们再进来依然会发http，如果我们让该tab标签持久化（缓存），那么再次进入时就可以减少http请求。
   实现方法：
   1、路由缓存
   <keep-alive>
       <router-view> </router-view>
   </keep-alive>
   这里所有的路由界面都会被缓存，但有些时候我们仅仅是需要个别路由缓存，其他路由不需要缓存
   方法一：
   <keep-alive>
       <router-view v-if="$route.meta.keepAlive"> </router-view>
   </keep-alive>
   <router-view v-if="!$route.meta.keepAlive"> </router-view>
   routes:[
       {path:'/test1',component:test1,meta:{keepAlive:true}},
       {path:'/test2',component:test2,meta:{keepAlive:false}}
   ]
   这样test1路由会使用keep-alive，test2路由不会使用keep-alive
   方法二：使用属性inlcude/exclude
   <keep-alive exclude="test1">
       <router-view></router-view>
   </keep-alive>
   注意：
   1、include是包含，exclude是除去......之外的（特别注意：如果不需要任何缓存时，includes的值是' ',中间有一个空格，而不是简单的空字符串！！！）
   2、记住加name属性，不是router中的name属性，而是在test1组件当中的name值。
   3、切记只是不会执行created和mounted里面的内容。
   4、如果需要缓存多个界面include="test1,test2,test3......"。
   5、如果存在多个路由嵌套时，最好在每个<router-view>外层都加上<keep-alive :include="你需要缓存的界面，多个以逗号隔开">
   问题二：
   问题描述：界面可以被缓存，但怎样动态的设置取消缓存？
   Form01   ->  Form02（该页面的信息需要被缓存）   ->  Form03
   Form03 -> 返回Form02（信息已经被缓存）-> Form01 
   Form01  ->  Form02（信息被缓存，但是不想要这个缓存的）   ->  Form03
   解决办法：
   方法一：使用beforeRouteEnter和beforeRouteLeave
   在需要缓存的界面中添加
   beforeRouteEnter(to,from,next){
       // 进入 当前组件的meta设置为true（需要缓存）
       to.meta.keepAlive = true;
       next();
   },
   beforeRouteLeave(to, from, next) {
       //离开 如果是去venueList，就缓存当前界面，
       //离开 如果是返回list就取消缓存
       if (to.name == 'VenueList') {
           from.meta.keepAlive = true;
       } else {
           from.meta.keepAlive = false;
           this.$destroy();
       }
       next()
   },
   方法二：使用includes或者exclude，动态设置值
   <keep-alive :include="includes">
   	<router-view></router-view>
   </keep-alive>
   includes使用vuex、cookie、session存储能保证实时监听即可。
   特别切记，动态设置时，includes初始化为' '，中间有一个空格，而不是简单的空字符串，如果是空字符串可能会缓存所有！！！
   问题三：
   问题描述：路由被缓存后，只是created和mounted生命周期不会执行，但重新唤醒和离开（不是销毁）时，想做一些操作怎么办？
   使用keep-alive会多出两个生命周期函数 activated、deactivated。
   activated：在组件被激活时调用，在组件第一次渲染时也会被调用，之后每次keep-alive激活时被调用。
   deactivated：在组件被停用时调用（是离开当前缓存路由界面，而不是销毁）。
   ```

9. ##### vue动画效果

   ```js
   官网上动画效果--排序
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
   备注：这种实现方式，能达到要求，但动画效果始终有点别扭。
   解决办法：
   ```

10. ##### vue在使用UI组件（如element等），怎样修改单个组件里的局部样式？

   ```css
问题描述：在vue的项目开发中，你若是设置了style的scoped属性，会发现针对ui框架的css重写无法生效，若是把style的scoped去掉，样式倒是可以重写，但这就把重写的样式提升为了全局样式，会影响到其它组件的样式，而你只是想重写当前组件的样式？
解决办法:
1、未使用css预处理语言
<div class="test">
	<el-alert title="成功提示的文案" type="success"></el-alert>
</div>
<style scoped>
.test >>> .el-alert__title{
    color:red;
}
</style>

2、使用预编译css如：less，sass等
<div class="test">
	<el-alert title="成功提示的文案" type="success"></el-alert>
</div>
<style lang="less" scoped>
.test{
    /deep/.el-alert__title{
        color:red;
    }
}
注意：以上两种方法都是外层嵌套样式修改，必须要有一个外层选择器如：class="test"。

elementUI中表格错位，优化方案
.el-table th.gutter{
    display: table-cell;
}
   ```

11. ##### vue中 sync修饰符的作用

    ```js
    vue组件传参，数据流都是单向的。
    <child-component :father-num="test"></child-component>
    test值修改后，child-component组件中的props属性接收到的father-num就会变更。
    但是假如child-component中想修改father-num就必须借助event事件的模式，通过方法传参给父组件，父组件监听到event事件后，修改father-num。从而实现双向数据修改（双向绑定），当然也可以实现一个组件的v-model数据的双向绑定。
    
    // 父组件
    <el-button @click="openDialog">外部修改</el-button>
    <div>
        {{ isShowDailog }}
    </div>
    <test-component :isShowDailog.sync="isShowDailog"></test-component>
    data(){
        return {
            isShowDailog:false,
        }
    },
    methods:{
        openDialog(){
            this.isShowDailog = true;
        }
    }
    // 子组件testComponent
    <p> sync测试 {{ isShowDailog }} </p>
    <div>
        <el-button @click="eidtDialog">内部修改</el-button>
    </div>
    props:{
        isShowDailog:{
            type:Boolean,
            default:false
        },
    },
    eidtDialog(){
        this.$emit('update:isShowDailog',false);
    }
    // 当点击父组件中的 ‘外部修改’ 按钮后，发现父组件中的isShowDialog和子组件中的isShowDialog都同时修改了。（单向数据流）
    // 当点击子组件中的 ‘内部修改’ 按钮后，发现父组件中的isShowDialog和子组件中的isShowDialog都同时修改了。（双向绑定）
    官网的说法是：
    <comp :foo.sync="bar"></comp>在编译时会被解释成
    <comp :foo="bar" @update:foo="val => bar = val"></comp>
    使用sync会自动帮我们实现一次监听，并更新值
    ```

12. ##### vue指令directive和过滤器filter

    ```js
    指令：directive
    1、vue自定义指令的基础用法
    //属性指令
    <div v-my-directive></div>
    Vue.directive('my-directive',function(el,binding,vnode,oldVnode){
        console.log(el);    //el：指向调用的DOM元素：<div></div>，可以用来操作DOM
        console.log(binding);  //binding：指的是引用指令my-directive中所传递的参数等信息
        console.log(vnode);   // vnode中的context可以取当前环境的this
        console.log(oldVnode);
    })
    //元素指令
    <my-directive></my-directive>
    Vue.elementDirective('my-directive',function(el,binding,vnode,oldVnode){
        console.log(el);    //el：指向调用的DOM元素：<div></div>，可以用来操作DOM
        console.log(binding);  //binding：指的是引用指令my-directive中所传递的参数等信息
        console.log(vnode);
        console.log(oldVnode);
    })
    //详解
    el: 指令所绑定的元素，可以用来直接操作 DOM 。
    binding: 一个对象，包含以下属性：
        name: 指令名，不包括 v- 前缀。
        value: 指令的绑定值， 例如： v-my-directive="1 + 1", value 的值是 2。
        oldValue: 指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
        expression: 绑定值的字符串形式。 例如 v-my-directive="1 + 1" ， expression 的值是 "1 + 1"。
        arg: 传给指令的参数。例如 v-my-directive:foo， arg 的值是 "foo"。
        modifiers: 一个包含修饰符的对象。 例如： v-my-directive.foo.bar, 修饰符对象 modifiers 的值是 { foo: true, bar: true }。
    vnode: Vue 编译生成的虚拟节点，查阅 VNode API 了解更多详情。（可以在vnode.context中查看vue的实例对象即this）
    oldVnode: 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
    
    2、vue自定义指令的分阶段用法
    Vue.directive('my-directive', {
        bind: function(el,binding){   //只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作
            //做绑定的准备工作
            //比如添加事件监听器，或是其他只需要执行一次的复杂操作
        },
        inserted: function(){
            //被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）
        },
        update: function(){
            //根据获得的新值执行对应的更新
            //对于初始值也会调用一次
        },
        componentUpdated: function(){
            //被绑定元素所在模板完成一次更新周期时调用
        },
        unbind: function(){
            //做清理操作
            //比如移除bind时绑定的事件监听器
        }
    })
    
    3、全局指令和局部指令
    全局指令：Vue.directive(......)指令的位置，应该放在new Vue(......)这个实例之前，切记否则会抛错！
    Vue.directive("modelDirectiv",function(el,binding){
        ......
    })
    new Vue({
        el: '#app',
        router,
        template: '<App/>',
        components: { App }
    })
    当然也可以创建一个js文件,然后再把他引入到main.js中即可。
    import Vue from 'vue';
    Vue.directive("directiveName1",function(){
        ......
    })
    Vue.directive("directiveName2",function(){
        ......
    })
    再到main.js中，引入该js文件，记住在new Vue(......)实例化之前。
    
    局部指令：
    new Vue({
    	......
    	directives:{
    		dirName:{
                bind: function(el,binding){   //只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作
                    //做绑定的准备工作
                    //比如添加事件监听器，或是其他只需要执行一次的复杂操作
                },
                inserted: function(){
                    //被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）
                },
                update: function(){
                    //根据获得的新值执行对应的更新
                    //对于初始值也会调用一次
                },
                componentUpdated: function(){
                    //被绑定元素所在模板完成一次更新周期时调用
                },
                unbind: function(){
                    //做清理操作
                    //比如移除bind时绑定的事件监听器
                }
    		}
    	}
    });
    也可以在每个.vue文件中使用
    data(){
        return{
            ......
        }
    },
    method:{
        ......
    },
    directives:{
        dirName:{
            ......
        }
    }
    注意：当然也可以创建一个js文件
    directives = {
        dirName:{
            bind: function(el,binding){
                ......
            },
    	}
    }
    然后在main.js中引入该js文件
    import directives from './assets/directive.js'
    new Vue({
        ......
        directives:directives
    })
    
    4、vue中输入验证。
    封装一个指令的方式，vue指令的方法，详情查看directive
    Vue.directive('inputNumberFilter', {
        bind: function(el) {
            el.handler = function() {
                //先把非数字的都替换掉，除了数字和.
                el.value = el.value.replace(/[^\d.]/g, "");
                //必须保证第一个为数字而不是.
                el.value = el.value.replace(/^\./g, "");
                //保证只有出现一个.而没有多个.
                el.value = el.value.replace(/\.{2,}/g, ".");
                //保证.只出现一次，而不能出现两次以上
                el.value = el.value.replace(".", "$#$").replace(/\./g, "").replace("$#$", ".");
                // //如果第一位是负号，则允许添加  如果不允许添加负号 可以把这块注释掉
                // if (t == '-') {
                //   obj.value = '-' + obj.value;
                // }
            }
            el.addEventListener('input', el.handler)  / / 绑定事件，keyup，keydown等
        },
        unbind: function(el) {
            el.removeEventListener('input', el.handler)  // 移除事件绑定
        }
    })
    使用方法时（当然也可以监听input的input事件，实现这一个功能）
    <input type="text" v-inputNumberFilter v-model="test"/>
    
    过滤器：filter
    1、局部组件中使用filter的方法
    methods:{
        ......
    },
    filters: {
        capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        }
    }
    
    2、全局注册filter
    import Vue from 'vue'
    Vue.filter('filter1', function (value) {
        // 返回处理后的值
        return value - 1;
    })
    在具体组件中使用filter
    <div>
        {{ number | filter1 }}
    </div>
    
    3、全局注册filter时，像上面一个一个注册，不是太友好，优化方案：
    定义一个全局filter.js(用于定义所有的filter)
    const filter1 = function(value){
        return value.slice(0,10);
    }
    const filter2 = function(value){
        return value.slice(0,10);
    }
    ......
    export {
        filter1,
    	filter2
    	......
    }
    在main.js中引入所有的filters并批量注册
    import * as filters from './assets/js/filters.js';
    Object.keys(filters).forEach(key => {
        Vue.filter(key, filters[key])
    })
    
    4、filter传递多个参数
    一般使用{{ num | filterNum }} 默认参数为num
    {{ num | filterNum(params1,param2......) }} // 第一个参数为num，第二个为params1，第三个为params2......
    
    5、v-model中不能使用filter，即input，textarea中实现双向绑定的地方不能使用filter，因为filter只是展示数据的过滤，不是编辑数据的功能。
    ```

13. ##### vue 服务类组件，动态创建组件，添加到对应DOM（又名函数式编程）

    ```js
    动态创建组件，并通过js方法渲染到界面上。参考:https://www.cnblogs.com/stoneniqiu/p/6877460.html
    问题描述：我们在创建组件时，一般会定义一个indicator.vue的组件。
    import Indicator from '../components/indicator.vue'  // 引入组件
    components:{    // 注册组件
        Indicator
    }
    <Indicator></Indicator>   // 使用组件。
    但是，我们在js文件中怎样调用该组件，例如：http请求拦截和响应拦截时怎样调用？动态创建插入组件就很有必要了，即服务类组件，虽然vue并不提倡。
    
    方法一：
    1、首先定一个组件indicator.vue 和平时的组件没多大区别
    <template>
        <div v-show="visible">
            <div class="loadingOut">
                <img class='loading' src="../assets/loading.svg" alt="">
                <div v-if="text" class="loadingText">{{ text }}</div>
    		</div>
            <div class="shadow"></div>
    	</div>
    </template>
    <script>
    export default {
        name:'indicator',
        props: {
            visible:{
                type:Boolean,
                    default:false
            },
    		text:String
        }
    };
    </script>
    2、通过indicator.js实例化该组件，并暴露出调用该组件的open和close方法
    import Vue from 'vue';
    import indicator from './indicator.vue'
    const Indicator = Vue.extend(indicator);
    let instance;
    export default {
        open(options = {}) {
            if (!instance) {
                if (options.text) {
                    let text = options.text;
                }
                instance = new Indicator({
                    el: document.createElement('div'),
                    data: {
                        message: '123',
                        // text:options.text ? options.text : '',
                        // 注意：
                        //    1、定义在data中的text与定义在props中的text其实是一样的，同一个组件只允许出现一个。
                        //    2、所以如果在此处用了一个text，那么indicator的props里就不能再出现一个text，否则抛错。
                        //    3、也可以用一个全局的text，例如：instance.text
                    }
                })
            }
            if (instance.visible) {
                return;
            }
            instance.text = typeof options === 'string' ? options : options.text || '';
            document.body.appendChild(instance.$el) // 添加到根节点 body
            Vue.nextTick(() => {   // 一定要在钩子函数里
                instance.visible = true;
            });
        },
        close() {
            instance.visible = false;
        }
    }
    3、使用
    import {indicator} from './indicator.js'
    indicator.open() // 这样loading效果就会出来了。
    
    方法二：
    在方法一中，只是简单的创建一个实例DOM，并添加到body中，通过v-show来控制组件显示/隐藏。但像Dialog嵌套等组件，不能控制其销毁，解决方案：
    1、定义组件，和平时组件没多大区别，只是在method中需要暴露一个方法，用于接收参数
    <template>
        <div v-show="visible">
            <div class="messageBox-out">
                <p class="messageBox-title">标题</p>
                <p class="messageBox-content">{{ text || '是否确定？' }}</p>
                <div class="messageBox-btnList">
                    <div class="messageBox-leftBtn" @click="sureBtn">确定</div>
                    <div class="messageBox-rightBtn" @click="falseBtn">取消</div>
                </div>
            </div>
            <div class="messageBox-shadow"></div>
        </div>
    </template>
    <script>
    export default {
    	name: "indicator",
        props: {
            visible: {
                type: Boolean,
                    default: false
            },
    		text: String
        },
    	methods:{
            // 暴露一个 方法，传递参数
            show(options){
                // 将options传入的信息，绑定到props上
                Object.assign(this.$props, options);
                this.visible = true;
            },
    		// 确定按钮
    		sureBtn(){
                this.$emit("confirm");
                this.visible = false;
            },
    		// 取消按钮
    		falseBtn(){
                this.$emit("cancel");
                this.visible = false;
            }
        }
    }
    </script>
    2、引入组件，并生成实例
    import Vue from 'vue';
    import messageBox from './messageBox.vue';
    //生成一个扩展实例构造器
    const messageBoxConstructor = Vue.extend(messageBox);
    //创建MessageBox Vue实例
    const initMessageBox = function() {
        let instance = new messageBoxConstructor({
            el: document.createElement('div')
        });
        return instance;
    }
    //销毁组件 （注意：动态创建的组件，不用时应该销毁，不然多次触发创建，就会不停的创建DOM并append到body中）
    const destroyMessageBox = function(instance) {
        if(!instance) {
            return;
        }
        instance.$destroy();
        document.body.removeChild(instance.$el);
    }
    //挂在实例到body，参数传入
    const showMessageBox = function(options) {
        const {
            sureBtn,
            falseBtn
        } = options;
        // let sureBtn = options.sureBtn || function(){};
        // let falseBtn = options.falseBtn || function(){};
        const instance = initMessageBox();
        document.body.appendChild(instance.$el);
        Vue.nextTick(() => {
            instance.show(options); // 将传入的options绑定到 实例化后的messageBox.vue上
        });
        // 监听实例上的 确认按钮
        instance.$on("confirm", function(res) {
            sureBtn();
            destroyMessageBox(instance);
        });
        // 监听实例上的 取消按钮
        instance.$on("cancel", function(res) {
            falseBtn();
            destroyMessageBox(instance);
        })
    }
    export default {
        // 打开messageBox
        open(options){
            showMessageBox(options);
        },
        // 关闭messageBox
        close(){
            destroyMessageBox(instance); // 注意这里的instance实例，可以定义一个全局的变量来接收实例（即上面的实例）。
        }
    }
    3、使用
    import MessageBox from './messageBox/messageBox.js';  // 引入
    MessageBox.open({
        text:'取消将无法使用app',
        sureBtn:function(){
            console.log('确定！')
        },
        falseBtn:function(){
            console.log('取消！')
        }
    });
    
    方法三：
    在上面的一种方法中，解决了组件销毁问题，但是一个组件既要支持挂载DOM节点渲染，同时也应该支持JS动态渲染，解决办法：
    const initMessageBox = function(options) {
        let instance = new messageBoxConstructor({
            el: document.createElement('div'),
            propsData:{ ...options }
        });
        return instance;
    }
    // 注意：这里是propsData就是组件中的props，合并options参数到实例化的props中。这里还有一个点，如果propsData传递的参数，在实例化组件的props中没有定义的话，是不能挂载的（即便传递了，合并时会过滤掉）！！！
    const showDialog = function(options) {
        const {
            callbackMethod = function(res){}
        } = options;
        const instance = initDialog(options);
    
        let el = options.mountDom || document.body;
        el.appendChild(instance.$el);
        // 监听实例上的按钮(确定、取消等)
        instance.$on("callbackMethod", function(res) {
            callbackMethod(res);
            destroyDialog(instance);
        })
    }
    // 挂载点,如果传入了挂载点（options.mountDOM），就使用该挂载点，否则就是全局document.body。注意这里是appendChild，而不是innerHTML。实例挂载还可以使用：instance.$mount(document.body)方法。
    const destroyDialog = function(instance) {
        if(!instance) {
            return;
        }
        instance.$destroy();
        instance.mountDom.removeChild(instance.$el);
    }
    // 销毁组件时，是指定DOM的销毁，如果没有就是document.body
    ```

14. ##### vue中DOM操作

    ```js
    1、vue中点击获取当前对象，操作DOM。
    <div v-on:click="isShowFilter($event)">
        ......
    </div>
    isShowFilter:function(event){   //获取到的是当前点击的dom对象，如果不传$event，在fireFox下无法识别，并抛错！
        console.log(event.currentTarget);
    }
    
    2、也可以使用$refs的方法
    <input type="text" ref="input" id="input"/>
    在获取时我们只需要this.$refs.input就可以获取到上面的input对象。
    document.getElementById("input")与$refs的效果一样，不过用$refs性能更加优越。
    
    3、在组件中也可以使用$refs，父组件操作子组件中的方法和属性。
    // 子组件中
    <div>
        子组件
    </div>
    methods:{
        childMethod:function(){
            alert(123);
        }
    }
    // 父组件中
    <div>
        <child ref="child"></child>
    </div>
    那么在父组件中想调用子组件的childMethod方法，就可以使用this.$refs.child.childMethod(); //alert(123)
    ```

15. ##### vue面试常见问题？

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
    created:组件实例创建完成，属性已经绑定，data等已经赋值，但DOM还未正式挂载，所以$el还不存在。
    beforeMount：虚拟DOM已经存在，但是正式DOM还未挂载。
    mounted：正式DOM已经生产，并挂载完成。
    beforeUpdate：组件更新之前
    updated：组件更新之后
    beforeDestroy：组件销毁之前
    destroyed：组件销毁之后
    activated：组件被激活时调用
    deactivated：组件被移除时调用
    注：activated,deactivated：这两个生命周期函数比较特别，是专为keep-alive组件使用的。
    
    4、vue父子组件加载顺序
    初次加载：
    父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
    数据修改渲染：
    beforeUpdate -> updated
    销毁过程：
    父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
    
    5、vue-router的hash、history模式的区别？
    hash模式在路由地址后面有一个‘#’，看着不太习惯。
    
    6、v-if和v-for为什么不建议在同一dom节点上使用？（虽然可以！）
    原因：v-for的执行顺序高于v-if，所以一般都是v-for循环后，再判断是否显示，存在一定的性能浪费（说实话，在我操作循环遍历时，几十几百条数据渲染，这点性能的浪费，可以忽略不计，这只是理论上的一种优化方式）。
    解决办法：可以在外层嵌套一个template或者其他dom标签，先v-if判定后，在v-for循环
    
    7、setTnterval在vue中，clearInterval后，仍不停的触发？
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
    解决办法：将setInterval挂载到window对象上，而不是当前vue实例上
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
    
    8、关于vue组件加载完成后，操作DOM
    一般在写vue组件挂载完成后执行的函数用:
    mounted:function(){
        ......
    }
    但如果渲染的界面是异步（http返回数据），然后需要操作DOM时，会存在获取不到DOM的情况？
    解决办法：
    mounted:function(){
        this.$nextTick(function(){
            ......
        });
    }
                       
    9、vue-cli3兼容ie问题，参考：https://refined-x.com/2018/12/04/VueCLI3%20%E5%85%BC%E5%AE%B9%E6%80%A7%E9%85%8D%E7%BD%AE/
    9.1、如果确切知道有兼容性问题的依赖包名，可以配置项目根目录下的vue.config.js（默认不存在），将依赖包名添加到transpileDependencies键中，这会为该依赖同时开启语法语法转换和根据使用情况检测 polyfill。例如：
    module.exports = {
    	transpileDependencies: ["vue-plugin-load-script"]       // 需要编译的依赖包名
    }
    9.2、如果确切的知道需要转译的语言特性，可以配置根目录下的babel.config.js，为presets的值添加所需要的 polyfill，例如：
    module.exports = {
        presets: [
            ['@vue/app', {
                polyfills: [
                    'es6.symbol'
                ]
            }]
        ]
    }
    9.3、然而更多的情况是，我们并不确切的知道项目中引发兼容问题的具体原因，这时还可以配置为根据兼容目标导入所有 polyfill，需要设置babel.config.js为：
    module.exports = {
        presets: [
            ['@vue/app', {
                useBuiltIns: 'entry'
            }]
        ]
    }
    9.4、同时在入口文件（main.js）第一行添加
    import '@babel/polyfill'
        
    10、vue-cli3 项目运行时，如果app.js超过244kb就会warn警告，limit限制
    解决办法：
    在vue.config.js中
    overlay: {
        warnings: false,
    	errors: true
    }, // 错误、警告在页面弹出
    将overlay中的warnings暂时修改为false，只为能使用，最根本的解决办法还未找到？？？
    
    11、vue中关于设置服务代理方式 参考:https://blog.csdn.net/qq_36407748/article/details/90679991
    在vue.config.js中
    module.exports = {
      devServer: {
        port: port,
        open: false,
        overlay: {
          warnings: false,
          errors: true
        },
        proxy: {
          '/apis': {
            target: process.env.VUE_APP_BUSI_API,
            secure: false,
            changeOrigin: true,
            pathRewrite: {
              '^/apis': ''
            }
          },
          '/api': {
            target: process.env.VUE_APP_SSO_HOST,
            changeOrigin: true,
            pathRewrite: {
              '/api': ''
            }
          },
          ......
        }
      }
    }
    添加.env.development
    # dev环境，也是本地开发环境的配置
    ENV = 'development'
    port = 9549
    # 本机代理url配置
    # 统一认证后端服务地址,对应/api/
    VUE_APP_SSO_HOST = 'http://172.17.6.196:8888'
    # 统一用户管理后端服务地址,对应/ctfo-cloud-system/
    VUE_APP_SYS_HOST = 'http://172.17.6.196:8001'
    # 应急系统后端服务地址,对应/apis/   或者  tocc-emergency-disposal-ch
    VUE_APP_BUSI_API = 'http://172.17.6.126:31001'
    # 文件服务后端服务地址,对应/file/
    VUE_APP_FILE_API = 'http://172.17.6.196:8002'
    # 数据魔方服务地址,对应/dataApi/
    VUE_APP_CUBE_API = 'http://172.17.6.192:8589'
    当然也可以添加.env.production环境，配置内容同development
    ```

16. 虚位以待！！！

