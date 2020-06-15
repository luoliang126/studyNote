# python笔记

1. ##### python准备

   ```python
   1、安装
   1.1、到python官网：https://www.python.org/downloads/windows/下载最新的安装包
   1.2、得到exe文件一直安装下一步即可。
   1.3、配置，安装的根目录到环境变量。
   1.4、cmd输入python，如果输出>>> 证明进入python运行环境，安装成功！
   1.5、退出python的运行环境：exit()
   ```
   
2. ##### python基础

   ```
   # 基本数据类型
   1、数字类型Number
       # 1-1、整数（包括负数,也可以是十六进制数，八进制，二进制）
       # print (-123)
       # print (0xff00)
       # 1-2、浮点数（就是小数，但精度比整数高）
       # print (1.23)
       # 整数与整数的运算还是整数，浮点数与浮点数的运算还是浮点数，整数与浮点数的运算为浮点数
       # print (2.5 + 10 / 4)
       # 1-3、布尔值(注意False和True的首字母是大写，小写无法识别)
       # print (False)
       # and运算是与运算，只有所有都为 True，and运算结果才是 True。
       # or运算是或运算，只要其中有一个为 True，or 运算结果就是 True。
       # not运算是非运算，它是一个单目运算符，把 True 变成 False，False 变成 True。
       # print (not True)
       # 1-4、复数部分（有待了解）
       # 1-5、空值None（None不能理解为0，因为0是有意义的，而None是一个特殊的空值。）
       # print (None)
   2、字符串String
   	# print ('罗亮')
   3、列表List //可以理解为数组
       fruits=['apple','banana','pear']
       print(fruits[0])   // 'apple'
   4、元祖，是特殊的一种列表，只能访问，不能编辑修改、或者删除元祖里面的内容，但可以删除元祖。
       a=(1,2)
       print(a)  // （1,2）
       print(a[0]) // 1
       #不能编辑
       a[0] = 3 // 抛错，不支持这种赋值方式
       #删除
       del a
       print(a) // a is not defined
       #元祖方法
       计算元祖长度，a中有几个元素
       len(a) // 2
       计算元祖中的最大值
       max(a) // 2
       计算元祖中的最小值
       min(a) // 1
   5、字典 {} // 可以理解为js中的对象
       a = {}
       5-1添加内容
       a['name'] = 'luoliang'
       print(a) // { 'name':'luoliang' }
       5-2访问方式
       print(a['name']) // luoliang
       5-3修改方式
       a['name'] = 'daiqian'
       print(a['name']) // daiqian
       5-4删除属性
       del a['name']
       print(a['name']) // {}
       5-5 len()方法 计算字典中的键值对个数
       len(a)  // 1
       5-6 str()方法 将字典转换为字符串方式
       str(a)  // "{ 'name':'luoliang' }"
       
   #基本使用方法
   1、变量(由字母，数字，下划线组成。和js一样是动态语言，对类型没有强制性要求，基本数据类型是可以相互转换的)
       # a = 1
       # b = 'luoliang'
       # a = b
       # print (b)
   2、if语句
       # score = 45
       # if score>50:
       #     print ('passed')
       # else:
       #     print ('failed')
   3、判断数据的类型 type()
       a=1                 // int
       a1=1.23             // float
       a2=True             // bool
       b='luoliang'        // str
       c=[1]               // list
       d=(1,2)             // tuple
       e={'name':'罗亮'}   // dict
   4、语法
   ```

3. 虚位以待！
