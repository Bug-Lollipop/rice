#### 三元操作符代替 if... else
```
const x = 20
let big
if (x > 10) { big = true; } 
else { big = false; }

```
```
const big = x > 10 ? true : false;

```

#### 判断是否存在
` if (a === true) `
改为
 ` if (a) `
 
 #### for 循环
 `for (let i = 0; i< user.length; i++)`
 
 改为
 
 `for (let index in user)`
 
 或 使用 `Array.forEach`
 
 #### 简写
 如果参数是null 或者是undefined ,可以将一行代码替代六行代码
 
 ```
let dbHost
if (process.env.DB_HOST) {
  dbHost = process.env.DB_HOST
} else {
  dbHost = 'localhost'
}
```
改为
` const dbHost = process.env.DB_HOST || 'localhost'`

#### 十进制指数简写
1e7 相当于 10000000 （1 的后面有 7 个 0）

` for (let i = 0; i < 100000; i++) {}`
改为

` for (let i = 0; i < 1e7; i++) {}`

#### 对象属性
` const obj = { x: x, y: y }
改为
` const obj = { x, y } `

#### 箭头函数

```
function sayHello(name) {
  console.log('Hello', name);
}

setTimeout(function() {
  console.log('Loaded')
}, 2000);

list.forEach(function(item) {
  console.log(item);
});
```

改为：

```
sayHello = name => console.log('Hello', name);

setTimeout(() => console.log('Loaded'), 2000);

list.forEach(item => console.log(item));
```

#### 隐式返回
return在函数中经常使用到的一个关键词，将返回函数的最终结果。箭头函数用一个语句将隐式的返回结果（函数必须省略{}，为了省略return关键词）。

如果返回一个多行语句（比如对象），有必要在函数体内使用()替代{}。这样可以确保代码是否作为一个单独的语句返回。

```
function calcCircumference(diameter) {
  return Math.PI * diameter
}
```
改成
```
calcCircumference = diameter => (
  Math.PI * diameter;
)
```

#### 默认参数值
可以用`if ` 语句来定义函数参数的默认值。
```
function volume(l, w, h) {
  if (w === undefined)
    w = 3;
  if (h === undefined)
    h = 4;
  return l * w * h;
}
```

简写

```
volume = (l, w = 3, h = 4 ) => (l * w * h);

volume(2) //output: 24

```

#### 模版字符串

```
const welcome = 'You have logged in as ' + first + ' ' + last + '.'

const db = 'http://' + host + ':' + port + '/' + database;
```

改成

```

const welcome = `You have logged in as ${first} ${last}`;

const db = `http://${host}:${port}/${database}`;

```

#### 模块加载
```
const observable = require('mobx/observable');
const action = require('mobx/action');
const runInAction = require('mobx/runInAction');

const store = this.props.store;
const form = this.props.form;
const loading = this.props.loading;
const errors = this.props.errors;
const entity = this.props.entity;
```

改成

```
import { observable, action, runInAction } from 'mobx';

const { store, form, loading, errors, entity } = this.props;
```

重命名：

```
const { store, form, loading, errors, entity:contact } = this.props;
```

#### 扩展语句（...）
```
// joining arrays
const odd = [1, 3, 5];
const nums = [2 ,4 , 6].concat(odd);

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()
```

改成
```
// joining arrays
const odd = [1, 3, 5 ];
const nums = [2 ,4 , 6, ...odd];
console.log(nums); // [ 2, 4, 6, 1, 3, 5 ]

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = [...arr];
```

不像concat()函数，使用Spread Operator你可以将一个数组插入到另一个数组的任何地方。

```
const odd = [1, 3, 5 ];
const nums = [2, ...odd, 4 , 6];
```
还可以当作解构符
```
const { a, b, ...z } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a) // 1
console.log(b) // 2
console.log(z) // { c: 3, d: 4 }
```

#### 强制参数

默认情况下，JavaScript如果不给函数参数传一个值的话，将会是一个undefined。有些语言也将抛出一个警告或错误。
在执行参数赋值时，你可以使用if语句，如果未定义将会抛出一个错误，或者你可以使用强制参数（Mandatory parameter）
```
function foo(bar) {
  if(bar === undefined) {
    throw new Error('Missing parameter!');
  }
  return bar;
}
```
改成
```
mandatory = () => {
  throw new Error('Missing parameter!');
}

foo = (bar = mandatory()) => {
  return bar;
}
```

#### Array.find

```
const pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]

function findDog(name) {
  for(let i = 0; i<pets.length; ++i) {
    if(pets[i].type === 'Dog' && pets[i].name === name) {
      return pets[i];
    }
  }
}
```

改成

```
pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
console.log(pet); // { type: 'Dog', name: 'Tommy' }
``` 

#### Object [key] 
Foo.bar也可以写成Foo[bar]吧。这个符号可以编写可重用代码块

```
function validate(values) {
  if(!values.first)
    return false;
  if(!values.last)
    return false;
  return true;
}

console.log(validate({first:'Bruce',last:'Wayne'})); // true
```

这个函数可以正常工作。然而，需要考虑一个这样的场景：有很多种形式需要应用验证，而且不同领域有不同规则。在运行时很难创建一个通用的验证功能。

```

// object validation rules
const schema = {
  first: {
    required:true
  },
  last: {
    required:true
  }
}

// universal validation function
const validate = (schema, values) => {
  for(field in schema) {
    if(schema[field].required) {
      if(!values[field]) {
        return false;
      }
    }
  }
  return true;
}


console.log(validate(schema, {first:'Bruce'})); // false
console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true
```
现在有个验证函数，可以做各种形式的重用

#### 双位操作符
双位操作符代替 Math.floor()

` Math.floor(4,9) === 4  // true `

改成

` ~~4.9 === 4 // true `
