#### document.execCommand() 方法

`document.execCommand()` 是需要调用的关键方法，它可以传入的参数包括 ‘cut’, ‘copy’ 和 ‘paste’ 。

最实用的是 `document.execCommand(‘copy’) `方法。
在调用之前，可以使用` document.queryCommandSupported(‘copy’)` 或 `document.queryCommandEnabled(‘copy’)`方法（这两个方法功能相同）来检测浏览器是否支持复制命令。

需要注意的是，Chrome 虽然支持复制命令的，但在 Chorme 中这两个方法都会返回 `false` 值。

检测 `document.execCommand `方法是否存在也是一个判断方法，但更好的做法是将 `document.execCommand(‘copy’)` 调用放在 `try-catch` 块内。

使用一个按钮作为触发器，添加数据属性（data-copytarget）来指定拷贝内容的目标（#website）：

```
<input type="text" id="website" value="http://www.sitepoint.com/" />
<button data-copytarget="#website">copy</button>
```

```
下面的自执行函数会给所有带有 data-copytarget 属性的元素添加一个点击事件处理方法，去选中区域里的文本并执行 document.execCommand(‘copy’) 。
如果这一操作失败了，文本将保持选中状态同时弹出提示。

```

(function() {

  'use strict';

  // click events
  // 添加点击事件
  document.body.addEventListener('click', copy, true);

  // event handler
  // 添加时间处理方法
  function copy(e) {

    // find target element
    // 搜索目标元素
    var
      t = e.target,
      c = t.dataset.copytarget,
      inp = (c ? document.querySelector(c) : null);

    // is element selectable?
    // 判断元素是否能被选中
    if (inp &amp;amp;&amp;amp; inp.select) {

      // select text
      // 选择文本
      inp.select();

      try {
        // copy text
        // 复制文本
        document.execCommand('copy');
        inp.blur();
      }
      catch (err) {
        alert('please press Ctrl/Cmd+C to copy');
      }

    }

  }

})();

```
