# css对齐

- ##### 水平居中对齐

```css
// 文本水平居中
text-align:center
// 块级元素水平居中
margin:0 auto
// 转换为行内块级元素，再使用text-align
display:inline-block;
// 转换为flex布局
display:flex
```

- ##### 垂直居中对齐 参考：https://www.cnblogs.com/zhouhuan/p/vertical_center.html

```css
// 文本垂直居中（当行文本）
line-height:50px

// 图片，块级元素垂直居中
vertical-align:middle

// 块级元素垂直居中
1、flex方式
<div class="out">
	<div class="in"></div>
</div>
.out{
    width: 500px;
    height: 500px;
    background-color: blue;
    display: flex;
    align-items: center;/*垂直居中*/
    /*justify-content: center;*//*水平居中*/
}
.in{
    width: 100px;
    height: 100px;
    background-color: red;
}

2、设置父元素相对定位，子元素position: absolute;top: 50%;同时margin-top值为-(子元素高度的一半)，因为设置top值时是相对于盒子顶部，所以想要居中还要往上移动半个盒子的高度才能实现。弊端就是：子元素的高度必须固定！！！
<div class="out">
	<div class="in"></div>
</div>
.out{
    width: 500px;
    height: 500px;
    background-color: blue;	
    position: relative;			
}
.in{
    width: 100px;
    height: 100px;
    background-color: red;
    position: absolute;
    top: 50%;
    margin-top: -50px;
}

3、利用定位（设置上下左右都为0，达到居中效果）
.out{
    width: 500px;
    height: 500px;
    background-color: skyblue;
    position: relative;
}
.in{
    width: 100px;
    height: 100px;
    background-color: salmon;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
}

4、使用transform的translateY属性
.out{
    width: 500px;
    height: 500px;
    background-color: skyblue;
    position: relative;
}
.in{
    width: 100px;
    height: 100px;
    background-color: salmon;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```

- ##### table表格居中

```css
<div id="container">
	<div id="content">content</div>
</div>

#container {
    height: 300px;
    display: table;/*让元素以表格形式渲染*/
}
#content {
    display:table-cell;/*让元素以表格的单元素格形式渲染*/
    vertical-align: middle;/*使用元素的垂直对齐*/
}
```

- ##### flex布局（设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。）

- ##### div右对齐

  ```
  <div algin="center">
  	<div style="width:100px"></div>
      <div style="width:200px"></div>
  </div> 
  div内部的块级元素居中对齐
  right：右对齐
  left：左对齐（默认）
  ```

- 虚位以待！！！