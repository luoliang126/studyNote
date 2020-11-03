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

   

4. ##### 基本操作

   ```
   1、electron中remote模块的使用。
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
   
   2、桌面应用程序的菜单栏Menu的使用
   
   ```

   

5. 虚位以待！！！