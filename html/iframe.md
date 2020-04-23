# iframe笔记

- ##### iframe加载网页，但iframe的高度不能确定，所以iframe要自适应内容的高度。

  原生js方法:通过contentWindow来获取子页面的元素，也有用contentDocument获取的，但是contentWindow 兼容各个浏览器，可取得子窗口的 window 对象。

  ```js
  <iframe src="" id="ifm" frameborder="0"></iframe>
  document.getElementById("ifm").onload = function(){
  	var ifmEle = document.getElementById('ifm');
  	//获取内部iframe的body
  	var ifmInEle = ifmEle.contentWindow.document.body;
  	//加30是为了让子页面中不出现滚动条
  	var mainheight = ifmInEle.offsetHeight + 30;
  	ifmEle.style.height = mainheight+"px";
  	//获取id=printPage 的标签
  	var divEle = ifmEle.contentWindow.document.getElementById("printPage") 
  };
  注意：以上的iframe当中的链接必须是同源，即不能够跨域，否则无法访问。
  ```

- ##### iframe跨域加载的页面，在父页面是无法修改子页面的内容。而非要修改的话，父子必须同源。

  ###### 参考地址：http://blog.csdn.net/fdipzone/article/details/17619673
  
  在A.html中
  
  ```js
  <button onclick="exec_iframe">执行B页面的方法</button>
  <iframe src="B.html" name="myframe" width="500" height="100"></iframe>
  <script type="text/javascript">
      function fMain(){
          alert('我是主页面');
      }
      function exec_iframe(){
          window.myframe.fIframe(); //执行B页面中的方法
      }
  </script>
  ```
  
  在B.html中
  
  ```js
  <button onclick="exec_main()">执行A页面的方法</button>
  <script type="text/javascript">
      function fIframe(){
  		alert('我是iframe里面的页面');
      }
      function exec_main(){
  		parent.fMain();  //执行A页面中的方法
      }
  </script>
  ```

- ##### 通过iframe + window.postMessage实现跨域通信传递参数

  ###### 参考链接：地址一：https://blog.csdn.net/szu_aker/article/details/52314817 地址二：https://blog.csdn.net/aurum_hj/article/details/78304787

  ```js
  1、主页面发送数据，子页面接收。
  在a.html文件中
  <iframe id="proxy" src="http://www.baidu.com/test/b.html" onload = "postMsg()"></iframe>
  <script type="text/javascript">
      var obj = {
          msg: 'this is come from client message!'
      }
      function postMsg (){
          var iframe = document.getElementById('proxy');
          var win = iframe.contentWindow;
          win.postMessage(obj,'*');
      }
      // 注意此处的*代表所有的子页面，如果是具体的界面（如http://www.baidu.com/test/b.html）。
      // 那么只能向该文件地址发送obj，如果为其他地址则无法接收。
  </script>
  在b.html中
  <script type="text/javascript">
      window.onmessage = function(e){ // onmessage和addeventListener是一样的
      if(e.origin !== 'http://localhost') return;
      console.log(e.origin+' '+e.data.msg);
  }
  </script>
  
  2、子页面发送数据，主页面接收
  在b.html中
  <script type="text/javascript">
      let data = {
          name:'luoliang'
      }
  window.parent.postMessage(data,"*");
  </script>
  在a.html中
  window.addEventListener('message',function(rs){
      console.log(rs.data);
  })
  
  注意：iframe嵌套必须在b.html加载完成后，才能执行。即onload之后执行postMessage方法。
  	 iframe的传参方式请查看smart-iframe-dialog方法封装组件
  ```

  

