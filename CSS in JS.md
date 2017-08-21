CSS in JS 
组件化,将css和html都包含在一起，让component变成一个黑盒 css in js，做到css with component才是最终目标。
组件和css 组合带来的问题:
原始css的样式的重新定制问题，component是一个黑盒； 第一是原始css被嵌入了组件中，如果要重新定制css的话，那么就等于无用的css也包含在内 第二是如何更好的支持的css重新定制，而不失可读性、可维护性
 https://github.com/cssobj/cssobj 可以封装CSS样式，以及动态更新样式

https://github.com/FormidableLabs/radium inline style的方式，写animation比较麻烦
 react style
——

如何看待《React: CSS in JS》

https://github.com/hax/hax.github.com/issues/22

是覆盖样式 还是彻底换肤

 > HTML 是内容，CSS 是表现。它们各搞各的模块化，但就是不会搞成同一个模块化。  
￼


 css-modules 和一个全局css的书写规范 https://github.com/css-modules/css-modules  styled-components
http://www.alloyteam.com/2017/05/guide-styled-components/ 支持伪类，媒体查询 props参数 组件之间的className值不会冲突,
支持css的嵌套语法
不用起className！！！

使用styled components，可将组件分为逻辑组件和展示组件，逻辑组件只关注逻辑相关的部分，展示组件只关注样式。通过解耦成两种组件，可以使代码变得更加清晰可维护。




————————————————————————————————————————
部件必须是组件 ，高拆分。
缺点：
缺乏高亮支持
解决什么问题，优点，缺点，利弊
