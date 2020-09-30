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

3、创建一个模块（在布局的时候，有些复杂的界面，需要拆分多个组件完成，此时创建一个模块来单独处理这些拆分的多个组件）
	3.1、ng generate module edit (简写：ng g m edit  //edit是模块名称)
    //这是在当前目录下创建了一个edit的目录，并在该edit目录下生成了一个edit.module.ts的文件
	import { NgModule } from '@angular/core';
	import { CommonModule } from '@angular/common';
    @NgModule({
      declarations: [],
      imports: [
        CommonModule
      ]
    })
	export class EditModule { }
	3.2、cd到该edit模块下，在该模块下创建的组件，就会自动引入到该EditModule中，并暴露出去（也可以手动添加）
    3.3、也可以以该模块命名，一个根组件，然后再创建子组件（edit目录的上一级，执行ng generate component edit）与模块名称一致即可！
    3.4、自定义组件的使用，在ts文件中有selector，即标签名称，绑定即可！
注意：如果前期该页面简单，没有创建模块，后续因为业务的扩展，需要拆分的时候，就需要手动创建模块，然后再挂载！

4、创建公用组件，公用模块等！
	4.1、cd到公用目录下，创建一个公用模块
    ng generate module public-module
	4.2、挂载该publicModule模块
    找到需要挂载的模块eg:components.module.ts
	import { NgModule } from '@angular/core';
	......
	import { TestModule } from './public-module/public-module.module'; // 引入模块
	@NgModule({
        imports: [ ],
        providers: [
            ComponentsService
        ],
        exports: [
            ......
            TestModule //挂载到该模块，并exports出去
        ]
    })
    export class ComponentsModule { } // 统一暴露出去
	注意：这种方式实现了模块的嵌套
    4.3、在每个模块中，创建组件 同上！

5、创建一个服务
```

- ##### angular组件

```js
1、父组件传值子组件（@Input方式）
// 父组件（绑定传值）
<app-eventcool-total #eventcoolTotalModal [detailData]="detailData"></app-eventcool-total>
// 子组件（@Input接收，引入Input模块）
import { Component, OnInit, Input } from '@angular/core'; 
@Input() detailData: object;

2、父组件传递方法给子组件，子组件调用父组件方法
// 父组件中
<app-person-total #personTotalModal (test)="doFatherMethod($event)"></app-person-total>
// 注意这里方法里的$event用于接收参数
doFatherMethod(data){
    console.log('test测试父组件传递方法')
    console.log(data)
}
// 子组件中
import { Component, OnInit,Output,EventEmitter } from '@angular/core';
@Output() test = new EventEmitter();
doMethod(){
    let params = {
        id:123
    }
    this.test.emit(params);
}

3、子组件传值给父组件，父组件获取子组件的值（类似ngModel）
参考：https://segmentfault.com/a/1190000016651999
// 父组件
<app-double-bind [(userName)]="userName"></app-double-bind>
userName:String = 'luoliang'
// 子组件
<div>
    <label for="">UserName:</label>
    <input type="text"  [(ngModel)]="userName" (ngModelChange)="change()">
</div>
import { Component, OnInit,Input, Output, EventEmitter } from '@angular/core';
@Input() public userName;
@Output() public userNameChange = new EventEmitter();
public change(userName: string) {
    this.userNameChange.emit(this.userName);
}

4、子组件传方法给父组件，父组件调用子组件方法。
// 父组件中
// 命名组件名称
<app-person-total #personTotalModal></app-person-total>
// 声明子组件方法
@ViewChild('personTotalModal', null) PersonTotalComponent: any
openPersonTotalModal() {
    this.PersonTotalComponent.changeModalStatus(); //PersonTotalComponent子组件
}
// 调用方法
scanEventEngin(data){
    console.log(data)
    this.openPersonTotalModal();
}
// 子组件中
isShowModal:Boolean = false;
changeModalStatus(){
    this.isShowModal = true;
}

```

- ##### angular使用注意事项！

```js
1、ts语法function
eg:
editData(data){
	if(data){
         this.validateForm.get('keyCode').setValue(data.keyCode);
        this.validateForm.get('keyName').setValue(data.keyName);
        this.validateForm.get('keyDesrc').setValue(data.keyDesrc);
        this.validateForm.get('isOpened').setValue(data.isOpened);
        this.itsDictionaryVals = data.itsDictionaryVals || [];
	}
	this.isShowEdit = true;
}
let params = {
    keyCode:1,
    keyName:'roche',
    keyDesrc:'描述内容',
    isOpened:false
}
this.editData(params); // 正确
this.editData(); // 语法错误
解决办法：声明函数时，data参数可以需要，可以不需要
editData(data?){
	if(data){
         this.validateForm.get('keyCode').setValue(data.keyCode);
        this.validateForm.get('keyName').setValue(data.keyName);
        this.validateForm.get('keyDesrc').setValue(data.keyDesrc);
        this.validateForm.get('isOpened').setValue(data.isOpened);
        this.itsDictionaryVals = data.itsDictionaryVals || [];
	}
	this.isShowEdit = true;
}
this.editData(); // 正确
```

- 虚位以待！！！