### 通过 visibilityState 的值检测页面当前是否可见
  场景：
  #### 1.页面被切换到其他后台进程的时候， 自动暂停音乐或视频的播放
  #### 2.页面被切换到其他后台进程再回来的时候，刷新页面
  #### 3. 浏览器tab 窗口切换，监听内容的显示与隐藏
  #### 4. 打开新的webview之后，返回该页面，刷新当前页面

```
// 对浏览器做一些兼容性处理：
var hidden, visibilityChange; 
if (typeof document.hidden !== "undefined") { // Opera 12.10 and Firefox 18 and later support 
  hidden = "hidden";
  visibilityChange = "visibilitychange";
} else if (typeof document.msHidden !== "undefined") {
  hidden = "msHidden";
  visibilityChange = "msvisibilitychange";
} else if (typeof document.webkitHidden !== "undefined") {
  hidden = "webkitHidden";
  visibilityChange = "webkitvisibilitychange";
}
document.addEventListener("visibilitychange", function(){
    alert();
}, false);
```
```
var videoElement = document.getElementById("videoElement");

// If the page is hidden, pause the video;
// if the page is shown, play the video
function handleVisibilityChange() {
  if (document[hidden]) {
    videoElement.pause();
  } else {
    videoElement.play();
  }
}
```

```
// 检测是否支持addEventListener 和 Page Visibility API
// Warn if the browser doesn't support addEventListener or the Page Visibility API
if (typeof document.addEventListener === "undefined" || typeof document[hidden] === "undefined") {
  console.log("This demo requires a browser, such as Google Chrome or Firefox, that supports the Page Visibility API.");
} else {
  // Handle page visibility change   
  document.addEventListener(visibilityChange, handleVisibilityChange, false);
    
  // When the video pauses, set the title.
  // This shows the paused
  videoElement.addEventListener("pause", function(){
    document.title = 'Paused';
  }, false);
    
  // When the video plays, set the title.
  videoElement.addEventListener("play", function(){
    document.title = 'Playing'; 
  }, false);

}
```

#### 属性 Document.visibilityState
返回当前文档可见性，在页面预加载或者浏览器tab窗口切换时

##### 返回值
- visible --> 页面内容可能至少部分可见。在实践中这意味着non-minimized窗口的前台页面选项卡。

- hidden --> 页面内容是不可见的。实践中如浏览器窗口多tab切换时，或者操作系统已经锁屏。
- prerender --> 正在prerendered页面内容和用户是不可见的(考虑隐藏document.hidden的目的)。文档可能会在这种状态下,但永远不会过渡到另一个值。注意:浏览器支持是可选的。
- unloaded --> 页面被从内存中卸载。注意:浏览器支持是可选的。

```
document.addEventListener("visibilitychange", function() {
  console.log( document.visibilityState );
  // Modify behavior...
});
```
注：
- 微信内置的浏览器因为没有标签，所以不会触发该事件。
- 手机端直接按Home键回到桌面，也不会触发该事件。
- PC端浏览器失去焦点不会触发该事件，但是最小化，或回到桌面会触发。
