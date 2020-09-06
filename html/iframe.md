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

- ##### 使用iframe实现打印

  ```
  调用h5的window.print方法可以实现打印网页内容功能，但这里有很多小问题？
  方法一：使用截取dom节点（需要打印的部分）
  1、首先截取需要打印的dom节点。
  2、然后将原来的body内容拷贝，备份。
3、将截取的dom节点添加到body根节点
  4、调用window.print方法实现打印。
  5、打印完后（可能没打印，而是预览后取消），将备份的body内容，重新添加到body中，实现界面还原。
  
  方法二：使用@media print属性
  @media print{
      .noprint{
  	    display:none
      }
  }
  // 页面上把不需要打印的部分添加class="noprint",然后执行window.print()方法时，凡是加了noprint的都不在打印范围以内。
  
  方法三：创建一个新的iframe，然后将需要打印的部分填充到iframe中去，调用iframe.contentWindow.print实现打印。
  
  比较优缺点：
  方法一：截取dom打印需要将原来的body节点备份，打印完后在重新将备份内容返回给body，表面看没问题，实际在使用中（以vue为例），dom节点备份后，再返回填充后，原来的dom节点上绑定的 @click事件等，将会失效（具体原因暂未找到）。优化办法是：在执行window.print后刷新当前界面，这样就不用备份，以及还原了，但浏览器会自动刷新一次，体验上如果可以接受，就不存在太大问题。
  方法二：使用css3属性@media print可以实现，不打印的dom节点添加class=“noprint”，但是这种方法仅仅适用于dom节点很少，或者只有一个html界面的情况下，如果像大型的SPA引用，怎么给其他组件的dom标签添加noprint样式，就会变得很复杂，而且没有优化解决办法。
  方法三：使用iframe打印的弊端在于，如果我外层文档document引用了elementUI等组件样式，我再创建iframe，并将需要打印部分的dom节点，添加到iframe中去，但原本的elementUI样式不会添加到iframe中去，所以就会造成样式的不一致。如果是少量dom节点还好（可以自行修改样式），大量的css样式将会变得很复杂。所以优化办法是：我们需要打印部分的dom节点、css样式等都手动写（最好不要用UI组价库，样式也直接内嵌到dom节点上，而不是用一个css链接），这样的话我们创建的dom片段就是一个自带样式的dom片段，在添加到iframe打印的时候，将会很自然。
  总结：还是那句，没有最好的解决办法，只有最适合当前情况的解决办法。
  ```
  
  

