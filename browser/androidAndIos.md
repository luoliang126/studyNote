# android和ios兼容问题

1. ##### 关于position：fixed

   ```
   ios使用fixed后（例如top：0固定在顶部），当调用输入框时，输入框会把fixed的定位部分往上顶，当我们关闭输入框的时候，顶部的fixed不会返回到top:0
   解决办法：使用position：absolute
   
   fixed定位：https://www.jianshu.com/p/47db0d2ab2a7
   在ios上无论顶部，还是底部都会被遮挡
   解决办法：
   在外层套一个div元素，
   ```

2. ##### 关于日期格式问题

   ```
   使用日期时，我们获取到时间是2019-03-07 但是使用ios时 不能识别（在new Date()时会抛错）。
   
   解决办法：将2019-03-07 转换为：2019/03/07（android和ios都可识别）
   
   顺便提一句，在ie下 可以识别2019-03-07也可以识别2019/03/07，但是却无法识别2019-03-07 00:00:00.
   所以最终的解决办法还是转换为2019/03/07 00:00:00
   ```

3. ##### 关于ios中，使用了input，调用键盘输入法后，界面被顶上去了，input失焦后，顶部无法滚下来，而是定在了顶部

   ```
   解决办法：在input失焦的时候blur 使用window.scrollTo(0, 0)
   ```

4. ##### input type=file 在上传的时候，如果是动态创建的dom元素，一定要插入到body中，然后设置display：none，再通过change事件获取value（ios获取不到，android可以）

   ```js
   let inputEle = document.createElement('input');
   document.body.append(inputEle);
   inputEle.onchange = async function(event){
       // 多种获取方式
       let files = event.target.files || inputEle.files ||document.getElementById(id).files; 
       for(let i=0;i<files.length;i++){
           let tempFileObject = {
               indexId:files[i].name, // new Date().getTime()
               url:'',
               fileObject:files[i]
           }
           _this.initFileObject.push(tempFileObject); // 保存文件对象（是文件对象）
       }
       let res = await _this.getScan(_this.initFileObject); // 将图片文件，转为img能识别的地址
       // 移出dom节点,如果不上传图片，点击取消，dom节点就一直存在 ？？？
       document.body.removeChild(document.getElementById(id));
       callbackData(res);
   }
   注意：这里还有一个ie兼容问题，详见ie兼容
   ```

5. ##### android返回键监听及使用

   ```js
   参考ievent项目
   ```

6. 虚位以待！

