# mysql笔记

- ##### mysql安装  

```html
参考文档：https://www.cnblogs.com/xiaodingdong/p/7223245.html
官网：https://dev.mysql.com/downloads/installer/
注意：选择操作系统和对应的文件。此处安装的参考文档是.msi的文件安装，也有免安装的教程，有空可以试试。
初始化操作：
1、创建my.ini文件
    [mysqld]
    # 设置3306端口
    port=3306
    # 设置mysql的安装目录(注意路径！！！)
    basedir=C:\Program Files\mysql-8.0.18-winx64
    # 设置mysql数据库的数据的存放目录(注意路径！！！)
    datadir=C:\Program Files\mysql-8.0.18-winx64\data
    # 允许最大连接数
    max_connections=200
    # 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
    max_connect_errors=10
    # 服务端使用的字符集默认为UTF8
    character-set-server=utf8
    # 创建新表时将使用的默认存储引擎
    default-storage-engine=INNODB
    # 默认使用“mysql_native_password”插件认证
    default_authentication_plugin=mysql_native_password
    [mysql]
    # 设置mysql客户端默认字符集
    default-character-set=utf8
    [client]
    # 设置mysql客户端连接服务端时默认使用的端口
    port=3306
    default-character-set=utf8

2、初始化，会打印数据库密码，记住该密码，后面会用到
mysqld --initialize --console

3、创建一个mysql服务
mysqld --install localmysql  //这里localmysql是服务的名字，可以不写或者自己命名一个服务
```

- ##### mysql的命令行操作

```mysql
1、启动数据库：net start xxx   
	//xxx代表数据库的名称eg：net start mysql, net start luoliang
2、重新启动数据库：net restart xxx
3、停止数据库：net stop xxx     // net stop mysql, net stop luoliang
4、登陆数据库：mysql –u root –p  enter键后会提示输入密码。   
	//其中root是最高权限用户，也有自己创建的用户，eg：luoliang
    //注意：登录后的数据库操作都应该以 “ ；”结尾。
    //退出登录用 exit;
5、修改数据库用户密码：mysqladmin -u 用户名 -p旧密码 password 新密码
    //注意格式：mysqladmin -u luoliang -p321456987 password 123 （切记一定要在net start mysql之后才能修改）。
    //如果没有初始密码 -p旧密码 这一处可以省略
6、添加用户：CREATE USER 'username'@'host' IDENTIFIED BY 'password';
	//username为创建的用户名eg：daiqian
	//host指定该用户在哪个主机上可以登陆,如果是本地用户可用localhost, 如果想让该用户可以从任意远程主机		登陆,可以使用通配符%。也可以为指定ip，eg：192.168.1.101。
	//password 用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器。或者不传表示没有密		码。
7、删除用户：  DROP USER 'username'@'host';
	//username 用户名
	//host 指定的主机域名ip。 eg：DROP USER 'daiqian'@'localhost';
8、用户的操作权限设定，以及修改密码等操作。
	参考：https://www.cnblogs.com/wanghetao/p/3806888.html
9、创建数据库：create database 数据库名称;  //eg: create database test;
10、删除数据库：drop database 数据库名;     //eg: drop database test;
11、查看数据库：show databases;   //查看所有数据库
12、使用某个数据库：use 数据库名称。 //eg: use test;
13、创建一个表：create table 表的名称(字段一 字段一类型,字段二 字段二类型,、、、、、、);
	//eg：create table users(username char(20),password char(20),sec char(20),age 			int,hobby char(20));
	// 其中users为表的名称
	// username ，password ，sec ，age等为字段的名称
    // char(20) 只能存储长度为20的字符串（字母，汉字等）
	// int 为整形。正整数。更多的mysql数据存储类型，请查看手册
14、查看数据库下的所有table表： show tables;
15、显示数据表结构 describe 表名;  
	//eg:describe users; 另外一种方法：show columns from [表名];
	//此处查看的是表的结构，如表的名称，字段，字段类型，Key键等。不能显示这张表里的内容。必须往表里添加具体数据后，
	通过select * from users，才能查看表里的具体内容。
16、往表里添加数据 insert into 表名(字段一,字段二,字段三) values('张三','男',18);
	// insert into users(username,password,sec,age,hobby) 								   		values('luoliang','123','男',28,'足球'); 其中字段的位置可以随意交换，但必须保持 键值对 一一		对应。
17、表里字段的增加。  
	alter table 表名 add 字段名 字段类型；
	// alter table users add sex char(20)；
18、表里字段的修改。  
	alter table 表名 change 旧字段名 新字段名 字段类型；
	// alter table users change sec sex char(20);
19、表里字段的删除。  
	alter table 表名 drop 字段名；
	// alter table users drop sex;
20、查看表里的数据。 
	select 字段名称一,字段名称二,、、、、、、 from 表名称 where 字段名称 = '根据字段查询的值';
	// 根据username来查找 select username,password,age,hobby from users where sex = '男';
	// 根据username和password来查找 select username,password,age,hobby from users where sex 		= '男' AND password = 123;
21、根据多个来查找时有AND 和 OR 意思为 且 和 或
	// 查询所有数据 select * from 表名;   eg：select * from users;
	// 注意：select username,sex from users where sex = '男';
	查找出的这条数据，应该有username,password,age,hobby,sex这五个字段，但由于输入的查找条件只有		username和sex，所以返回的结果也是只有username和sex两个字段，其他字段不显示。
22、修改表里的数据。 update 表名称 set xx=xx,xxx=xx where xxx=xxx and xxx=xxx;
	// update users set age=28,hobby='踢足球' where username='luoliang' and password=123;
23、删除表里的数据。 
	delete from 表名称 where xx=xxx and xxx = xxx or xxx = xxx;
	// delete from users where age=28 and password = 123;
```

- ##### sql语句

```mysql
从fndictionarykeys表中查找所有数据
SELECT * FROM `fndictionarykeys`

从fndictionarykeys表中查找数据（从第一条开始，查找10条）
SELECT * FROM `fndictionarykeys` limit 10
SELECT * FROM `fndictionarykeys` limit 0,10

从fndictionarykeys表中查找数据（从第二条开始，查找10条）
SELECT * FROM `fndictionarykeys` limit 1,10

指定条件查询（IsOpened=1）的所有数据
SELECT * FROM `fndictionarykeys` where IsOpened=1

指定条件查询（IsOpened=1）的前10条数据
SELECT * FROM `fndictionarykeys` where IsOpened=1 limit 10

查询id在1~4之间的数据
SELECT * FROM `user` WHERE id BETWEEN 1 AND 4;

查询id为（1，2，3）的数据
SELECT * FROM `user` WHERE id IN (1,2,3);

模糊查询（%为通配符，理解为补全缺失的部分）
SELECT * FROM `user` WHERE username LIKE '%r'；//查询用户名以r结尾的人
SELECT * FROM `user` WHERE username LIKE 'r%'；//查询用户名以r开头的人
SELECT * FROM `user` WHERE username LIKE '%r%'；//查询用户名包含r的人

ASC: 升序（默认） DESC: 降序
SELECT * FROM `user` ORDER BY id ASC;//根据id升序查询
SELECT * FROM `user` ORDER BY id DESC;//根据id降序查询
SELECT * FROM `user` ORDER BY id ASC , username;//根据id升序，用户名字母顺序排列，前者权重高于后者，id权重高于用户名

聚合函数max() min() avg() count() sum()
select count(ifnull(id,0)) from student;//查询 id 字段个数，如果为 null，则使用 0 代替
```

