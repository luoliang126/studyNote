# input笔记

- ##### input 上传文件

```js
async localUpload({ accepts,multiple,callback }){
        let callbackData = callback || function(data){
            console.log(data);
        }
        if(typeof(callbackData) != 'function'){
            alert('回调应该是一个方法！');
            return;
        }
        let _this = this;
        let inputEle = document.createElement('input');
        inputEle.setAttribute('type','file');
    	if(multiple){
	        inputEle.setAttribute('multiple','multiple'); // 设置可以选择多张图片
        }
        inputEle.setAttribute('accept',accepts); // input accept允许类型。
        let id = new Date().getTime();
        inputEle.setAttribute('id',id);
        inputEle.style.display = 'none';
        document.body.append(inputEle);
        inputEle.click();
        inputEle.onchange = async function(event){
            // 这里使用三种方式获取files，避免在创建时获取不到
            let files = event.target.files || inputEle.files || 
                		document.getElementById(id).files;
            for(let i=0;i<files.length;i++){
                let tempFileObject = {
                    indexId:files[i].name, // new Date().getTime()
                    url:'',
                    fileObject:files[i]
                }
                _this.initFileObject.push(tempFileObject); // 保存文件对象（是文件对象）
            }
            // 将图片文件，转为img能识别的地址
            let res = await _this.getScan(_this.initFileObject); 
            // 移出dom节点,如果不上传图片，点击取消，dom节点就一直存在 ？？？
            document.body.removeChild(document.getElementById(id));
            callbackData(res);
        }
    };
注意：以上方法在chrome上是没问题的，但是ie无法监听到input的change事件。
```

```js
上面方法的优化方案：将事件方法先绑定到window上，然后创建input，这样的方式监听可以实现兼容ie

```

