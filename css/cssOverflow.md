# css文本溢出显示

1. ##### 显示省略号......

   ```css
   1、单行文本溢出
   <p class="singleOver">
    	桌花，是指装饰于会议桌、接待台、演讲台、餐桌、几案等场所的花饰。
   </p>
   .singleOver{
       overflow: hidden;
       text-overflow:ellipsis; // 超出部分显示...(需要配合overflow:hidden使用)
       white-space: nowrap; // 文本不会换行，文本会在在同一行上继续。(强制单行显示，这样才会出现...)
   }
   
   2、多行文本溢出
   <p class="over">
   	桌花，是指装饰于会议桌、接待台、演讲台、餐桌、几案等场所的花饰。在实际生活中应用也非常普遍。
       因其常使用花钵作为容器，因此也被称作钵花。桌花一般置于桌子中央(如中餐桌、圆形会议桌和西餐桌等)
       或一侧(如演讲台、自助餐台、双人餐桌等)。桌花可以是独立式或组合式，会议主席台、演讲台等还常结合桌子
       的立面进行整体装饰
   </p>
   .over{
   	display: -webkit-box;
       -webkit-box-orient: vertical;
       -webkit-line-clamp: 3;  // 规定三行显示，超出部分显示 ...
       overflow: hidden;
   }
   ```

2. ##### 纯数字和字母时 参考：http://www.divcss5.com/jiqiao/j777.shtml

   ```css
   css关于 纯数字，纯字母 无法自动换行问题
   问题描述：当全字母或者数字时，div是无法换行的
   <div>dfdfdfhjdfhjdfhjdhfjfhjdfhjd。。。。。。</div>
   解决方案：
   添加强制换行：word-wrap:break-word
   ```

3. ##### 使用js制作

   ```js
   1、获取内容的长度如：60
   2、大于60 代码填充 控制显示 。。。收起/展开
   ```

   