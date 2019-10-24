---
title: 对象转指定的数组对象（entires-map-reduce）
tags:
 - es6
 - 算法
---

### 1. 需要实现的数据转换
```json
{ a: 1, b: 2, c: 3 }
=======>
[ {text: 1, value: "a"}, {text: 2, value: "b"}, {text: 3, value: "c"} ]
```

### 1.1 实现

```js
const obj = { a: 1, b: 2, c: 3 };
console.log(Object.entries(obj)); // [ ["a", 1], ["b", 2], ["c", 3] ]
const result = Object.entries(obj).map( item => {
    return { text: item[1], vaule: item[0] }
})
console.log(result) // [ {text: 1, value: "a"}, {text: 2, value: "b"}, {text: 3, value: "c"} ]

//Object.entries() 把对象转为集合
```

### 2. 需要实现的数据转换
```json
[ {text: 1, value: "a"}, {text: 2, value: "b"}, {text: 3, value: "c"} ]
=======>
{ a: 1, b: 2, c: 3 }
```
<!-- more -->
### 2.1 实现
```js
const arr = [ {text: 1, value: "a"}, {text: 2, value: "b"}, {text: 3, value: "c"} ]

const obj1 = arr.reduce((preValue, curValue) => {
    preValue[curValue.value] = curValue.text
    return preValue
},{})

console.log(obj1) // { a: 1, b: 2, c: 3 }
```
