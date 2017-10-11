### 1. Meta标签，禁止缩放
```
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;" name="viewport" />
```
### 2. 禁止ios上自动识别电话
```
<meta content="telephone=no" name="format-detection" />
```

### 3. 禁止android上自动识别邮箱
```
<meta content="email=no" name="format-detection" />
```
下面两个是针对ios上的safari上地址栏和顶端样式条的
```
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 听说在ios7以上版本就没效果了 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- 可选default、black、black-translucent 但是我都是用black-->
```
### 4.打电话发短信
```
<a href="tel:020-11811922">打电话给:0755-10086</a>
<a href="sms:10086">发短信给: 10086</a>
```
### 5. css3过渡动画开启硬件加速
缺点：可能会耗电过快
```
.translate3d{
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   -ms-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
}
```
说到这里，也顺便说下动画或者过渡的两个建议：
1.在手机上（其实PC也是一样）。CSS3动画或者过渡尽量使用transform和opacity来实现动画，不要使用left和top。
2.动画和过渡能用css3解决的，就不要使用js。如果是复杂的动画可以使用css3+js（或者html5+css3+js）配合开发，效果只有想不到，没有做不到。

### 6.移动端click屏幕产生200-300 ms的延迟响应
click事件因为要等待确认是否是双击事件，会有300ms的延迟（两次点击事件间隔小于300ms就认为是双击），体验并不好。
现在的解决方案，第一个就是采用touchstart或者touchend代替click。

或者封装tap事件来代替click 事件，所谓的tap事件由touchstart事件+ touchmove（判断是否是滑动事件）+touchend事件封装组成。
![示例图](https://sfault-image.b0.upaiyun.com/240/529/240529230-59c132356973f)
在手机上，click的延迟将近400ms。对于用户而言，是一个很严重的延迟了！所以在手机上，并不建议用click

### 7. img还是background
图片的展示方式有两种，一种是以图片标签显示，一种是以背景图片显示！下面写了这两者的区别。
img:html中的标签img是网页结构的一部分会在加载结构的过程中和其他标签一起加载。
background：以css背景图存在的图片background会等到结构加载完成（网页的内容全部显示以后）才开始加载
也就是说，网页会先加载标签img的内容，再加载背景图片background引用的图片。
引入一张图片，那么在这个图片加载完成之前，img后的内容不会显示。
而用background来引入同样的图片，网页结构和内容加载完成之后，才开始加载背景图片，网页内容能正常浏览，但是看不到背景图片。
至于这两种，大家按照习惯，需求等权重因素选择！
