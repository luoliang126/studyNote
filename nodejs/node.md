# nodejs笔记

1. ##### npm

   ```js
   1、npm的基本操作，参考：https://blog.csdn.net/lu_embedded/article/details/79153826
   npm更新自己
   	npm install -g npm   // 将npm更新到最新版本
   search	在存储库中查找模块包
   	npm search change_password  // change_password为包名
   install	使用在存储库或本地位置上的一个package.json文件来安装包	npm install
       npm install express
       npm install express@0.1.1 //安装指定的express的 0.1.1的这个包
       npm install express@latest //安装express的最新包（一般情况不跟latest就是最新的包）
       npm install ../tModule.tgz  //安装压缩包
   	install -g	在全局可访问的位置安装一个包	npm install express -g
   uninstall	卸载一个模块
       npm uninstall express
   remove	删除一个模块
   	npm remove express
   update  更新一个模块到最新版本
   	npm update smart-filer  // 更新smart-filer到最新的版本
   pack	把在一个package.json文件中定义的模块封装成.tgz文件
   	npm pack
   view	显示模块的详细信息
   	npm view express
   publish	把在一个package.json文件中定义的模块发布到注册表，当前与package.json在同一文件夹下的包发布。(在发布包到npm官网上时，常用)
   	npm publish
   unpublish	取消发布您已发布到注册表的一个模块（在某些情况下，还需使用 --force 选项）
   	npm unpublish myModule
   owner 允许您在存储库中添加、删除包和列出包的所有者
   	npm add <username> myModule
   	npm rm <username> myModule
   	npm ls myModule
   whoami	（根据指定注册表模块）打印用户名
   	npm whoami
   adduser	将用户信息添加到当前的开发环境
   	npm adduser
   login	等同于adduser
   	npm login
   logout	将用户信息从当前的开发环境中清除
   	npm logout
   init	初始化Node包的信息，会创建package.json文件
   
   2、nrm的使用，管理npm的不同版本
   
   3、npm发包（基于vue-cli3）
   
   ```

2. 虚位以待！！！