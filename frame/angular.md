# angular笔记

说明：本记录基于angular6及以上内容！

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

- ##### angular基本指令操作

```js
1、创建一个组件
   在当前目录下，创建一个heroes的组件
   ng generate component heroes （简写：ng g c heroes）
   注意：如果是根目录会在app.module.ts中自动引入该组件，并注册。如果是在具体某一文件夹下，则会在最近的			module模块中引入该组件，并注册。
   在项目中直接使用<app-heroes></app-heroes>
                
2、创建一个路由模块
	注意：如果在ng new my-app的时候选择了路由模块的话，会自动生成app-routing.modules.ts，就不需要在		单独创建路由模块了，如果最开始创建项目时没选择router，那么后面需要的时候就要单独创建。
	ng generate module app-routing --flat --module=app  
	// 添加路由模块，并自动在app.module.ts中添加引用。
	
```

- ##### angular组件

