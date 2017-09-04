### css设置垂直居中

html代码:
```
<div class="parent">
<div class="child">Text here</div>
</div>
```

(1) 行内文本垂直居中

css代码:
```
.parent {
    height: 100px;
    border: 1px solid #ccc; /*设置border是为了方便查看效果*/
}
.child {
    line-height: 100px;
}
```
(2) 行内非文本垂直居中(以img为例)

html代码:
```
<div class="parent">
    <img src="image.png" alt="" />
</div>
```
css代码
```
.parent {
    height: 100px;
    border: 1px solid #ccc; /*设置border是为了方便查看效果*/
}
.parent img {
    //注意此时应该保证图片自身的高度或者你设置的高度小于父元素的200px的行高,不然你看不出来居中的效果.
    line-height: 100px;
}
```
(3) 未知高度的块级元素垂直居中
html代码:
```
<div class="parent">
  <div class="child">
    <!--.child的高度未知,父元素要有高度-->
    sddvsds dfvsdvds
  </div>
</div>
```
第一种方法(不需要加padding):
css代码:
```
.parent {
      width: 100%;
      height: 100%;
      position: relative;

      /*display: table;*/
    }
    .child {
      width: 500px;
      border: 1px solid #ccc; /*设置border是为了方便查看效果*/
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
    }
 ```
第二种方法(不使用transform):
css代码:
```
.parent {
    position: relative;
    width: 100%;
    height: 100%;
}
.child {
  width: 500px;
  border: 1px solid #ccc;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  height: 30%;
  margin: auto;
}
```
第三种方法(需要加padding):
css代码:
```
#parent {
  padding: 5% 0;
}
#child {
  padding: 10% 0;
}
```
第四种方法:
(使用display: table,此种方法也适用于行内文本元素的居中):
css代码:
```
.parent {
  width: 100%;
  height: 100%;
  display: table;
}
.child {
  display: table-cell;
  vertical-align: middle;
}
```
(4) 已知高度的块级元素垂直居中
html代码:
```
<div class="parent">
  <div class="child">
    <!--.child的高度已知,父元素高度已知-->
    sddvsds dfvsdvds
  </div>
</div>
```
css代码:
```
#parent {
  height: 300px;
}
#child {
  height: 40px;
  margin-top: 120px; /*这个只为父元素的高度减去这个元素的高度除以二计算得到的*/
  border: 1px solid #ccc;
}
```
