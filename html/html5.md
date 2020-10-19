# HTML5的新特性

参考：https://www.cnblogs.com/binguo666/p/10928907.html

1. ##### 本地存储

   ```js
   1、cookie（不属于h5，在之前就已经有cookie了）
   let userName = 'luoliang';
   document.cookie = "userName" + "=" + userName;
   1.1、cookie取值：
   document.cookie取出来的值是：userName=admin;password=1;pageSize=6 的字符串，所以要字符串拆分、筛选
   var cookies = document.cookie.split(";");
   for (var i = 0; i < cookies.length; i++) {
       var str = cookies[i].split("=");
       //console.log(str[0]);
       if (str[0] == "userName") { //选出字符为userName的字段
           console.log(str[1]);
       }
   }
   1.2、为cookie设置过期时间，时间一到cookie自动销毁（没有删除cookie的方法）
   var exp = new Date();
   exp.setTime(exp.getTime() + 60 * 2000);//过期时间2分钟 (getTime()得到的是毫秒)
   document.cookie = "userName" + "=" + userName + ";expires=" + exp.toGMTString(); //把时间转换为字符串(现在常用：toUTCString()方法)
   封装删除cookie的方法：
   function delCookie(name){
       var exp = new Date();
       exp.setTime(exp.getTime() - 1);
       var cval=getCookie(name);
       if(cval!=null){
           document.cookie= name + "="+cval+";expires="+exp.toGMTString();
       }
   }
   1.3、为cookie设置 域名访问权限
   问题描述：有个地址www.aaa.com一级域名 aaa.com 如果在该一级域名下，还有一个二级域名eg:www.bbb.aaa.com 这里的bbb就是二级域名，那么问题来了，设置在www.aaa.com下的cookie 在 www.bbb.aaa.com下可以访问，子域可以访问顶域的cookie。那假如设置在www.bbb.aaa.com的cookie想在 www.aaa.com下怎么访问？（顶域无法获取子域数据）
   解决办法：
   将子域的cookie设置为顶域
   eg: setCookie('token','.......',{domain:window.idomain}) // 这里的window.idomain 可以设置为aaa.com
   
   2、localStorage存储
   存值:window.localStorage.setItem(key,value)：将value存储到key字段
   取值:window.localStorage.getItem(key):获取指定key本地存储的值
   删除:window.localStorage.removeItem(key):删除指定key本地存储的值
   localStorage在存储单个key，value时，很方便，但在存储JSON对象时，应当先将JSON对象转换为JSON字符串
   var students = {
       liyang:{name:"liyang",age:17},
       lilei:{name:"lilei",age:18}
   } // 需要存储的JSON对象
   students = JSON.stringify(students);//将JSON对象转化成字符串
   localStorage.setItem("students",students);//用localStorage保存转化好的的字符串
   取值时：
   var students = localStorage.getItem("students");// 取students变量
   students = JSON.parse(students);//把字符串转换成JSON对象
   
   3、sessionStorage存储
   sessionStorage与localStorage方法相同，唯一不同的就是localStorage是本地存储，不删除的话就会常驻浏览器内存中。而sessionStorage是会话存储，当关闭浏览器是自动销毁！！！
   ```

2. ##### navigator对象：通常用于获取浏览器和操作系统的信息(详情查看w3cschool)

   ```js
   
   ```

3. 虚位以待！！！