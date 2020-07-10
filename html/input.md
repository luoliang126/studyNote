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
// 判断是否为ie浏览器
judgeIE(){
    if(!!window.ActiveXObject || "ActiveXObject" in window){
        return true;
    }else{
        return false;
    }
}
// 本地 上传文件(允许选择多张图片)
async localUpload({ 
    accepts,
    multiple=false,
    callback=function(data){
    	console.log(data)
	}
}){
    let isIe = this.judgeIE()
    let _this = this;
    // 每次先清空divEle
    this.divEle.innerHTML = '';

    // 文件处理
    let compileFile = async (files) => {
        let fileObject = [];
        for(let i=0;i<files.length;i++){
            let tempFileObject = {
                indexId:files[i].name, //new Date().getTime() 
                url:'',
                fileObject:files[i]
            }
            fileObject.push(tempFileObject); // 保存文件对象（是文件对象）
        }
        let res = await _this.getScan(fileObject); // 将图片文件，转为img能识别的地址
        callback(res);
    }

    if(isIe){        
        window.inputChange = async function(event){
            let files = event.target.files
            compileFile(files)
        }
        let inputEle = `<input id="ieTest" type="file" onchange="(function(event){inputChange(event)})(event)" style="display:none;"/>`
        this.divEle.innerHTML = inputEle
        document.getElementById('ieTest').click();
    }else{
        let inputEle = document.createElement('input');
        inputEle.setAttribute('type','file');
        // 设置可以选择多张图片
        if(multiple){
            inputEle.setAttribute('multiple','multiple');
        }
        // input accept允许类型。
        inputEle.setAttribute('accept',accepts);
        let id = new Date().getTime();
        inputEle.setAttribute('id',id);
        inputEle.style.display = 'none';
        this.divEle.appendChild(inputEle);

        inputEle.click();
        inputEle.onchange = async function(event){
            let files = event.target.files || inputEle.files || document.getElementById(id).files;
            compileFile(files)
        }
    }
};

```

