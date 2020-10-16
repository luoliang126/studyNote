# js其他知识、问题点

1. ##### js实现打印

   ```js
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
   
   打印样式修改
   1、调整打印页边距
   @page{
   	margin:0; // 具体值已打印效果为准
   	margin:auto 0mm; （上下设置为自动居中，左右边距为0）
   }
   2、调整打印方向（横向/纵向）
   @page{
   	size：portrait;（纵向)
   	size：landscape;（横向)
   }
   更多时候，我们在调用window.print()的时候，更希望用js方法调整打印方向。
   原理：动态创建一个style样式表，在执行打印的时候将其append到需要打印的容器中（默认为body）
   let $changePrint = function (type) {
     let html = '';
     type = type ? type : 1;
     if(type=='1'){
       html='<style type="text/css" media="print">\n' + '  @page { size: landscape; }\n' + '</style>';
     }else{
       html='<style type="text/css" media="print">\n' + '  @page { size: portrait; }\n' + '</style>';
     }
     return html;
   };
   document.body.append($changePrint(1));// 将样式append到根节点，或者需要打印的容器中。
   ```

2. ##### 前端微服务 参考：https://zhuanlan.zhihu.com/p/96717332

   ```js
   1、使用 HTTP 服务器的路由来重定向多个应用，即路由分发
   2、在不同的框架之上设计通讯、加载机制，诸如Single-SPA 和 Mooa
   3、通过组合多个独立应用、组件来构建一个单体应用
   4、iFrame。使用 iFrame 及自定义消息传递机制
   5、使用纯 Web Components 构建应用
   ```

3. ##### js日期、时间对象

   ```js
   日期对象：
   var date = new Date; //获取到的是，标准时间   Fri Mar 24 2017 16:06:02 GMT+0800 (中国标准时间)
   var year = date.getFullYear(); //获取到具体年份 2017，注意：如果是date.getYear()获取到的是117，而不是2017；
   var month = date.getMonth()+1; //月份是从0开始计算，所以加1
   var day = date.getDate(); //获取今日
   var hour = date.getHours(); //获取当前小时
   var minute = date.getMinutes(); //获取当前分钟
   var second = date.getSeconds(); //获取当前秒
   var weekday = date.getDay(); //获取星期几 如：5  代表星期五
   var time = data.getTime(); //获取到的是以毫秒计算的数值（时间戳）
   var arr = ["星期日","星期一","星期二","星期三","星期四","星期五","星期六"]
   divEle.innerHTML = year + "年" + month + "月" + day + "日" + hour + "时" + minute + "分" + second + "秒" + arr[weekday];
   
   倒计时：
   var curtime = new Date;
   var endtime = new Date("2017,6,7")
   var lefttime = endtime.getTime() - curtime.getTime(); //　转换为时间戳进行　减法
   // 转换为毫秒1天 = 24h  1h = 60minutes 1minute = 60seconds 1second = 1000ms
   var day = Math.ceil(lefttime/(24*60*60*1000));　// 转化为 ‘天’
   var second = parseInt(lefttime%60)	//此处用取整，不用math.ceil(因为倒计时不用向上取整)
   var minute = parseInt(lefttime/60%60);	//转换为m
   var hour = parseInt(lefttime/(60*60)%24);	//对24取模是因为每天有24h，我们只需要余下的部分
   var date = parseInt(lefttime/(24*60*60));
   
   日期的越界：在处理像2月这种不知道最后一天是28，29的时候最有用
   var preMonthLastday = new Date(2017,5,0)  //6月的第0天，也就是上个月的最后一天，即2017年5月31日
   获取本月的最后一天也可以用：var currentMonthLastDay = new Date(2017,6,0); //下月的第0天
   
   js时间格式化format 参考：https://blog.csdn.net/vasilis_1/article/details/73649961
   // 对Date的扩展，将 Date 转化为指定格式的String
   // 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符，
   // 年(y)可以用 1-4 个占位符，
   // 毫秒(S)只能用 1 个占位符 (是 1-3 位的数字)
   Date.prototype.Format = function (fmt) {
       var o = {
           "M+": this.getMonth() + 1, // 月份
           "d+": this.getDate(), // 日
           "h+": this.getHours(), // 小时
           "m+": this.getMinutes(), // 分
           "s+": this.getSeconds(), // 秒
           "q+": Math.floor((this.getMonth() + 3) / 3), // 季度
           "S": this.getMilliseconds() // 毫秒
       };
       if (/(y+)/.test(fmt)){
           fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
   	}
       for (var k in o)
           if (new RegExp("(" + k + ")").test(fmt))
               fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
       		return fmt;
   }
   // 使用：
   new Date().Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423
   new Date().Format("yyyy-M-d h:m:s.S") ==> 2006-7-2 8:9:4.18
   
   ```

4. ##### js正则

   ```js
   
   ```

5. 虚位以待！