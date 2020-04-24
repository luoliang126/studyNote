# css居中对齐

- ##### 水平居中对齐

```css
// 文本水平居中
text-align:center
// 块级元素水平居中
margin:0 auto
// 转换为行内块级元素，再使用text-align
display:inline-block;
```

- ##### 垂直居中对齐

```css
// 文本垂直居中
line-height:50px
// 图片，块级元素垂直居中
vertical-align:middle
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