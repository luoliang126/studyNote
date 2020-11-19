# Electron笔记

1. ##### electron介绍

   ```
   Electron 是一个框架，可以让您使用 JavaScript, HTML 和 CSS 创建桌面应用程序。 然后这些应用程序可以打包在macOS、Windows和Linux上直接运行，或者通过Mac App Store或微软商店分发。
   ```

   

2. ##### electron安装

   ```js
   安装electron
   npm install -g electron // 全局安装
   npx electron -v // 查看安装版本号，npx必须要node5.0以上才能使用
   
   原生创建项目
   1、到项目目录下，新建一个index.html，内容为hello world
   2、创建主入口文件main.js
   let electron = require("electron"); //引入electron
   let app = electron.app; // 引用app
   let BrowserWindow = electron.BrowserWindow; // 引入electron的窗口对象BrowserWindow
   let mainWindow = null; // 声明一个主窗口
   app.on('ready',() => {  // 监听窗口ready后的事件
   	mainWindow = new BrowserWindow({
   		width:800,
   		height:600,
   	});
   	mainWindow.loadFile('index.html'); // 加载主页面
   	mainWindow.on('close', () => { // 监听窗口的关闭回调，设置mainWindow为null，方便垃圾回收，否则多次运行可能卡顿，甚至崩溃
   		mainWindow = null;
   	})
   })
   3、在终端中，执行npm init --yes // 默认都为yes，省去每次的手动操作
   执行后会生成一个package.json文件，里面就有入口文件main.js等信息，当然可以后期修改！
   如果，第一步就执行npm init的话，那么它里面默认的main入口为index.js，当然这些都可以配置修改，根据个人操作习惯而定。
   在package.json中
   {
       ......
       "main": "main.js" // 指定入口文件为main.js
   }
   4、终端执行：electron . 这是运行electron项目的方法，当然你可以配置一个npm的运行指令。
   在package.json中配置
   "scripts": {
       "start": "electron ." // 指定运行脚本electron
   },
   5、此时应该就会打开一个窗口了。
   
   克隆github上的demo项目
   1、克隆示例项目的仓库
   $ git clone https://github.com/electron/electron-quick-start
   2、进入这个仓库
   $ cd electron-quick-start
   3、安装依赖
   $ npm install
   4、运行
   $ npm start
   ```

   

3. ##### 运行发布

   ```js
   1、创建主脚本main.js
   const { app, BrowserWindow } = require('electron') //导入应用程序，浏览器窗口
   function createWindow () {
     const win = new BrowserWindow({ //新建一个主窗口
       width: 800,
       height: 600,
       webPreferences: {
         nodeIntegration: true
       }
     })
     win.loadFile('index.html')　// 加载一个静态html界面
     win.webContents.openDevTools()
   }
   app.whenReady().then(createWindow)
   app.on('window-all-closed', () => {
     if (process.platform !== 'darwin') {
       app.quit()
     }
   })
   app.on('activate', () => {
     if (BrowserWindow.getAllWindows().length === 0) {
       createWindow()
     }
   })
   
   2、创建网页index.html
   <OCTYPE html>
   <html>
   <head>
       <meta charset="UTF-8">
       <title>Hello World!</title>
       <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline';" />
   </head>
   <body>
       <h1>Hello World！</h1>
       我们正在使用节点 <script>文档。 Rite(process.versions.node)</script>,
       Chrome <script>document.write(process.versions. hrome)</script>,
       and Electron <script>document.write(cess.versions.electron)</script>.
   </body>
   </html>
   
   3、配置package.json
   {
       "name": "my-electron-app",
       "version": "0.1.0",
       "main": "main.js" // 指定入口文件为main.js
   }
   "scripts": {
       "start": "electron ." // 指定运行脚本electron
   },
   
   4、打包发布应用程序
   ```

   

4. ##### remote模块

   ```js
   remote模块：electron有且只有一个主进程就是new BrowserWindow创建的主进程，在里面我们加载一个渲染进程win.loadFile('index.html')，但是在index.html渲染进程里面我们又想调用主进程或者操作其他渲染进程界面时怎么办？这时就需要remote模块了。
   1.1、在主进程中，设置nodeIntegration和enableRemoteModule为true,才能使用remote模块。（10.1.2版本之后必须设置）
   webPreferences: {
       nodeIntegration: true,
       enableRemoteModule: true,
       preload: path.join(__dirname, 'preload.js')
   }
   1.2、新开窗口时，获取的是remote下的BrowerWindow!!!
   const demo1Ele = document.querySelector('#demo1');
   const { BrowserWindow } = require('electron').remote;
   window.onload = function(){
       demo1Ele.onclick = () => {
           const demo1Window = new BrowserWindow({
               width: 960,
               height: 700,
               webPreferences: {
                 nodeIntegration: true,
                 enableRemoteModule: true
               }
           })
           demo1Window.loadFile('demo1.html')
       }
   }
   点击按钮demo1Ele后，就会重新打开一个窗口demo1.html
   ```

   

5. ##### Menu模块

   ```js
   菜单栏Menu的自定义，绑定事件，鼠标右键
   2.1、主窗口的menu菜单
   新建一个menu.js
   const { Menu } = require('electron')
   let template = [
       {
           label:'测试标题1',
           click:() => {
               alert(123)
           },
           submenu:[
               {
                   label:'测试标题1-1'
               },
               {
                   label:'测试标题1-2'
               },
           ]
       },
       {
           label:'测试标题2',
           submenu:[
               {
                   label:'测试标题2-1'
               },
               {
                   label:'测试标题2-2'
               },
           ]
       },
   ]
   let nav = Menu.buildFromTemplate(template);
   Menu.setApplicationMenu(nav);
   在主进程main.js中引入
   const mainWindow = new BrowserWindow({
       width: 960,
       height: 700,
       webPreferences: {
           nodeIntegration: true,
           enableRemoteModule: true,
           preload: path.join(__dirname, 'preload.js')
       }
   })
   require('./views/demo1/demo1.js'); // 引入定义好的menu菜单栏即可，是在loadFile之前
   mainWindow.loadFile('index.html')
   2.2、如果是打开的新窗口怎么定义菜单栏呢？（渲染进程）。在打开新窗口（新的渲染进程的时候，配置一个menu，记住是remote模块的Menu）
   配置一个渲染进程的demo1MenuConfig.js
   const { remote } = require('electron')
   let template = [
       {
           label:'测试标题1',
           click:() => {
               alert('123')
           },
           submenu:[
               {
                   label:'测试标题1-1',
                   accelerator:'ctrl+n', //配置快捷键
               },
               {
                   label:'测试标题1-2',
                   accelerator:'ctrl+p',
               },
           ]
       },
       {
           label:'测试标题2',
           submenu:[
               {
                   label:'测试标题2-1'
               },
               {
                   label:'测试标题2-2'
               },
           ]
       },
   ]
   let nav = remote.Menu.buildFromTemplate(template);
   remote.Menu.setApplicationMenu(nav);
   在打开demo1.html之前，引入该demo1MenuConfig.js
   const demo1Ele = document.querySelector('#demo1');
   const { BrowserWindow } = require('electron').remote;
   window.onload = function(){
       demo1Ele.onclick = () => {
           let demo1Window = new BrowserWindow({
               width: 500,
               height: 500,
               webPreferences: {
                 nodeIntegration: true,
                 enableRemoteModule: true
               }
           })
           require('./views/demo1/demo1MenuConfig.js'); // 打开demo1.html之前引入这个Menu配置
           demo1Window.loadFile('./views/demo1/demo1.html')
           demo1Window.on('close', ()=> {
               demo1Window = null;
           })
       }
   }
   这样打开的demo1.html渲染进程的menu菜单栏就被修改了。
   注意accelerator快捷键，只有子菜单才有，一级菜单是没有快捷键的，即便配置了也没有！！！
   如果menu菜单栏被自定义后？怎么打开调试模式？在打开界面之前首先执行下面方法即可！
   demo1Window.webContents.openDevTools(); 
   ```

   

6. ##### shell模块

   ```js
   使用默认应用程序管理文件和 url。
   1、使用shell打开浏览器,并定位到http://www.baidu.com
   const { shell } = require("electron");
   window.onload = function(){
       let aEle = document.querySelector('#atest');
       aEle.onclick = function(e){
           e.preventDefault();
           let href = this.getAttribute('href') || 'http://www.baidu.com';
           shell.openExternal(href);
       }
   }
   ```

   

7. ##### 基本操作

   ```js
   1、鼠标右键配置，在渲染进程中，监听鼠标右键
   在demo1.js中
   const { remote } = require("electron");
   // 自定义一个鼠标右键的菜单栏
   let rightTemplate = [
       { label:'复制',accelerator:'ctrl+c' },
       { label:'粘贴',accelerator:'ctrl+v' },
   ]
   let m = remote.Menu.buildFromTemplate(rightTemplate);
   // 监听鼠标右键
   window.addEventListener('contextmenu',function(e){
       e.preventDefault();
       m.popup({
           window:remote.getCurrentWindow()
       })
   })
   
   2、新开一个子视图，类似iframe嵌套的方式BrowserView
   
   父子窗口的通信
   
   文件操作
   ```

   

8. 虚位以待！！！