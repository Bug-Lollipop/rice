#### 需求
今天了解到一个新的定位属性： `position:sticky`
很多网站在页面滚动的时候，都希望一直显示侧边栏的区域。
以往我们要去实现这个布局，会去用 `posiiton: fixed` 结合 js 去实现,这就用起来很难受了，
为了解决元素在容器中被滚动超过指定的偏移值时，元素在容器内固定在指定位置这种状态，position:sticky诞生了

#### 表现
position:sticky是一个新的css3属性
它的表现类似`position:relative`和`position:fixed`的合体，
在目标区域在屏幕中可见时，它的行为就像`position:relative`; 
而当页面滚动超出目标区域时，它的表现就像`position:fixed`，它会固定在目标位置。

最关键的是：元素不会脱离文档流，并保留元素在文档流中占位的大小

#### 用法
```
.sticky { 
  position: -webkit-sticky; position:sticky; top: 15px;  
}
```
#### 浏览器兼容性
不咋地。。。只有 chrome 55+ 支持 =_=
