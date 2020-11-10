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
   使用TPM获取皮肤颜色值（十六进制值），通过style绑定。
   实现方式：
   1. iaccount在初始化时通过/api/tpm/cfg/getsettings拿到当前租户的颜色值，存入cookie；
   2. 使用时通过style绑定computed.background-color;
   优势：在增加新皮肤颜色时，只需配置当前租户的自定义皮肤颜色值，前端无需变更代码即可实现换肤。
   劣势：修改点太多，涉及皮肤修改都需要使用双向绑定颜色值。
   
   对比两种方法：
   第一种更全局更优雅，但扩充颜色主题较麻烦。
   第二种简单，但如果一个大项目较多时，使用的全局theme.scss定义的class也会较多，不适合大型项目
   总结：没有最好的解决办法，只有根据当前项目最适合的解决办法。
   ```

4. 虚位以待！