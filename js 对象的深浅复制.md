### 浅复制

关于js对象的深浅复制，先来一段代码
```
//对象浅复制
function shadowCopy(obj){
    if(typeof obj !== 'object') return ;
    var newObj;
    if(obj.constructor === Array){
        newObj = [];
    } else {
        newObj = {};
        newObj.constructor = obj.constructor;//保留对象的constructor属性
    }
    for(var prop in obj){
        if(obj.hasOwnProperty(prop)){
          newObj[prop] = obj[prop];
        }
    }
    return newObj;
 }
 ```
 
 ### 深复制
 深复制

那么深复制可能就需要层层递归，复制对象的所有属性，包括对象属性的属性的属性，有人想出了用JSON的解析实现，如下代码：

```
function deepCopy(obj){
   if(typeof obj !== "object"){ return ;}
   var str = JSON.stringify(obj);
   return JSON.parse(str);
}
```
https://juejin.im/entry/59ca5a2151882579e9555b41?utm_source=gold_browser_extension
