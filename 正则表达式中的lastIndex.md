看到了一片文章，想起了gaylen 之前遇到的一个问题,学习了一下原因

```
var reg1 = /a/
var reg2 = /a/g

console.log(reg1.test('abcabc')) // true
console.log(reg1.test('abcabc')) // true
console.log(reg1.test('abcabc')) // true
console.log(reg1.test('abcabc')) // true

console.log(reg2.test('abcabc')); // true
console.log(reg2.test('abcabc')); // true
console.log(reg2.test('abcabc')); // false
console.log(reg2.test('abcabc')); // true

上面的正则是 查看字符串abcabc 是否有字符a,但是结果中却又一个特殊的 false 存在，Why?
```

### lastIndex（针对于带参数g的正则表达式）

在每个实例化的RegExp对象中，都存在一个lastIndex属性，其初始值为0

```
/a/.lastIndex // 0
new RegExp('a').lastIndex // 0
```

 `lastIndex`表示匹配成功时候，匹配内容最后一个字符所在原字符串中的位置 + 1，
也就是匹配内容的下一个字符的index（如果匹配内容在字符串的结尾，同样返回原字符串中的位置 + 1，也就是字符串的length）。
如果未带参数g，`lastIndex`始终为 0。

```
var reg = /ab/g;
reg.test('123abc');
console.log(reg.lastIndex) // 5

// 匹配内容在最后
var reg = /ab/g;
reg.test('123ab');
console.log(reg.lastIndex) // 5

// 不带参数g
var reg = /ab/;
reg.test('123abc');
console.log(reg.lastIndex) // 0
```

而这个`lastIndex`也就是用该正则进行其他匹配操作的时候匹配 *开始* 的位置。而匹配失败时重置`lastIndex`为 0。

```
var reg = /ab/g;

// 初始值为0，从最开始匹配  匹配成功， lastIndex为4
console.log(reg.test('12ab34ab'), reg.lastIndex); // true 4

// 从第4位字符"3"开始匹配 匹配内容为第二个ab  lastIndex 为 8
console.log(reg.test('12ab34ab'), reg.lastIndex); // true 8

// 从第8位 (字符长度为8，没有第8位) 开始匹配 匹配不成功 重置lastIndex 为 0
console.log(reg.test('12ab34ab'), reg.lastIndex); // false 0

// 从头匹配 同第一步
console.log(reg.test('12ab34ab'), reg.lastIndex); // true 4

```
### 问题解决
把全局 `g` 去掉
