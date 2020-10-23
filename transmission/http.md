# http协议笔记

1. ##### http协议介绍

   ```js
   1、http超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是一种用于分布式、协作式和超媒体信息系统的应用层协议。传输HTML文件、图片文件、查询结果等。
   发展史：在互联网早期的时候，我们输入的信息只能保存在本地，无法和其他电脑进行交互。我们保存的信息通常都以文本即简单字符的形式存在，文本是一种能够被计算机解析的有意义的二进制数据包。而随着互联网的高速发展，两台电脑之间能够进行数据的传输后，人们不满足只能在两台电脑之间传输文字，还想要传输图片、音频、视频，甚至点击文字或图片能够进行超链接的跳转，那么文本的语义就被扩大了，这种语义扩大后的文本就被称为超文本(Hypertext)。
   
   2、特点
   2.1、简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
   2.2、灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
   2.3.无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
   2.4.无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。
   2.5、支持B/S及C/S模式。
   ```

2. ##### 什么是传输？

   ```
   两台计算机之间会形成互联关系进行通信，我们存储的超文本会被解析成为二进制数据包，由传输载体（例如同轴电缆，电话线，光缆）负责把二进制数据包由计算机终端传输到另一个终端的过程，称为传输(transfer)。通常我们把传输数据包的一方称为请求方，把接到二进制数据包的一方称为应答方。请求方和应答方可以进行互换。
   ```

3. ##### 网络模型（分层）

   ```
   为了给网络协议的设计提供一个结构，网络设计者以分层(layer)的方式组织协议，每个协议属于层次模型之一。每一层都是向它的上一层提供服务(service)，即所谓的服务模型(service model)。每个分层中所有的协议称为 协议栈(protocol stack)。因特网的协议栈由五个部分组成：物理层、链路层、网络层、运输层和应用层。我们采用自上而下的方法研究其原理，也就是应用层 -> 物理层的方式。
   1、应用层
   应用层是网络应用程序和网络协议存放的分层，因特网的应用层包括许多协议，例如我们学 web 离不开的 HTTP，电子邮件传送协议 SMTP、端系统文件上传协议 FTP、还有为我们进行域名解析的 DNS 协议。应用层协议分布在多个端系统上，一个端系统应用程序与另外一个端系统应用程序交换信息分组，我们把位于应用层的信息分组称为 报文(message)。
   
   2、运输层
   因特网的运输层在应用程序断点之间传送应用程序报文，在这一层主要有两种传输协议 TCP和 UDP，利用这两者中的任何一个都能够传输报文，不过这两种协议有巨大的不同。
   TCP 向它的应用程序提供了面向连接的服务，它能够控制并确认报文是否到达，并提供了拥塞机制来控制网络传输，因此当网络拥塞时，会抑制其传输速率。
   UDP 协议向它的应用程序提供了无连接服务。它不具备可靠性的特征，没有流量控制，也没有拥塞控制。我们把运输层的分组称为 报文段(segment)
   
   3、网络层
   因特网的网络层负责将称为 数据报(datagram) 的网络分层从一台主机移动到另一台主机。网络层一个非常重要的协议是 IP 协议，所有具有网络层的因特网组件都必须运行 IP 协议，IP 协议是一种网际协议，除了 IP 协议外，网络层还包括一些其他网际协议和路由选择协议，一般把网络层就称为 IP 层，由此可知 IP 协议的重要性。
   
   4、链路层
   现在我们有应用程序通信的协议，有了给应用程序提供运输的协议，还有了用于约定发送位置的 IP 协议，那么如何才能真正的发送数据呢？为了将分组从一个节点（主机或路由器）运输到另一个节点，网络层必须依靠链路层提供服务。链路层的例子包括以太网、WiFi 和电缆接入的 DOCSIS 协议，因为数据从源目的地传送通常需要经过几条链路，一个数据包可能被沿途不同的链路层协议处理，我们把链路层的分组称为 帧(frame)
   
   5、物理层
   虽然链路层的作用是将帧从一个端系统运输到另一个端系统，而物理层的作用是将帧中的一个个 比特 从一个节点运输到另一个节点，物理层的协议仍然使用链路层协议，这些协议与实际的物理传输介质有关，例如，以太网有很多物理层协议：关于双绞铜线、关于同轴电缆、关于光纤等等。
   ```

4. ##### URL介绍

   ```js
   HTTP使用统一资源标识符（Uniform Resource Identifiers）URI来传输数据和建立连接，而URL是一种特殊类型的URI。
   eg:http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name
   1.协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符.
   2.域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用
   3.端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口
   4.虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”
   5.文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名
   6.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分
   7.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。
   ```

5. ##### http报文组成部分。参考：https://blog.csdn.net/mao_xiaoxi/article/details/89313734

   ```js
   请求报文：请求行、请求头、请求体
   请求行：POST /chapter17/user.html HTTP/1.1
       1、请求方法，GET和POST是最常见的HTTP方法，除此以外还包括DELETE、HEAD、OPTIONS、PUT、TRACE。
       2、请求对应的URL地址，它和报文头的Host属性组成完整的请求URL。
       3、协议名称及版本号 HTTP/1.1
   请求头：
   	HTTP的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息。与缓存相关的规则信息，均包含在header中
       例如：
       Accept: application/json, text/javascript,; q=0.01
       Accept-Encoding: gzip, deflate, br
       Accept-Language: zh-CN,zh;q=0.9
       Connection: keep-alive
   	Cookie: $Version=1; Skin=new;jsessionid=5F4771183629C9834F8382E23 
       ......
       这里重点提一下accept、cookie等
   	accept：通过一个“Accept”报文头属性告诉服务端，客户端接受什么类型的响应。属性的值可以为一个或多个MIME类型的值（描述消息内容类型的因特网标准， 消息			能包含文本、图像、音频、视频以及其他应用程序专用的数据）
       cookie：客户端可以通过Cookie传值给服务端。
       Cache-Control：对缓存进行控制，如一个请求希望响应返回的内容在客户端要被缓存一年，或不希望被缓存就可以通过这个报文头达到目的。
   请求体：
   	即报文体，它将一个页面表单中的组件值通过param1=value1&param2=value2的键值对形式编码成一个格式化串，它承载多个请求参数的数据。不但报文体可以传递请求参数，请求URL也可以通过类似于“/chapter15/user.html? param1=value1&param2=value2”的方式传递请求参数
   
   响应报文：响应行、响应头、响应体
   响应行：HTTP/1.1 200 OK
       1、报文协议及版本； 
   	2、状态码及状态描述；
   响应头：
       响应报文头，也是由多个属性组成（参考请求头）
       Cache-Control：响应输出到客户端后，服务端通过该报文头属告诉客户端如何控制响应内容的缓存。常见的取值有private、public、no-cache、max-age，no-store，默认为private。
           private客户端可以缓存
           public客户端和代理服务器都可缓存（前端的同学，可以认为public和private是一样的）
           max-age=xxx 缓存的内容将在 xxx 秒后失效
           no-cache 需要使用对比缓存来验证缓存数据
           no-store所有内容都不会缓存
       Set-Cookie服务端可以设置客户端的Cookie，其原理就是通过这个响应报文头属性实现的
   响应体：
       响应报文体，即我们真正要的“干货”。我们从接口拿到的数据都放在这里面。
   ```

6. ##### http和https。参考：https://zhuanlan.zhihu.com/p/43789231

   ```js
   http://www.domain.com/index
   网站的URL会分以下几部分：通信协议(http)、域名(www.domain.com)、地址(index)。
   域名/地址都很好理解，不同的域名/地址表示不同网站中不同的页面，而通信协议，简单来说就是浏览器和服务器之间沟通的语言。网站中的通信协议一般就是HTTP协议和HTTPS协议。
   
   HTTP协议：是一种使用明文数据传输的网络协议。一直以来HTTP协议都是最主流的网页协议，但HTTP协议的明文传输会让用户存在一个非常大的安全隐患，那就是在提交一些敏感数据的时候（如银行卡卡号、密码），传输时被第三方截取。
   HTTPS协议：可以理解为HTTP协议的升级，就是在HTTP的基础上增加了数据加密。在数据进行传输之前，对数据进行加密，然后再发送到服务器。这样，就算数据被第三者所截获，但是由于数据是加密的，所以你的个人信息仍然是安全的。这就是HTTP和HTTPS的最大区别。
   
   我们常常说https比http更加安全可靠，是因为https在http上进行了加密，但是怎么加密的呢？怎么就更安全了呢？
   https在http基础上，在传输层传输之前进行加密
   ```

7. ##### 原生http的封装和使用

   ```js
   var request = new XMLHttpRequest();// 创建一个XMLHttpRequest对象
   var method = "GET"; //设置请求方式method类型
   request.open(method,url); // 确定访问地址url
   request.send(null); // 发送请求
   request.onreadystatechange = function(){ //监听响应事件
       if(request.readyState == 4){
           if(request.status ==200 || request.status == 304){
               console.log(request.responseText);
           }
       }
   }
   XMLHttpRequest的属性：
   1、onreadystatechange 每个状态改变都会触发这个时间处理器，通常会调用一个JavaScript函数
   2、readyState 请求的状态，有5个可用值，0代表未初始化，1代表正在加载，2代表已经加载，3代表交互中，4代表完成
   3、responseText 服务器的响应，表示一个串
   4、responseXML 服务器的响应，表示为XML，这个对象可以解析为DOM对象
   5、status 服务器的HTTP状态码（200对应OK,304对应上次请求后没有更新就可以不用上传节省流量开支，404对应not found服务器上没有找到对应的界面等，500服务器错误无法完成请求）
   6、statusText HTTP状态码的相应文本（ok,NOT found等）
   
   XMLHttpRequest的方法：
   1、abort() 停止当前请求
   2、getAllResponseHeaders() 把HTTP请求的所有响应首部作为键/值对返回
   3、getResponseHeader("header") 返回指定首部的串值
   4、open("method","url") 建立对服务器的调用，method:GET/POST/PUT,url:路径可以是相对路径也可以是绝对路径
   5、send(content) 向服务器发送请求附带参数，如果没有为null
   6、setRequestHeader("header","value") 把指定首部设置为所提供的值，在设置任何首部之前必须先调用open()
   ```

8. ##### axios的使用 <a href="http://blog.csdn.net/binginsist/article/details/65630547" target="_blank">参考文档</a>

   ```js
   以vue为例：
   1、安装axios： npm install axios
   
   2、在项目中引入axios：import axios from 'axios'
   
   3、使用get/post
   axios.get('/user?ID=12345')
   .then(function (response) { //成功后的回调函数
       console.log(response);
   })
   .catch(function (response) { //失败后的回调函数
       console.log(response);
   });
   // 也可以通过 params 对象传递参数
   axios.get('/user', {
       params: {
           ID: 12345
       }
   })
   .then(function (response) {
       console.log(response);
   })
   .catch(function (error) {
       console.log(error);
   });
   axios.post('/user', {
       firstName: 'Fred',
       lastName: 'Flintstone'
   })
   .then(function (response) {
       console.log(response);
   })
   .catch(function (response) {
       console.log(response);
   });
   
   4、并发多个请求方法：
   function getUserAccount() {
       return axios.get('/user/12345');
   }
   function getUserPermissions() {
       return axios.get('/user/12345/permissions');
   }
   axios.all([getUserAccount(), getUserPermissions()])
   .then(axios.spread(function (acct, perms) {
       //acct，perms分别对应第一个，第二个请求的返回值
   }));
   
   5、全局 axios 默认配置
   axios.defaults.baseURL = 'https://api.example.com';
   axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
   axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
   
   6、拦截器（很实用），在发送请求前，响应后都可以拦截到，并作相应的处理。
   // 添加一个请求拦截器
   axios.interceptors.request.use(function (config) {
       // Do something before request is sent 可以在这里添加请求头部信息，添加验证token等
       return config;
   }, function (error) {
       // Do something with request error
       return Promise.reject(error);
   });
   // 添加一个响应拦截器
   axios.interceptors.response.use(function (response) {
       // Do something with response data   可以在这里拦截响应信息，例如返回值格式化等
       return response;
   }, function (error) {
       // Do something with response error
       return Promise.reject(error);
   });
   可以拦截每一次发送的http请求，从而做出一个loading的显示和隐藏功能
   
   7、axios虽然好，但是不兼容IE，所以需要引入npm install es6-promise --save-dev。参考文档：https://segmentfault.com/q/1010000010160881
   然后再main.js中使用（放在第一行）
   import promise from 'es6-promise';
   promise.polyfill();
   
   8、挂载全局axios
   // 在http.js文件中，封装axios方法
   import config from './config'
   import api from "axios"
   import qs from 'qs' // 使用qs模块是为了参数序列化，构造出表单的请求体
   import router from '../router/index'
   import $cookie from 'vue-cookie'
   import iView from 'iview';
   import configuration from '@/configuration'
   // axios的默认配置
   api.defaults.baseURL = config.host;
   api.defaults.timeout = 5000;
   api.defaults.headers.common['Authorization'] = '';
   api.defaults.headers.post['Content-Type'] = 'application/json; charset=utf-8';
   // axios的请求拦截
   api.interceptors.request.use((request) => {
       iView.LoadingBar.start();
       request.url = config.build(request.url);
       //只有当content-type为form类型时才使用qs.stringify
       if (request.data && request.headers['Content-Type'] === 'application/x-www-form-urlencoded') {
           request.data = qs.stringify(request.data);
       }
       var token = $cookie.get('token');
       // 调用登陆站点
       if (token && token != null) {
           request.headers["Authorization"] = 'Bearer ' + token;
       }else{
           var returnUrl = window.location.href;
           window.location.href = configuration.uriIS + "?returnUrl=" + returnUrl;
       }
       return request;
   },
   error => {
       return Promise.reject(error);
   });
   // axios的响应拦截
   api.interceptors.response.use(response => {
       iView.LoadingBar.finish();
       // return response.data;
       return response;
   },
   error => {
       iView.LoadingBar.finish();
       if (error.response) {
           switch (error.response.status) {
               case 400:
                   break;
               case 401:
                   router.replace({
                       path: '/Login'
                   })
                   break;
           }
       }
       var data = {
           message: error.message,
           data: (error.response && error.response.data) || null
       };
       return Promise.reject(data);
   });
   export {
   	api,
       qs
   }
   // 注意：qs模块的使用，不是所有请求都需要，只有request.headers['Content-Type'] === 'application/x-www-form-urlencoded'才需要。
   
   // 挂载到vue上，提供全局使用
   import Vue from 'vue'
   import api from "./apis/http.js";
   Vue.prototype.api = api;
   
   使用axios上传进度的监听(不是真实的http上传进度，而是预请求的进度) 参考：https://www.cnblogs.com/wxb-it/p/7774758.html
   var response = await this.api.post(urls.urlUpload, formData, {
       method: "post",
       headers: {
           "Content-Type": "multipart/form-data"
       },
       onUploadProgress: function (e) {
           let percentage = Math.round(e.loaded * 100 / e.total) || 0;
           let waitPercentage = null;
           if (percentage >= 99){  // 模拟进度，加载到99时就停止，直到真正的http请求响应结束才置为100
               percentage == 99;
               waitPercentage = 99;
           }
           fileItem.progress = waitPercentage ? waitPercentage : percentage;
       }
   }).catch(function (error) {
       console.log("upload error:", error);
       return Promise.reject(error);
   });
   if (response) {
       console.log(response);
   }
   ```

9. 虚位以待！

