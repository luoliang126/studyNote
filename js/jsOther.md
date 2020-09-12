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

2. 虚位以待！