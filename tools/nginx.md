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
   
   3、命令行操作（到nginx根目录下）
   配置nginx启动项：找到conf目录下的nginx.conf，配置方式查看百度文档。
   start nginx 或 nginx.exe - 启动命令
   nginx -s reload - 重启
   nginx -s stop 或 nginx -s quit
   注意：stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。
   其他更多命令可查阅文档
   
   4、访问：localhost:8888 (需要到nginx.conf中自行配置启动项，以及端口等)
   ```

3. 虚位以待！