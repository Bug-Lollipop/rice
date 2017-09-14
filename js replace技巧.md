语法：

`var strings = string.replace(regexp, replacement)`

`regexp` ：您要执行替换操作的正则表达式，如果传入的是一个字符串，那就会当作普通字符来处理，并且只会执行一次替换操作；
如果是正则表达式，并且带有 global (g) 修饰符，则会替换所有出现的目标字符，否则，将只执行一次替换操作。

`replacement` ：您要替换成的字符。

返回值是执行替换操作后的字符串。

### String.replace( ) 的简单用法
```
var text = "javascript 非常强大 ！";
text.replace(/javascript/i, "JavaScript"); // 返回：JavaScript 非常强大 ！
```

##### String.replace( ) 替换所有出现的目标字符

```
var text= "javascript 非常强大 ！JAVASCRIPT 是我最喜欢的一门语言 ！";
text.replace(/javascript/ig, "JavaScript"); // 返回：JavaScript 非常强大 ！JavaScript 是我最喜欢的一门语言 ！
```

##### String.replace( ) 实现调换位置

```var name= "Doe, John"; 
name.replace(/(\w+)\s*,\s*(\w+)/, "$2 $1"); 
// 返回：John Doe
```
##### String.replace( ) 实现将所有双引号包含的字符替换成中括号包含的字符

```
var text = '"JavaScript" 非常强大！';
text.replace(/"([^"]*)”/g, “[$1]“); // 返回：[JavaScript] 非常强大！ 
```

##### String.replace( ) 将所有字符首字母大写

`var text = 'a journey of a thousand miles begins with single step.';`

```
text.replace(/\b\w+\b/g, function(word) {
  return word.substring(0,1).toUpperCase( ) + word.substring(1); 
 }); // 返回：A Journey Of A Thousand Miles Begins With Single Step.
 ```
