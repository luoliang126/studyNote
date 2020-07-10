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

3. #####  数组排序

4. 虚位以待！