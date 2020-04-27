# angular笔记

说明：本记录基于angular6及以上内容，放弃angular1！

- ##### 安装angular-cli

```js
1、安装angular-cli脚手架（最好-g全局安装）
	npm install -g @angular/cli
2、创建项目，新建一个文件夹下，执行以下命令
	ng new my-app  // 其中my-app为项目目录
3、启动服务（默认localhost:4200）
	cd my-app
	ng serve --open //其中ng serve是启动服务，--open是自动打开默认浏览器并访问localhost:4200
    ng serve --host 0.0.0.0 --port 4001  //host 0.0.0.0允许通过ip访问,指定4001端口
```

