# sass笔记

1. ##### sass的安装

   ```css
   1、官方安装，参考：https://www.sass.hk/
   
   2、基于vue的sass
   npm安装node-sass、sass-loader
   
   3、配置，查看vue-cli2和cli3的区别
   ```

2. ##### sass的基本使用

   ```scss
   1、变量引用
   $width150:150px;  //定义一个$width150的变量
   $colorDefault:#666666;
   div {
       width:$width150; // 使用变量
       color:$colorDefault;
   }
   
   2、计算功能
   body {
       margin: (14px/2);
       top: 50px + 100px;
       right: $width150 * 10%;
   }
   
   3、嵌套
   元素嵌套
   div {
       h1 {
           color:red;
       }
   }
   属性嵌套
   p {
       border: {
           color: red;
       }
   }  
   // 注意，border后面必须加上冒号。
   引用父元素嵌套
   a {
       &:hover { color: #ffb3ff; }
   }
   编译后
   a:hover{
       color: #ffb3ff;
   }
   
   4、继承
   extend继承（class2继承class1的样式）
   .class1 {
       border: 1px solid #ddd;
   }
   .class2 {
       @extend .class1;
       font-size:120%;
   }
   
   5、mixin定义一个代码块，使用@include引用
   @mixin left {
       float: left;
       margin-left: 10px;
   }
   .float-left{
       @include left;
   }
   也可以当成函数（传参）使用
   @mixin left($value: 10px) { // 10px是默认值(如果没有传递参数时使用)，也可以传递多个参数，跟函数一样
       float: left;
       margin-right: $value;
   }
   .testDiv {
       @include left(20px);
   }
   
   6、外部文件引入
   @import "path/filename.scss";
   @import "foo.css";
   
   6、高阶用法
   if条件判断，for循环，function方式，暂未领悟！
   ```

3. ##### 基于vue+sass的皮肤更换方案

   ```scss
   方法一（推荐使用）：
   1、创建一个variable.scss文件，用于定义所有的主题色
   $color-primary: #2D82F0; //罗氏蓝
   $color-roche: #2D82F0; //罗氏蓝
   $color-merck: #0099FF; //默克蓝
   $color-lilly: #D7290F; //礼来红
   
   2、创建一个mixin.scss文件，用于设置主题色
   @import "./variable.scss";  // 引入配置所有的主题色
   @mixin color() {
       [data-theme="lilly"] & {  // 判断不同data-theme，选用不同的颜色
           color: $color-lilly;
       }
       [data-theme="roche"] & {
           color: $color-roche;
       }
       [data-theme="merckweb"] & {
           color: $color-merck;
       }
   }
   @mixin backgroundColor() {
       [data-theme="lilly"] & {
           background-color: $color-lilly;
       }
       [data-theme="roche"] & {
           background-color: $color-roche;
       }
       [data-theme="merckweb"] & {
           background-color: $color-merck;
       }
   }
   @mixin borderColor() {
       [data-theme="lilly"] & {
           border-color: $color-lilly;
       }
       [data-theme="roche"] & {
           border-color: $color-roche;
       }
       [data-theme="merckweb"] & {
           border-color: $color-merck;
       }
   }
   
   3、在入口文件App.vue中，设置data-theme属性
   let tenant = this.$cookies.getCookie('tenant').replace(/\s/g, "") || "roche";
   window.document.documentElement.setAttribute("data-theme", tenant);  // 设置html标签的 data-theme属性
   
   4、在组件中使用
   @import '@/assets/css/mixin.scss';  // 引入mixin.scss文件
   .isActive{
       @include color();  // 调用 mixin中的color()方法，因为App.vue中已经为dom元素添加了data-theme属性，那么mixin.scss中会自动匹配。从而得到对应的值
   }
   
   问题描述：
   1、以上文件中，我们可以发现，所有主题色只有一种，那么有多种怎么处理？
   解决办法：使用opacity透明度，这样主题只有一个，而对应的其他值有多个（opacity透明只能是颜色相近的情况，如果颜色跨度大，怎么解决？）
   2、如果非要用多个跨度大的颜色
   解决办法如下
   
   方法二：
   1、定义一个theme.scss
   @mixin theme($mainColor:#2D82F0,$inputBgColor:#0086DF,$inputColor:#FF9686){
       .sm-header{
           background:$mainColor !important;
       }
       .addrq-btn{
           background:$mainColor !important;
           box-shadow: 0px 0px 18px 1px $mainColor !important;
       }
       ......
   }
   把能用到关于主题的颜色的class标签都放在theme方法中
   
   2、添加全局样式
   在App.vue中
   <template>
   <div id="app" :class="theme">
   <router-view></router-view>
   </div>
   </template>
   
   export default {
       name: 'App',
       computed:{
           theme:{
               get:function(){
                   let tenant_code = this.iStorage.get('tenant_code')
                   let themeTip = '';
                   switch (tenant_code){
                       case 'roche':
                           themeTip = 'theme-roche'
                           break;
                       case 'lilly':
                           themeTip = 'theme-lilly'
                           break;
                       case 'novartis-master':
                           themeTip = 'theme-novartis'
                           break;
                       case 'pfizer':
                           themeTip = 'theme-pfizer'
                           break;
                       default:
   	                    themeTip = 'theme-default'
                   }
                   return themeTip;
               }
           }
       },
   }
   
   @import "./assets/sass/theme.scss";
   .theme-default{
       @include theme()
   }
   .theme-roche{
       @include theme(#2D82F0,#24A7FF,#ffffff)
   }
   .theme-lilly{
       @include theme(#D7290F,#EC4228,#FF9686)
   }
   .theme-novartis{
       @include theme(#0099FF,#24A7FF,#ffffff)
   }
   .theme-pfizer{
       @include theme(#00aae7,#47adf1,#ffffff)
   }
   
   方法三：
   获取皮肤颜色值（十六进制值），通过style绑定。
   实现方式：
   1. iaccount在初始化时通过/api/tpm/cfg/getsettings拿到当前租户的颜色值，存入cookie；
   2. 使用时通过style绑定computed.background-color;
   优势：在增加新皮肤颜色时，只需配置当前租户的自定义皮肤颜色值，前端无需变更代码即可实现换肤。
   劣势：修改点太多，涉及皮肤修改都需要使用双向绑定颜色值。
   
   方法四：css3的更换皮肤方案
   参考css3部分！
   
   方法五：通过插件webpack-theme-color-replacer插件方式更换皮肤
   基于element-ui的方法：参考：https://www.jianshu.com/p/10a9fe676145?from=singlemessage
   1、安装插件：webpack-theme-color-replacer、node-sass、sass-loader、babel-plugin-component、element-theme-chalk这几个插件是我基于vue-cli3搭建项目时，所用到的。
   2、在根目录下创建一个config目录---> app-config.js
   module.exports = {
       LOGIN_PATH: './',
       title: 'vue + webpack4 + element-ui脚手架项目',
       description: 'vue + webpack4 + element-ui脚手架项目',
       themeColor: '#409EFF' // 这个色号必须和主题色是一个颜色才行，要不然出来的css模板文件是空的
   }
   3、在src目录下，创建plugins目录---> themeColorClient.js（这个是修改主题色的插件，当然可以根据自己项目结构随意调整位置，注意引用路径即可！！！）
   import client from 'webpack-theme-color-replacer/client'
   import forElementUI from 'webpack-theme-color-replacer/forElementUI'
   // 注意自己项目里的引入路径
   import appConfig from '../../config/app-config'
   export let curColor = appConfig.themeColor
   // 动态切换主题色
   export function changeThemeColor(newColor) {
       var options = {
           newColors: [...forElementUI.getElementUISeries(newColor)]
       }
       return client.changer.changeColor(options, Promise)
       .then(() => {
           curColor = newColor
           localStorage.setItem('theme_color', curColor) // 把修改的主题色保存到localStorage，方便下次使用，当然也可以走接口保存！
       })
   }
   // 初始化主题色（就是上面保存时候的值）
   export function initThemeColor() {
       const savedColor = localStorage.getItem('theme_color')
       if (savedColor) {
           curColor = savedColor
           changeThemeColor(savedColor)
       }
   }
   4、配置babel.config.js  这个babel-plugin-component插件很重要，必须装否则无效，同时依赖的.scss，所以node-sass和sass-loader必须！
   module.exports = {
       presets: ["@vue/cli-plugin-babel/preset"],
       plugins: [
           [
               'babel-plugin-component',
               {
                   libraryName: 'element-ui',
                   styleLibraryName: '~node_modules/element-theme-chalk/src',
                   ext: '.scss'
               }
           ]
       ],
   };
   5、注意main.js
   ......
   import ElementUI from 'element-ui'; 
   // 注意，这里就不用引入element-ui的主题样式了，因为是我们后面生成的，可以查看一下DOM结构，生成的css样式会动态的append到body后面，我们下次动态修改主题色的时候就是修改的这个css。
   // import 'element-ui/lib/theme-chalk/index.css';
   Vue.use(ElementUI)
   // 初始化主题色
   import { initThemeColor } from './utils/themeColorClient'
   initThemeColor();
   ......
   6、界面上使用时
   <template>
       <div class="home">
   	    <el-button type="primary">主要按钮</el-button>
   	    <el-color-picker size="medium" @change="changeColor"></el-color-picker>
       	<div>
               <span class="font-test">
               	看我到底变不变色
               </span>
               <span class="font-test1">
   	            看我到底变不变色
               </span>
           </div>
       </div>
   </template>
   <script>
   import { changeThemeColor } from '@/utils/themeColorClient'
   export default {
       methods: {
           changeColor(newColor) {
               changeThemeColor(newColor).then(() => {......})
           }
       }
   }
   </script>
   注意：class="font-test"这个font-test选择器，估计是作者添加的，凡是用了这个class的标签，颜色会自动取主题色！！！
   基于ant-design的换肤方案，参考ant-design-vue-pro的那套！
   
   对比几种方法：
   第一种更全局更优雅，但扩充颜色主题较麻烦。
   第二种简单，但如果一个大项目较多时，使用的全局theme.scss定义的class也会较多，不适合大型项目
   第三种适用于少量界面，微小型项目。
   第四种，其原理是将皮肤色绑定到根节点dom上，这样css通过var方法获取，js通过
   var styles = getComputedStyle(document.documentElement);
   var value = styles.getPropertyValue("--theme-color");
   console.log(value);
   获取，使用方便简单，但会在根节点渲染时将主题色添加上。
   第五种，通过插件的方式，将UI组件的所有定义的主题色全部修改，再append到body上，每次切换主题色的时候，都是动态生成的主题样式表css文件，理论上这种方式是最可取的。目前还未遇到任何问题！
   总结：没有最好的解决办法，只有根据当前项目最适合的解决办法。
   ```

4. 虚位以待！

