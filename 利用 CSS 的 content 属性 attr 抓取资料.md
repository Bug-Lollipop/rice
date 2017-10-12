1、利用 CSS 的 content 属性 attr 抓取资料

需求

鼠标悬浮实现一个提示的文字，类似github的这种，如图：

CSS 黑魔法小技巧，让你少写不必要的JS，代码更优雅

想必大家都想到了伪元素 after ，但是文字怎么获得呢，又不能用 JavaScript 。

CSS 的伪元素是个很強大的东西，我们可以利用他做很多运用，通常为了做一些效果， content:" " 多半会留空，但其实可以在里面写上 attr 抓资料哦！

<div data-msg="Open this file in Github Desktop">  
hover
</div>
div{
width:100px;
border:1px solid red;  
position:relative;
}
div:hover:after{
content:attr(data-msg);
position:absolute;
font-size: 12px;
width:200%;
line-height:30px;
text-align:center;
left:0;
top:25px;
border:1px solid green;
}
在 attr 里面塞入我们在 html 新增的 data-msg 属性，这样伪元素 (:after) 就会得到该值。

最终效果

CSS 黑魔法小技巧，让你少写不必要的JS，代码更优雅

同样的，你还可以结合其他强大的选择器使用，例如： 使用属性选择器选择空链接

显示没有文本值但是 href 属性具有链接的 a 元素的链接：

a[href^="http"]:empty::before {
  content: attr(href);
}
这样做很方便。
