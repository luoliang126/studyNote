# nginx操作说明

1. ##### nginx安装说明

   ```
   1、官网下载：http://nginx.org/en/download.html
   2、解压，安装一步到位！
   ```

2. ##### nginx基本操作

   ```
   1、进入nginx安装根目录
   
   2、找到nginx.exe运行（当然也可以使用命令行）
   
   3、命令行操作，到nginx根目录下（切记cmd要以管理员身份进入）
   配置nginx启动项：找到conf目录下的nginx.conf，配置方式查看百度文档。
   start nginx 或 nginx.exe - 启动命令
   nginx -s reload - 重启
   nginx -s stop 或 nginx -s quit
   注意：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。
   其他更多命令可查阅文档
   
   4、访问：localhost:8888 (需要到nginx.conf中自行配置启动项，以及端口等)
   ```

3. ##### nginx配置多个前端项目

   ```js
   参考：https://www.cnblogs.com/zhaoxxnbsp/p/12691398.html
   	https://blog.csdn.net/weixin_33868027/article/details/92139392
   1、基于域名配置
   
   2、基于端口配置
   
   3、基于location配置
   nginx.conf文件
   # user  nginx;
   worker_processes  1;
   events {
       worker_connections  1024;
   }
   http {
       include       mime.types;
       default_type  application/octet-stream;
       sendfile        on;
       keepalive_timeout  65;
       server {
           listen       8888;
           # server_name  localhost;
           location / {
               root   html;   // 注意根目录使用root
               index  index.html;
           }      
           location /account_manage { 
               alias  html/account_manage; // 子路由使用alias别名
               index index.html;
           }
           location /business_system_manage { 
               alias  html/business_system_manage;
               index index.html;
           }
           ......
       }
   }
   注意：我们将nginx的路由地址修改了，比如访问 localhost:8888/account_manage，那么我们对应的account_manage站点内的路由地址就要变更
   在account_manage中的router.js
   let router = new Router({
       base:'/account_manage/', // 修改后我们才可以在nginx服务的account_manage目录下找到我们的前端静态文件！！！
       mode: 'history',
       routes: routerList
   })
   在vue.config.js中，资源引入问题，原来webpack打包的资源路径都是 ‘./’ ，修改为：
   module.export = {
       publicPath:'/account_manage/'，
       ......
   }
   // 这样我们打包的静态资源文件，在index.html中才会变成 /account_manage/js/.......或者/account_manage/css/......以及其他静态资源
   以上方法就可以直接访问了，例如：localhost:8888/account_manage，在使用路由跳转时localhost:8888/account_manage/xxx路由，但是有个尴尬的问题又来了，假如我们刷新的话，会发现404？怎么会这样？因为nginx静态文件中并没有/account_manage/xxx路由，这个是我们前端项目定义的路由，所以这里需要修改一个nginx.conf文件，参考：https://www.jb51.net/article/148083.htm
   旧路径访问
   location /business_system_manage { 
       alias  html/business_system_manage;
       index index.html;
   }
   新路径访问
   location ^~/business_system_manage { 
       alias  html/business_system_manage;
       index index.html;
       try_files $uri $uri/ /business_system_manage/index.html;
   }
   以上：nginx 本地配置多个前端项目完成！
   ```
   
   
   
4. 虚位以待！