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

### 如何解决
 #### 1.全局命名空间
 
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
#### 2.CSS 文件之间可能存在依赖
   一直以来，我们在开发中需要什么样式，就会通过 require CSS 将 CSS 文件引入。不过一个问题来了，有的时候有些文件已经被其他人 require 了一些 CSS 但你不知道，不过依旧能正常运行（这其实很不安全不是么？！万一哪天 break down 了很难找问题）。

cx 可以支持一些静态分析，帮助你避免这类问题，所以 issue 2，我们也算解决了。
#### 3.消除 Dead Code

因为cx是唯一生成样式的方式，如果使用了 cx，那么 css 和 className 中使用的类名都一致了！如果什么时候 className 不再使用了，同时干掉 css 里的就好了。
#### 4.压缩
通过cx可以最小化所有的classname，让js和css文件更小，同时cx 会对类名替换，酷，这样你连 CSS 的名字都猜不出来了
#### 5.共享常量
这里说的是CSS 和 JS 之间共享常量
Facebook 提出了 “CSS Variable” 的方案，通过一个 php 文件把 CSS 里的常量导出为一个 array，为 JS 暴露一个叫 cssVar 的函数来调用。
```
.button/container {
  padding: var(button-padding);
}
```
```
/* CSSVar.php */
$CSSVar = array(
  'button-padding' => 5
);
```
```
var buttonPadding = cssVar('button-padding');
```
这样就实现了 CSS 和 JS 之间共享常量了
以上的 5 个问题，其实（FB）现有的工具都可以 handle，那么下面两个问题，就不那么好解决了。
#### 6.非确定性的样式
在一个 CSS 文件中，如果你对某一个样式定义了多次，属性有重复，后者就会覆盖前者的样式定义
```
.small { font-size: 12px; }
html { font-size: 12px; }
.small { font-size: 22px; }
```
上面 `<div lcass="small"> 你好</div> ` 最终的字体大小是22px,这种方式常用于我们去自定义覆盖组件的样式，比如我们基于Element 组件 做一些修改，定制，用自己的CSS 样式文件去覆盖某些选择器上已有的样式。
这个不是重点，重点是不是在所有的情况下，CSS 文件只有一个。如果某一个样式写在不同的文件，而这些文件是异步加载的，浏览器收到统一选择器的样式顺序就不一定了。
#### 7.隔离
假设我们有一套基础的 UI 库，可以给大家用，例如Element。包含 NavMenu、messageBox、Pagination 等，每个组件都做了很多场景的展现，不过当设计和开发希望加入一些新特性的时候，我们可能需要和提出此需求的团队沟通，看看怎么样才比较合适，实现他们的需求。
最后搞定了，开发人员写了很多trick 语法来兼容各个团队的需求，但是，后期可能会发现，新需求影响了组件内的很多东西，因为这样做很容易破坏各个选择器之间的隔离性

```
<Menu className={cx("menu")}>
  <MenuList order="1" />
  <MenuList order="2" />
  <ButtonGroups />
</Menu>
```
```
.menu > a {
  /* blablabla... */
}
```
当我们改动了 menu 这个样式，谁知道会发生什么呢？<a> 最终影响的到底是 list 呢还是 button group 呢？如果我们的 dom 结构再变化，还会影响什么呢？要靠猜的吧！hhh...

### CSS in JS
上面的问题，前5个都可以解决，但是6和7 却不是很好去解决，继续引入CSS，反而会将情况变得更复杂。尝试一下用 inline-style 写 React 样式
```
var styles = {
  container: {
    padding: 2,
    backgroundColor: '#eee',
  },
  depressed: {
    borderRadius: 2,
  }
}
<div style={styles.container}></div>

```
除了结构化的 JS 对象，以及 CSS 属性变成了驼峰命名，px 值变成了数字，inline style 好像没那么糟糕了，不是吗？因为这里的 inline，并不是变成了 style="a:1; b:2; c:3;" 这样难以辨认的字符串，而是让你用 js 的对象来定义，只在 React 组件的 style 属性那里传入一下样式的引用。JS 对象，就好像映射了一个个 CSS 的 class，而 inline style refer，就是一个隔离的好办法。
当引入 JS 后，玩法就更多了。我们甚至可以加入逻辑，比如函数、条件等等
```
propTypes: {
  isDpressed: React.PropTypes.bool
}
<div style={Object.assign(
  styles.container,
  this.props.isDpressed && styles.depressed
)}>
</div>
```
用以上的代码，我们已经完成了一个样式根据状态的变化：当变成 isDepressed (被按下) 的状态，就应用 depressed 的样式。

### PPT总结
综上所述可以理解为，FB 发现 CSS 不好解决的问题可以通过 JS 来解决，所以用了 JS。而不是强行必须照搬 Web 开发原有的一套。React 在 Web 端其实还是可以使用 CSS 的，用 className 的话。但 React Native 就不行了，但有了这样的样式设计，无论是为了解决问题，还是减少 RN 实现复杂度，都有极大的帮助，所以 RN 最后的官方 doc 说的是压根没实现 CSS。(我并不知道他们以后打不打算实现 CSS)
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
