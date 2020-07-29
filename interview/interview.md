# 面试常见问题

1. ##### this指向问题

   ```js
   var name = "The Window";
   var object = {
       name: "My Object",
       getNameFunc: function () {
           var that = this;
           return function () {
               return that.name;
           };
       }
   };
   console.log(object.getNameFunc()());
   //输出结果:My Object
   
   (function Fn(){
       var user = "My Object";
       console.log(this.user);
       console.log(this);
   })();
   //输出结果:undefined   window
   
   var a = 1, b = 2;
   a = [b,b=a][0];
   console.log(a, b);
   //输出结果:2，1
   ```

2. #####  数组去重

   ```
   详见js algorithm部分
   ```

3. #####  数组排序

   ```
   详见js algorithm部分
   ```

4. ##### 浏览器兼容问题

5. ##### vue中父子组件生命周期加载的顺序 

   ```
   初次加载：
   父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
   
   数据修改渲染：
   beforeUpdate -> updated
   
   销毁过程：
   父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed
   ```

6. 虚位以待！