CSS in JS
CSS in JS 
### 概念由来

最早是2014年11月一个Facebook的员工发布的PPT [React: CSS in JS by vjeux](https://speakerdeck.com/vjeux/react-css-in-js) 

PPT 上列举了 CSS 目前面临的一些问题：
1. Global Namespace （全局命名空间）
2. Dependencies （依赖）
3. Dead Code Elimination （无用代码移除）
4. Minification （压缩）
5. Sharing Constants （共享常量）
6. Bon-deterministic Resolution （定制化）
7. Isolation （隔离）
Vjeux 认为，我们应该使用 Javascript 编写CSS，以获得使用真正的编程语言带来的好处，然后建议我们使用内联样式，他认为类名现在是无意义的，因为我们不再有HTML，都是用JS 和 React 去实现，所以使用内联样式的缺点也就不存在了

### 解决的问题
 #### 全局命名空间
    我们开发了两个 CSS 类，会产生两个全局的 CSS 变量（就是全局的两个类），我们在 JS 中已经了解到全局变量的坏处（如可能被覆盖值、被重定义），但在 CSS 中我们依旧在使用他们。
   在 Facebook，工程师们使用了一种叫 cx（CSS Extension）的工具，为 CSS 提供命名空间，使用如下：
```
 /* button.css */
.button/container {
  padding: 5px;
}
```
 
  相应的，在 jsx/js 中
```
/* button.js */
<div className={cx("button/container")}>
```
#### CSS 文件之间可能存在依赖
   一直以来，我们在开发中需要什么样式，就会通过 require CSS 将 CSS 文件引入。不过一个问题来了，有的时候有些文件已经被其他人 require 了一些 CSS 但你不知道，不过依旧能正常运行（这其实很不安全不是么？！万一哪天 break down 了很难找问题）。

cx 可以支持一些静态分析，帮助你避免这类问题，所以 issue 2，我们也算解决了。

### 事例
`<div class="my-heading">`

to this:

`<div style=styles.myHeading>`

将一个字符串替换为一个变量，只使用内联样式来覆盖样式表解析选择器的匹配，实际上提高了性能。但是，内联样式无法表示伪类，伪元素，以及 media query 等等

————————————————————————————————————————
部件必须是组件 ，高拆分。
缺点：
缺乏高亮支持
解决什么问题，优点，缺点，利弊
