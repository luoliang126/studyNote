# mongo笔记

- ##### mongo的安装--官网"https://www.mongodb.com/"

```js
1、安装mongodb：此处为window环境下。到官方网站下载。记住自己的操作系统和计算机详细情况：如32位or 64位。下载后是一个后缀名为.msi的文件。然后安装选择目录即可
2、MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要手动创建它。请注意，数据目录应该放在根目录下（(如： C:\ 或者 D:\ 等 )。所以配置好目录后应该是这样的：D:\data\db
3、为了从命令提示符下运行MongoDB服务器，你必须从MongoDB目录的bin目录中执行mongod.exe文件。注：也可以将bin目录配制到全局环境中，这样方便启动，首次启动过后你会发现D:\data\db目录瞬间多了300M左右，这是因为我们将数据库配制好了。
4、如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo.exe文件，MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境，即通过cmd命令行方式来操作数据库的‘增删改查’,默认进入mongose的命令行操作会自从创建一个名为test的数据库。
注意：mongodb的命令行操作:文档的数据结构和JSON基本一样。所有存储在集合中的数据都是BSON格式。BSON是一种类似json的一种二进制形式的存储格式,简称Binary JSON。
```

- ##### mongo的命令行操作

```mysql
1、查看所有数据库：show dbs
2、创建数据库：use luoliang 有两层意思：如果存在luoliang数据库就切换到该数据库下，如果没有，则创建一个luoliang数据库
3、查看当前数据库中所有的集合：show collections
4、创建一个集合：
	db.createCollection("mycol", { capped : true, size : 6142800, max : 10000 } )
	capped布尔（可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会		自动覆盖最早的文档。当该值为 true 时，必须指定 size 参数。
    size数值（可选）为固定集合指定一个最大值（以字节计）。如果 capped 为 true，也需要指定该字段。
    autoIndexID	布尔（可选）如为 true，自动在 _id 字段创建索引。默认为 false。（试了会抛错，一般不用这个字段）
    max	数值（可选）指定固定集合中包含文档的最大数量。
5、插入数据：
	db.test.insert({"username":"luoliang"})
	db.test.save({"username":"bobo","password":"123"})
	//注意：此处的意思是：向luoliang数据库中的test集合中插入一个{"username":"luoliang"}，
	//同理：如果luoliang数据库中不存在test集合，那么就直接创建一个test集合
6、查看数据：
	db.test.find() //查询出test集合下的所有文档
	db.test.find().pretty() //查询出来的显示格式更佳优雅，方便查看
	db.test.count()  //查看test集合下的所有条数 返回一个number
7、条件查询： 
	等于db.test.find({"title":"MongoDB 教程"}).pretty()	
	//查询test集合中title = MongoDB 教程的这条数据
	小于db.test.find({"likes":{$lt:50}}).pretty()	//查询 likes < 50
	小于或等于db.test.find({"likes":{$lte:50}}).pretty()	//查询 likes <= 50
	大于db.test.find({"likes":{$gt:50}}).pretty()	//查询 likes > 50
	大于或等于db.test.find({"likes":{$gte:50}}).pretty()	//查询 likes >= 50
	不等于db.test.find({"likes":{$ne:50}}).pretty() //查询 likes != 50
	"且" 条件查询（多个条件查找）
    	eg：db.test.find({title:"MongoDB 教程",likes:100})
	"或" 条件查找
    	eg: db.test.find({$or:[{key1: value1},{key2:value2}]}).pretty()
	"或"和"且" 的混用
	    eg: db.test.find({"likes": {$gt:50}, $or: [{"description": "MongoDB 是一个Nosql数据				库"},{"title": "MongoDB 教程"}]}).pretty()
8、更新数据： db.test.update({title:"MongoDB 教程"},{$set:{title:"MongoDB"}}) 
	//注意：只能修改查找到的第一条数据，如果要需要修改所有数据库中 title:"MongoDB 教程" 的数据
	db.test.update({title:"MongoDB 教程"},{$set:{title:"MongoDB"}},{multi:true}) 
	//修改数据库中所有 title:"MongoDB 教程" 的数据
	db.test.update({title:"MongoDB 教程"},{$set:{price:"￥48"}}) 
	// 添加一个price字段
9、删除 数据库 > 集合 中的一条数据：
	db.test.remove({age:14}) 
	// 注意如果不是唯一的id，那么代表删除所有age:14的数据
10、删除 数据库 > 集合test中的所有数据：db.test.remove({ })
11、删除 数据库中的 test 集合：db.test.drop()
12、删除 整个数据库: db.dropDatabase();
13、数据排序: 
	按照升序 db.test.find().sort({age:1}).pretty()   
	//将test集合中所有的数据按照 age 的升序方式排序，并格式化显示出来
	按照降序 db.test.find().sort({age:-1}).pretty()
14、分页 : db.test.find().sort({age:1}).limit(3).skip(3)   
	//limit代表取多少个document，skip代表跳过前多少个document。注意分页为什么必须要先排序再分页？因为		如果不排序就分页的话，每次查询到的数据顺序不一样，那分页就可能出现重复的数据重复查找出来
15、获取数据的数量：
	db.test.count({name:null})//获取数据中name为null的个数，返回一个number
	db.test.find({name:null}).count()
16、退出数据库操作：exit
```

