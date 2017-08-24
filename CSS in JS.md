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
上面的问题，前5个都可以解决，但是6和7却不是很好去解决，继续引入CSS，反而会将情况变得更复杂。尝试一下用 inline-style 写 React 样式
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
除了结构化的 JS 对象，以及 CSS 属性变成了驼峰命名，px 值变成了数字，inline style 好像没那么糟糕了，不是吗？因为这里的 inline，并不是变成了 style="a:1; b:2; c:3;" 这样难以辨认的字符串，而是让你用 js 的对象来定义，只在 React 组件的 style 属性那里传入一下样式的引用。JS对象，就好像映射了一个个 CSS 的 class，而 inline style refer，就是一个隔离的好办法。
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
综上所述可以理解为，FB 发现 CSS 不好解决的问题可以通过 JS 来解决，所以用了 JS。而不是强行必须照搬 Web 开发原有的一套。React 可以在 Web 端用className去使用 CSS 。但 React Native 就不行了，但有了这样的样式设计，无论是为了解决问题，还是减少 RN 实现复杂度，都有极大的帮助。

### 优点
Mark Dalgleish 写的 [A Unified Styling Language](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660) 文章，阐述了CSS in JS 的一些优点，主要概括为一下几项：
 1. 范围约束
 2. 关键样式 (Critical CSS)
 3. 更智能的优化
 4. 包管理
 5. 非浏览器平台样式 (Non-browser styling)
 #### 1.范围约束
  CSS 社区投入了很大精力想解决这个问题, 通过一些方式增加样式的可维护性, 例如 OOCSS, SMACSS 等. 其中最流行的是来自 Yandex 的 BEM, Block Element Modifier.
  BEM( 纯 CSS 实现)只是一个命名约定, 通过像是 .Block__element--modifier 的 class 限制样式. 在 BEM 的项目中, 开发者不得不在所有场合遵循 BEM 规则, 能做到这一点, BEM 是很好用的, 但范围限制只能以纯粹的规范来约束么?
实际上， 大多数的CSS in JSS 都是遵循BEM的，用不同的方式将样式定位到某一元素中，例如：
```
import { css } from 'glamor'
const title = css({
  fontSize: '1.8em',
  fontFamily: 'Comic Sans MS',
  color: 'blue'
})
console.log(title)
// → 'css-1pyvz'
```
这段代码中 CSS class 并没有出现，对样式的引入不需要在代码里写死，而是由库去自动生成，从而不用担心选择器会全局冲突，也就意味着我们不需要手机加BEM这种形式的前缀，选择器的范围就是代码的范围，如果想让这规则在整个应用中适用，可以以JS 模块的方式引入，对于长期维护项目是很强大的，可以确保样式可以向其他资源一样被轻易追踪到，从简单的规范约束到代码约束，提高了样式代码的基础质量。

还有很重要的一点是：生成的样式是CSS，而不是行内样式
早期 CSS-in-JS 类直接将样式绑定在每一个元素上, 元素上的 style 无法代替 css 的所有功能。
但是新的类库基本上会生成全局的样式, 实时进行插入和删除。
例如：
 JSS，可以实现像是 hover, @media 之类的功能, 直接生成相同的 css 代码
 
 styled-components，直接创建样式关联的组件
 
 Glamorous，提供了和 styled-components 相似的 component-first API，但是是用 object 定义样式而非字符串，这种做法更方便以后减少样式体积或是提高性能。
 
#### 2.关键样式
最近相对流行的做法是在文档的顶部引入当前页需要的内联样式来优化页面加载时间. 这与传统加载样式的方式不同, 需要等所有样式都加载完毕才开始进行渲染.
CSS-in-JS 就不一样了：
举个例子, Aphrodite 通过在元素上绑定 class 时调用 CSS 函数跟踪一次渲染所需要哪些样式.
```
import { StyleSheet, css } from 'aphrodite'
const styles = StyleSheet.create({
  title: { ... }
})
const Heading = ({ children }) => (
  <h1 className={css(styles.heading)}>{ children }</h1>
)
```
即使样式是通过 js 定义的, 也可以很方便找到当前页面上的全部样式, 并在 server render 时以<style>形式插入文档顶部
由于渲染 HTML 和 CSS 的时间是一致的， Aphrodite之类的工具通常会同时计算关键样式并渲染出 HTML. 渲染 React 组件的过程也是相似的.
```
const appHtml = `
  <div id="root">
    ${html}
  </div>
`
```
服务端使用 CSS-in-JS, 不但能让客户端没有 JavaScript 的场合下正常工作, 还能渲染的更快.

#### 3.更智能的优化
例如[Styletron](https://github.com/rtsao/styletron) : 核心API 只做一件事情, 将样式分组, 然后生成对应 className。放弃对 class 的直接管理, 仅仅声明我们需要的样式. 由库自己去进行 class 命名的优化，减少代码文件体积
#### 4. 包管理
例如[Polished](https://github.com/styled-components/polished) 提供了一套完善的混合/色彩函数/快捷操作等工具。 有一点像 JavaScript 版的 Sass. 关键差别在于 Polished 生成的代码能更好的组合/测试/分享, 并完全适用于 js 包管理的生态。

#### 5.非浏览器平台样式
 例如[ReactNative](https://facebook.github.io/react-native/)做的事情, 用 JavaScript 语法写原生应用, 最终生成原生组件， 用 Text 和 View 代替 div 和 span。
  在浏览器之外, React Native 也实现了一套 Native 的 flexbox.这个功能最早作为一个名为 css-layout 的 JavaScript 包被发布, 后来又被重构成了 C。
  
鉴于其被广泛使用及重要性， 最终诞生了Facebook开源跨平台前端布局引擎: [Yoga](https://facebook.github.io/yoga/)

Yoga 避免了不必要的级联样式， 专注于 flexbox 的实现。这给跨平台(cross-platform)组件带来了许多可能性。

其中一个惊喜是 airbnb 的 [react-sketchapp](http://airbnb.io/react-sketchapp/)。

直到 react-sketchapp的到来前, 我们还是不得不将前端开发和设计单独进行。

react-sketchapp 让我们根据 Sketch 的文档, 自动生成跨平台的 react 组件。 这颠覆了开发者和设计师过去的合作方式。 现在, 当我们想改变组件的 UI 时， 我们只需要改变设计， 反之同理。

### 缺点
Gajus Kuizinas 的[Stop using CSS in JavaScript for web development](https://medium.com/@gajus/stop-using-css-in-javascript-for-web-development-fa32fb873dcc) 阐述了CSS in JSS 的缺点的文章。
这篇作者是 react-css-modules 的作者，所以观点会有一些偏颇。但整体上还是认可的，社区里流行的 css-in-js 有点过于理想主义，低估了 CSS 本身的能力和生态。这篇文章表面是在讲 CSS in JS，实际上是 CSS Modules 支持者与 styled-components 拥护之间的唇枪舌剑、你来我往。
文中提到第1点，css-in-js 号称解决的命名空间和样式冲突的问题早已不是问题。为了解决这些问题，社区里的解决方案也是出了一茬又一茬，还有人维护了一份完整的
[CSS in JS ] 
但是确实 CSS in JS 有适用的场景，但是也有局限性：
 #### 1.不适用于做样式需要定制的组件
 样式就像小孩的脸，说变就变。比如是最简单的 button，可能在用的时候由于场景不同，需要设置不同的 font-size，height，width，border 等等，如果全部使用 css-in-js 那将需要把每个样式都变成 props，如果这个组件的 dom 还有多层级呢？你是无法把所有样式都添加到 props 中。同时也不能全部设置成变量，那就丧失了单独定制某个组件的能力。
 css-in-js 生成的 className 通常是不稳定的随机串，这就给外部想灵活覆盖样式增加了困难。
 
 #### 2.适用于偏逻辑型，样式比较少，且不太需要外部覆盖样式的场景。
 
 如果是要做一个组件库，让使用方拿着npm就能直接用，样式全部自己搞定，不需要依赖其它组件，如 react-dnd 这种，比较适合。
 
 #### 3.适用于 react-native 这类本身就没有 css 的运行环境。


第4点论扩展性我也觉得 CSS 占优，第5点需要设置条件样式我觉得 CSS 方案也是更好的，而且 CSS 会更易于调试。
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
