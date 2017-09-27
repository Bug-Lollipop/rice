哪些场景下无法获得上一页referrer信息:

直接在浏览器地址栏中输入地址；
使用location.reload()刷新（location.href或者location.replace()刷新有信息）；
在微信对话框中，点击链接进入微信自身的浏览器；
扫码进入QQ或者微信的浏览器；
直接新窗口打开一个页面； 2017.8.3更新 新版本Chrome测试，新窗口页面依然有document.referrer
从https的网站直接进入一个http协议的网站（Chrome下亲测）；
a标签设置rel="noreferrer"（兼容IE7+）；
meta标签来控制不让浏览器发送referer；
