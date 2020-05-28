# flutter笔记

- ##### flutter安装，以及相关需求

   JavaSE安装，并配置环境变量

   下载flutter SDK安装包（官网，github都行），并安装，配置环境变量

   下载安装Android Studio（注意，不要随意升级Android Studio），并配置Android模拟器

   配置编辑器（Android Studio和VS Code都行）
   	Android Studio
   ​	在Android Studio中file-->settings-->Plugins 搜索flutter和dart并安装
   ​	重启Android Studio就可以在启动界面发现多了 start new flutter project
   ​	新建项目一直next

   ​	VS Code（推荐使用！）
   ​	搜索插件flutter，默认dart一起安装了
   ​	在查看>命令面板（View>Command Palette）中选择flutter new project创建一个项目
   ​	根据提示项目创建成功后，会看到对应的项目结构内容，其中lib-->main.dart为项目的入口
   ​	F5 键或调用Debug>Start Debugging，打开调试，会选择一个android模拟设备，已经在android Studio中	设置好

- ##### 项目介绍 (在lib下的main.dart为项目的主入口)

  ```dart
  import 'package:flutter/material.dart';
  void main() => runApp(MyApp());
  class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
          return MaterialApp(
              title: 'hello',
              theme:ThemeData.light(),
              home:Scaffold(
                  appBar: AppBar(
                      title: Text('hello world'),
                  ),
                  body: Center(
                      child: Text('hello world'),
                  ),
              )
          );
      }
  }
  
  1、import 'package:flutter/material.dart'
     material是flutter的一个UI库，里面封装了样式
  2、void main() => runApp(MyApp()); 主运行函数
  3、class MyApp extends StatelessWidget {......} MyApp继承一个StatelessWidget类
     StatelessWidget：是一个静态的部件
     StatefulWidget：是一个动态的部件，根据状态改变 //setState
  4、@override 重写 Widget 视图，返回return 一个MaterialApp组件
  5、title:'hello' 是这个视图的标题
  6、theme:ThemeData.light(), 这是应用的主题色
  7、home：Scaffold(......)  是这个视图的内容区域，主题样式（一个外壳磨具，里面填充组件）
  8、appBar: 头部 导航栏 内容部分
  9、body: 主体内容部分
  10、Center(......) 组件，居中（记住是垂直居中，而不是左右居中）
  11、Text(......)文本组件
  ```

- ##### 常用组件库

  ```dart
  Text(......)文本组件（类似span）
  Text(
      '这是一段文字描述',
      textAlign:TextAlign.left,   // 和css类似，需要注意语法规范
      maxLines:1,   // 最大行数
      // 超出部分的样式 ellipsis省略号，fade从上往下的渐变(淡出的效果，直到消失)
      overflow:TextOverflow.ellipsis 
      style:TextStyle(
          fontSize:25.0,  // 记住这里是浮点数
          // 需要注意的是A是渐变色，与css的RGBA 相反A在最前面
          color:Color.fromARGB(255，255，125，125),  
          decoration:TextDecoration.underline,   // 文字的下划线
          decorationStyle:TextDecorationStyle.solid,	 // 文字下划线（实线）
  	)
  )
  ```

  ```dart
  Container(......) 组件  （类似div）
  child: Container(
          child:Text(
              '测试内容',
              style: TextStyle(fontSize: 40)
      	),
      	alignment: Alignment.topRight,
  	    width:50,
  	    height:50,
  	    // 这里是背景色，注意与decoration中区别，一个contaier只能有一个颜色，否则跑错！
     		color:Colors.yellow    
          // padding内边距， all代表上，下，左，右。
          // const EdgeInsets.fromLTRB(10.0,20.0,30.0,40.0)   LTRB代表左，上，右，下
      	padding: const EdgeInsets.all(10),   
  	    margin:  // 同padding一样
      	decoration: new BoxDecoration(
              gradient: const LinearGradient(
              	// 渐变色，记住在container中，颜色只能有一个，如果定义了2个或以上，
              	// 会跑错不能像css那样覆盖！！！
              	colors:[Colors.lightBlue,Colors.lightGreen,Colors.pink] 
      		),
      		border: Border.all(width:5.0,color: Colors.red)
      	),
  ),
  ```

  ```
  new Image(......) 图片组件
  1、本地图片引用
  在根目录下创建images文件夹存放图片文件。
  在pubspec.yaml中的 flutter: 部分找到assets（之前应该被注释掉了）
  声明
  assets:
    - images/tupian.png // 这里要特别注意-前面有两个空格
  在项目中使用
  body: new Center(
  	child: new Image.asset("images/tupian.png")  //路径要写全
  ),
  
  2、使用线上图片资源
  child:new Image.network(
  	'http://www.baidu.com/xxxx.jpg'
  )
  ```

- ##### 组件的拆分、引用

  ```dart
  定义子组件 （子组件一般是动态的StatefulWidget，需要传参）
  import "package:flutter/material.dart";
  class IndexPage extends StatelessWidget {
      const IndexPage({Key key}) : super(key: key);
      @override
      Widget build(BuildContext context) {
          return Container(
          	child: Text('ceshi'),
          );
      }
  }
  
  父组件 引入，使用
  import 'package:flutter/material.dart';
  import './pages/index_page.dart';   // 需要引入的组件
  void main() => runApp(MyApp());
  class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
          return MaterialApp(
              title: 'hello',
              theme: ThemeData.light(),
              home:IndexPage()  //使用
          );
      }
  }
  ```

- ##### 路由

   ```dart
   路由的跳转方式
   1、
   import './page/login.dart'; // 引入需要跳转的界面
   Navigator.of(context).push(
       MaterialPageRoute(
           builder: (context)=>Login()
       )
   );
   Login()是跳转到的路由界面里面的方法。
   ```

   

