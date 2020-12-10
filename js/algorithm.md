# js算法

1. ##### 数组、字符串、对象的基本操作

   ```js
   数组添加
   1、arr.push(1);		//添加到数组最后，返回push后数组的长度length
   2、arr.unshift(2)	//添加到数组最前面，返回unshift后数组的长度length
   3、arr.splice(2,0,7);	//添加到数组指定位置第二个数之后，0:默认为第二个数，7：插入的数据。无返回值
   eg：let arr = [0,1,2]
   console.log(arr.splice(2,0,7)) // [0,1,7,2]
   
   数组删除
   1、arr1.pop();		//删除数组中的最后一个数据，返回的是删除的那个值
   eg：let arr1 = [0,1,2,3]
   console.log(arr1.pop());  // 3
   2、arr1.shift();	//删除数组中最前方的一个数据，返回的是删除的那个值
   eg：let arr1 = [0,1,2,3]
   console.log(arr1.shift());  // 0
   3、arr1.splice(2,2);	//删除数组中从第二个开始，以后的两个数据，返回的是删除的数组
   eg:let arr1 = [0,1,2,3]
   console.log(arr1.splice(2,2)) //[0,1]
   
   数组的截取
   arr2.slice(1,3)	//截取数组,从第二个开始(包括第二个)，到第四个之间的值(不包括第四个)，并返回截图的值（也是个数组）。原数组不变。
   eg:let arr2 = [0,1,2,3,4,5]
   console.log(arr2.slice(1,3)) // [1,2]
   console.log(arr2) // [0,1,2,3,4,5]
   
   字符串的截取substring
   substring一个参数时，截图的是字符串的第N位数字后面的所有字符（不包含N）
   substring二个参数时，截图的是字符串的第N位到第M位数字之间的所有字符（不包含N，M）
   let str="Hello world!"
   let result = str.substring(3)) // lo world!
   let result = str.substring(3,7) // lo w
   
   数组的合并
   arr2.concat(arr1,[8,9],......); //可以多个合并，返回的是合并后的新数组。原数组并未改变（可用于深拷贝）
   eg:let arr = [0,1,2,3,4,5]
   let arr1 = [6,7]
   console.log(arr.concat(arr1)) // [0,1,2,3,4,5,6,7]
   console.log(arr) // [0,1,2,3,4,5]
   
   数组颠倒顺序
   arr.reverse(); // 只是 颠倒顺序，不做任何排序操作。返回颠倒顺序后的数组
   eg:let arr = [0,1,2,3,4,5]
   console.log(arr.reverse()) // [5,4,3,2,1,0]
   
   数组转字符串
   let array = [1,2,3,4,5];
   array = array.join(); // "1,2,3,4,5"
   array = array.join('') // '12345'
   console.log(array) 
   也可以指定分隔符
   array = array.join("\") // "1\2\3\4\5";
   注意：join方法有返回值，返回值才是join后的结果，但原数组并未改变。
   eg:let arr = [1,2,3,4,5];
   let newArr = arr.join();
   console.log(newArr) // "1,2,3,4,5"
   console.log(arr) // [1,2,3,4,5]
   
   判断 数组/字符串 中是否包含某个值 indexOf / includes
   indexOf: 返回的是对应的索引，如果没有返回的是 -1
   let a = [1,2,3,4,5]
   console.log(a.indexOf(2)); //返回的是2这个元素在数组a中的索引 即1，如果没有的话返回 -1。如果a是一个字符串的话，会将a当成数组来操作，返回对应的索引
   includes:返回的是true,如果没有返回false
   let arr = [1,2,3,4,5]
   let res = arr.includes(2)
   console.log(res) // true
   
   判断数组中，元素是否满足需求 some / every
   some: 判断是否有一个满足即可，返回true/false
   let arr = [1,2,3,4]
   let result1 = arr.some( (i, v) => i < 3)
   console.log(result1)   // true,依据条件判断，数组元素中是否有一个满足，若满足返回true，否则false
   every：判断所有是否满足，返回true/false
   let arr = [1,2,3,4]
   let result2 = arr.every((i, v) => i < 5)
   console.log(result2) // true,依据判断条件，数组的元素是否全满足，若满足则返回ture，否则false
   当逻辑复杂的时候，可以单独拆分
   let ages = [3, 10, 18, 20];
   function checkAdult(age) {
       return age >= 18;
   }
   let result3 = ages.some(checkAdult);
   // 注意，这里调用checkAdult方法名，不需要执行，some方法会自动将每次遍历的item传入 checkAdult(age)的行参age中！！！
   // every同理
   
   数组中所有数据迭代相加reduce/reduceRight求和
   reduce:从左往右相加
   array.reduce(function(previousValue, currentValue, currentIndex, array){
   	return previousValue + currentValue
   }, initialValue);
   参数：
   previousValue 	必选 --上一次调用回调返回的值，或者是提供的初始值（initialValue）
   currentValue 	必选 --数组中当前被处理的数组项
   currentIndex 	可选 --当前数组项在数组中的索引值
   array 		 	可选 --原数组
   initialValue 	可选 --初始值
   let arr = [0,1,2,3,4]
   let num = arr.reduce((preValue, curValue) =>
   	return preValue + curValue
   )
   console.log(num) // 10
   reduceRight：从右往左相加
   参数：与reduce相同
   let arr = [0,1,2,3,4]
   let num = arr.reduceRight((preValue, curValue) =>
   	preValue + curValue
   )
   console.log(num)  // 10
   注意：reduce对于空数组是不会执行回调函数的。
   求最大值
   var max = arr.reduce(function (prev, cur) {
       return Math.max(prev,cur);
   });
   
   数组中查找某个元素find，filter方法。参考：https://blog.csdn.net/lhjuejiang/article/details/80112547
   find()方法为数组中的每个元素都调用一次函数执行(有先后顺序)，当数组中的元素在测试条件时返回true，后面的函数不会执行
   find()返回符合条件的元素，之后的值不会再执行函数。如果没有符合条件的元素则返回undefined。
   var arr = [1,2,3,4,5,6,7];
   var newArr = arr.find(function(elem,index,arr){
       return elem > 5;
   });
   console.log(newArr); //6
   注：提一下findIndex，find返回的是找到的值value，而findIndex返回的是该值value在数组中对应的索引值index
   let arr = [1,2,3,4,5]
   let arr1 = arr.findIndex((value, index, array) => value > 3)
   console.log(arr1)  // 3
   filter(): 创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素，如果一个都没有就返回一个空数组 []
   var arr = [1,2,3,4,5,6,7];
   var ar = arr.filter(function(elem,index,arr){
       return elem > 5;
   });
   console.log(ar); // [6,7]
   由此可见find是找到一个，然后就返回该元素。filter是赛选出所有符合条件的元素，并返回一个新的数组。
   
   数组的循环中断
   for(var i=0;i<3;i++){
       if(i>1){
           alert('满足条件了')
           break;
       }
       console.log(i); // 只有0,1，alert后面的因为break不再执行，中断循环操作
   }
   forEach没有中断一说，但可以使用try...catch抛错实现中断
   try{
       const arr = [0,1,2,3,4,5];
       arr.forEach(item => {
           if(item>3){
               throw new Error('满足条件了')
           }
           console.log(item); //0,1,2,3 后面没有了，因为抛错了，走catch
       })
   }catch(error){
       console.log(error); // Error:满足条件了
   }
   当然其他的遍历也可以使用这种方式
   
   判断 字符串 是否以 xxx 开头 startsWith()，返回的是boolean
   let a = 'luoliang'
   console.log(a.startsWith('luo'))  // true
   
   字符串转数组
   let str = 'ab+c+de';
   let a = str.split('+');
   let b = str.split('');
   console.log(a) // [ab, c, de]
   console.log(b) // [a,b,+,c,+,d,e]
   console.log(str) // 'ab+c+de' 
   注意：split拆分有返回值，返回值才是拆分后的结果，但原字符串并未改变。
   特殊使用案例，使用位运算符实现：
   let demo = "hello"
   let str = [...demo];
   console.log(str);// ["h", "e", "l", "l", "o"]
   
   字符串替换replace
   1、replace字符串替换方法！
   eg：let a = "aaabbbccc"
   let b = a.replace("b","1")
   console.log(b)  //  b -> aaa1bbccc 而这时的a -> aaabbbccc，不变！！！
   上面我们可以看到replace值替换了 第一个，其余的未替换，如果想全部替换怎么办？
   解决办法：结合正则匹配
   let a = "aaabbbccc"
   let b = aaa.replace(/b/g,'1') //    /s/g 全局匹配字符串中含有s的字符串
   console.log(b)  // b -> aaa111ccc
   2、假如 需要替换内容也是一个变量怎么办？
   let ccc = "变量";
   let reg = "/"+ccc+"/g";
   let str = "这是一个变量，这是一个变量";
   var val = str.replace(eval(reg),"替换"); // 这里使用eval函数，计算函数内部的值
   console.log(val); // val -> "这是一个替换，这是一个替换"
   也可以使用：
   let ch = "/";
   let str = "这是一/个变量，这是一个变量";
   let val = str.replace(new RegExp(ch,'g'),"b");
   console.log(val);  // val -> "这是一b个变量，这是一个变量"
   
   对象的访问方式：
   var test = {
       name:'luoliang'
   }
   1、‘.’访问方式
   console.log(test.name);
   2、‘ [] ’数组访问方式
   console.log(test['name']);
   两者的区别：总的来说性能各个方面区别不大，但各自有特定的访问地方。如：test['name']，当我们的key键值为一个数字时，就不能用‘.’访问，而只能用‘[]’;
   var test = {
       123:'luoliang'
   }
   console.log(test.123)      // 抛错
   console.log(test['123'])   // luoliang
   
   对象权限设置
   var person = {};
   Object.defineProperty(person, 'name', {
       configurable: false, //表示能否使用delete操作符删除从而重新定义，或能否修改为访问器属性。默认为true;
       Enumberable:true, //表示是否可通过for-in循环返回属性。默认true;
       writable: false, //表示是否可修改属性的值。默认true;
       value: 'Luoliang' //表示name的属性值
   });
   console.log(person.name);//Luoliang
   delete person.name; //不能删除掉
   console.log(person.name);//Luoliang
   person.name = 'bobo'; //也不能修改掉
   console.log(person.name);//Luoliang
   
   对象的判断
   1、constructor：每个实例化对象都有一个constructor指向构造他们的构造函数
   class Person {
       constructor(){}
       getName(){}
   }
   let luoliang = new Person();
   console.log(luoliang.constructor); // Person
   2、instanceof:判断某个对象是否属于另外一个对象，或被另外一个对象所创建。要求其左边的是一个对象，右边是对象类的名字或构造函数
   console.log(luoliang instanceof Person); // true
   const arr = [];
   console.log(arr instanceof Array) //true其他为false，可用于区别object和array
   3、typeof：返回结果是一个基本数据类型。如："number"，"string"，"boolean"，"object"，"function"，"undefined"
   typeof Person // 返回function，因为它是一个构造函数
   typeof luoliang // 返回object
   typeof 1 和 typeof NaN // 返回 number
   typeof 'test' // 返回 string
   typeof true   // 返回 boolean
   typeof Null、typeof undefined  // 返回 undefined
   4、isPrototypeOf: 判断某个proptotype对象和某个实例之间的关系
   console.log(Person.prototype.isPrototypeOf(luoliang)); // true
   5、hasOwnProperty：用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。
   console.log(luoliang.hasOwnProperty("getName")); // false，因为getName属性是从原型Person的prototype中继承过来的
   luoliang.getName = () => {}
   console.log(luoliang.hasOwnProperty("getName")); // true，这是可以在实例luoliang对象上添加getName方法
   6、in：用来判断，某个实例是否含有某个属性（包括自身属性和prototype上的属性）
   console.log('getName' in luoliang) //true， luoliang本身已添加getName方法，继承的Person上也有getName方法(两种都行，使用时会用luolian本省的getName)
   7、判断一个对象是否为空对象？
   let temp = {}
   JSON.stringify(temp) == '{}' // 转换字符串方式
   Object.keys(temp).length == 0 // 获取对象的keys,返回一个数组，如果为空则为[]，再判断其length值
   
   对象的合并
   obj1={a:1}
   obj2={b:2}
   1、for in循环赋值
   for(var key in obj2){
       //此处hasOwnProperty是判断自有属性，使用for in循环遍历对象的属性时，原型链上的所有属性都将被访问会避免原型对象扩展带来的干扰
       if(obj2.hasOwnProperty(key)===true){
           obj1[key] = obj2[key]; // 如果相同，则用obj2的覆盖obj1
       }
   }
   console.log(obj1); // {a:1,b:2}
   2、使用Object.assign(obj1,obj2,......)，是深拷贝的合并，而不是简单的变量引用！！！
   for in 方法，在处理多个对象合并的时候就不那么友好了，解决办法使用Object.assign(target,sources1,sources2,...)方法。
   该方法是将sources源合并到target中，并返回target对象。
   let obj = Object.assign(obj1,obj2,......)
   console.log(obj); // {a:1,b:2}
   但该方法也存在一个问题obj1也被修改成了{a:1,b:2}
   优化方案，修改target为空对象
   let obj = Object.assign({},obj1,obj2,......)
   console.log(obj); // {a:1,b:2}  此时的obj1还是{a:1}
   obj,obj1,obj2是相互独立的变量和变量地址，如果修改各自的属性，都不会影响其他。
   3、使用...展开符，再使用一个新对象包含
   let obj3={a:3}
   let newObj = {
       ...obj1,
       ...obj2,
       ...obj3
   }
   console.log(newObj) // {a:3,b:2} 合并三个对象，如有相同靠后的会覆盖靠前的
   ...操作符：用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中，实际上就是Object.assign()操作
   ```

2. ##### 数组的遍历

   ```js
   for循环方法
   var arr = [1,2,3,4,5,6];
   for(var i=0;i<arr.length;i++){
       ......
   }
       
   for in循环方法(可以遍历一个对象，获取key和value)
   for(var i in arr){
       console.log(i,arr[i])
   }
   let object1 = {name:'luoliang',age:29}
   for(var i in object1){
   	console.log(i,object1[i]) // i是key，object1[i]是value
   }
       
   forEach循环方法（操作的是arr数组本身，没有返回值）
   arr.forEach(function(item,index,arry){ //item是对应的arr里面实际的值，index是arr的索引，arry是arr本身
       return arr[index] = item*2;
   })
       
   map方法（创建一个新数组，用来保存返回值）
   let newArr = arr.map(function(item,index,arry){ //item是对应的arr里面实际的值，index是arr的索引，arry是arr本身
   	return item*2;
   })
   console.log(newArr) // [2,4,6,8,10,12]
   console.log(arr) // [1,2,3,4,5,6] 原数组未改变
   forEach和map的总结：
       能用forEach()做到的，map()同样可以。反过来也是如此。
       map()会分配内存空间存储新数组并返回，forEach()不会返回数据。
       forEach()允许callback更改原始数组的元素。map()返回新的数组。
       map执行效率大于forEach
   
   Object.keys()，Object.values()方法。数组、字符串返回索引，对象返回key
   数组时
   let arr = [9,5,2,3,6]
   Object.keys(arr)  // ["0", "1", "2", "3", "4"]
   对象时
   let obj = {name:'luoliang',age:18,sex:'男'}
   Object.keys(obj)  // ["name", "age", "sex"]
   Object.values(obj) // ['luoliang',18,'男']
   字符串时
   let str = 'luoliang'
   Object.keys(str)  // ["0", "1", "2", "3", "4", "5", "6", "7"]
   ```

3. ##### 数组去重

   ```js
   数组去重1 (用数组中前一个数，分别与后面的数相比较，重复就剔除，会修改原数组)
   var arr = [2,56,4,89,4,343,465,67,2,3,5,7,565,123];
   for(var i=0;i<arr.length;i++){
       for(var j=arr.length;j>i;j--){
           if(arr[i] == arr[j]){
               arr.splice(j,1);
           }
       }
   }
   console.log(arr);
   
   //数组去重2 （新建一个空数组，如果arr数组中的数，没在newArr中，就向newArr中push该数）
   var arr = [2,56,4,89,4,343,465,67,2,3,5,7,565,123];
   var arrNew = [];
   for(var i = 0;i<arr.length;i++) {
       for (var j = 0; j < arrNew.length; j++) {
           if (arr[i] == arrNew[j]) {
               break;
           }
       }
       if (j == arrNew.length) {
           arrNew.push(arr[i]);
       }
   }
   console.log(arrNew);
   
   //数组去重3 (先将数组排序，然后再相邻两个进行比较，不同则选出来，相同则不作为。该方法会有一个先排序的动作，使用时根据需要选用)
   var arr = [2,56,4,89,4,343,465,67,2,3,5,7,565,123];
   var arrNew = [];
   arr = arr.sort(function (a,b) {
       return a - b;
   });
   for(var i=0;i<arr.length;i++){
       if(arr[i] != arr[i+1]){
           arrNew.push(arr[i])
       }else{
   
       }
   }
   console.log(arrNew);
   
   //数组去重4，将数组转化为对象，并对该值赋值1。比如：arr=[1,2,3] 转为对象: temp={"1":1,"2":1,"3":1}，判断temp对象是否有该属性即可。
   var arr = [2,56,4,89,4,343,465,67,2,3,5,7,565,123];
   var temp = {};
   var arrNew = [];
   for(var i = 0;i<arr.length;i++){
       if(!temp[arr[i]]){ //此处的temp对象不能使用 . 操作，因为对象的键是一个number类型;
           temp[arr[i]] = 1; //temp对象没有该属性，就添加。
           arrNew.push(arr[i])
       }
   }
   console.log(arrNew);
   
   数组去重5，配合Array.from和new Set。// 该方法不仅能去重，还能区别null，undefined，NaN
   let arr = [1,23,4,5,2,1,1,null,null,undefined,undefined,NaN,NaN]
   let res = Array.from(new Set(arr))  // new Set后，并非是一个数组，所以使用Array.from类似数组的对象转换为数组
   let res1 = [...new Set(arr)] // new Set之后，使用展开符一样可以实现类数组转换为数组
   console.log(res); //[1, 23, 4, 5, 2, null, undefined, NaN]
   
   数组去重6，多数组的合并去重
   let arr1 = [1, 2, 3, 4]
   let arr2 = [2, 3, 4, 5, 6]
   let set1 = new Set([...arr1, ...arr2])
   console.log(set1)  // {1,2,3,4,5,6}
   注意：数据结构Set的更多用法，参考：https://www.cnblogs.com/kongxianghai/p/7250248.html
   
   数组去重7，使用reduce方法
   var newArr = arr.reduce(function (prev, cur) {
       prev.indexOf(cur) === -1 && prev.push(cur);
       return prev;
   },[]);
   ```

4. ##### 数组排序算法

   ```js
   1、sort方法(按照数字0-9，字母A-Za-z排序，数字在字母前面，大写的字母在小写的字母前面)。
   let arr = [8,2,'luo','Luo',12]
   arr.sort(); // [12, 2, 8, "Luo", "luo"]
   这里提一下8,2,12 是根据数字最高位（第一位）来排序的，而不是本身数值的大小，很明显我们要的不是这种效果
   解决办法：使用一个函数来处理
   function sequence(a,b){
       if (a>b) {
           return 1;
       }else if(a<b){
           return -1
       }else{
           return 0;
       }
   }
   console.log(arr.sort(sequence)); // [2, 8, "Luo", "luo", 12]
   后期演化成：
   arr.sort(function(a,b){
       return a-b;
   })
   因为：若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
   若 a 等于 b，则返回 0。
   若 a 大于 b，则返回一个大于 0 的值
   
   2、冒泡排序
   let arr = [23,1,56,2,7,8,9,1,5]
   for (var i = 0; i < arr.length; i++) {
       for (var j = 0; j < arr.length - 1 - i; j++) {
           if (arr[j] > arr[j+1]) { //相邻元素两两对比
               var temp = arr[j+1];
               arr[j+1] = arr[j];
               arr[j] = temp;
           }
       }
   }
   console.log(arr);
   
   3、选择排序，顾明思议就是 每次挑选出最大/最小值，然后放入一个存放数组中，（并将它从原数组中删除，这样可以减少循环时间）
   
   4、二分查找法(在数据量较少时，不推荐使用效率不高，大量数据推荐使用！)
   ```

5. ##### Math对象

   ```js
   parseInt(5/2)  //丢弃小数部分,保留整数部分
   Math.ceil(5/2)  //向上取整,有小数就整数部分加1
   Math.round(5/2)  //四舍五入取整
   Math.floor(5/2)  //向下取整
   Math.round(Math.random()*9+1); //1-10之间的随机数
   Math.round(Math.random()*12+2); //2-14之间的随机数
   Math.abs(x)	// 返回数的绝对值
   
   Math三角函数对象
   acos(x)	返回数的反余弦值
   asin(x)	返回数的反正弦值
   atan(x)	以介于 -PI/2 与 PI/2 弧度之间的数值来返回 x 的反正切值
   atan2(y,x)	返回从 x 轴到点 (x,y) 的角度（介于 -PI/2 与 PI/2 弧度之间）
   ceil(x)	对一个数进行上舍入。
   cos(x)	返回数的余弦
   exp(x)	返回 e 的指数。
   floor(x)	对一个数进行下舍入。
   log(x)	返回数的自然对数（底为e）
   max(x,y)	返回 x 和 y 中的最大值，max(1,2,65,21,0,5)可以传递多个参数
   min(x,y)	返回 x 和 y 中的最小值，min(1,2,65,21,0,5)可以传递多个参数
   pow(x,y)	返回 x 的 y 次幂
   random()	返回 0 ~ 1 之间的随机数
   round(x)	把一个数四舍五入为最接近的整数
   sin(x)	返回数的正弦
   sqrt(x)	返回数的平方根
   tan(x)	返回一个角的正切
   ```

6. ##### 递归

   ```js
   let arr1 = [{
       codeName:'ceshi123',
       codeValue:123,
   },{
       codeName:'ceshi456',
       codeValue:456,
       children:[
           {
               codeName:'456-1',
               codeValue:4561
           },
           {
               codeName:'456-2',
               codeValue:4562,
               children:[
                   {
                       codeName:'456-2-1',
                       codeValue:45621
                   }
               ]
           },
       ]
   }]
   let tempFn = function(data){
       let result = [];
       if(data.length){
           data.forEach(jtem => {
               let tempObj = {
                   label: jtem.codeName, 
                   value: jtem.codeValue,
                   children:null
               }
               if(jtem.children && jtem.children.length){
                   tempObj.children = tempFn(jtem.children)
               }else{
                   delete tempObj.children;
               }
               result.push(tempObj);
           })
       }
       return result;
   }
   let temp = tempFn(arr1);
   // 递归：适用于多级嵌套数组，如果想批量在每一级添加/修改/删除动作时使用。关键在于条件判断和函数的返回值！
   
   // 为每一级添加一个id，定义的递归函数传递一个当前index
   let tempFn = function(data,index){
       let result = [];
       if(data.length){
           data.forEach(jtem => {
               let tempObj = {
                   label: jtem.codeName, 
                   value: jtem.codeValue,
                   index:index,
                   children:null
               }
               if(jtem.children && jtem.children.length){
                   tempObj.children = tempFn(jtem.children,index+1)
               }else{
                   delete tempObj.children;
               }
               result.push(tempObj);
           })
       }
       return result;
   }
   let temp = tempFn(arr1,0);
   
   ```

7. ##### js实现惯性滚动，下拉回弹效果

8. 虚位以待！！！
