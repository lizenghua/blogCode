---
title: reduce 使用
tags:
 - es6
---

###### 1. 语法

   ```js
   arr.reduce(callback,[initialValue]);
   // reduce 为数组中的每个元素依次执行回调函数
   // callback: 有四个参数  (previousValue, currentValue, index, array)
   // 1.1 previousValue: 上一次调用回调返回的值，或者是初始值
   // 1.2 currentValue: 数组中当前被处理的元素
   // 1.3 index: 当前元素在数组中的索引
   // 1.4 array: 调用 reduce 的数组,这里是arr
   // initialValue: 作为第一次调用 callback 的第一个参数（初始值）
   ```

###### 2. 关于 **initialValue** 参数

   ​	2.1 例子1：不设置 initialValue 参数情况

```js
var arr = [1,2,3,4];
var sum = arr.reduce(function(prev, cur, index, arr){
	console.log(prev, cur, index);
    return prev + cur;
})
console.log(arr, sum);
```

> 打印结果
>
> 1 2 1
>
> 3 3 2
>
> 6 4 3
>
> [1,2,3,4] 10

以上例子中没有设置 初始值，index 是从 1 开始的， 第一次的 prev 值是数组的第一个值，数组长度是4，但是 reduce 函数只循环了 3 次。

<!-- more -->

​	2.2 例子2 设置了 initialValue 参数

```js
var arr = [1,2,3,4];
var sum = arr.reduce(function(prev, cur, index, arr){
    console.log(prev, cur, index);
    return prev + cur;
},0); //注意这里设置了初始值
console.log(arr, sum);
```

> 打印结果
>
> 0 1 0
>
> 1 2 1
>
> 3 3 2
>
> 6 4 3
>
> [1,2,3,4] 10

以上例子中设置了 初始值，index 是从 0 开始， 第一次的 prev 值是设置的初始值 0，数组长度4， reduce函数循环了 4 次

##### * 结论：如果没有提供 initialValue，reduce 会从索引 1 的地方开始执行 callback 方法， 跳过第一个索引。如果提供 initialValue，从索引 0 开始

##### 3. 如果数组为空

   3.1 没有给定初始值

   ```js
   var  arr = [];
   var sum = arr.reduce(function(prev, cur, index, arr) {
       console.log(prev, cur, index);
       return prev + cur;
   })
   //报错，"TypeError: Reduce of empty array with no initial value"
   ```

   3.2 给定初始值

   ```js
   var  arr = [];
   var sum = arr.reduce(function(prev, cur, index, arr) {
       console.log(prev, cur, index);
       return prev + cur;
   }，0)
   console.log(arr, sum); // [] 0
   ```

   #####  * 一般来说我们提供初始值通常更安全

#### 4. 简单用法

   ​	4.1 数组求和，求乘积

   ```js
   let arr = [1, 2, 3, 4];
   let sum = arr.reduce((x,y)=>x+y);
   let mu1 = arr.reduce((x,y)=>x*y);
   console.log(sum); // 求和 10
   cosole.log(mu1); // 求乘积 24
   ```

   
#### 5. 高级用法

   5.1 计算数组中每个元素出现的次数

   ```js
   let names = ['zhangsan', 'lisi', 'wangwu', 'zhaoliu', 'zhangsan'];
   let nameNum = names.reduce((prev,cur) => {
       if(cur in prev){
           prev[cur] ++;
       }else{
           prev[cur] = 1;
       }
       return prev;
   },{});
   console.log(nameNum); // {zhangsan: 2, lisi: 1, wangwu: 1, zhaoliu: 1}
   ```

   5.2 数组去重

   ```js
   let arr = [1, 2, 3, 4, 4, 1];
   let newArr = arr.reduce((prev,cur)=>{
       if(!prev.includes(cur)){
           return prev.concat(cur);
       }else{
           return prev;
       }
   },[]);
   console.log(newArr); // [1,2,3,4]
   ```

   5.3 将二维数组转化为一维

   ```js
   let arr = [[0,1], [2,3], [4,5]];
   let newArr = arr.reduce((prev,cur)=>{
       return prev.concat(cur);
   },[]);
   console.log(newArr); // [0,1,2,3,4,5]
   ```

   5.4 将多维数组转化为一维

   ```js
   let arr = [[0,1], [2,3], [4, [5,6,7]]];
   /*const newArr = function(arr){
      return arr.reduce((pre,cur)=>pre.concat(Array.isArray(cur)?newArr(cur):cur),[])
   }*/
   const newArr = (arr) => arr.reduce((prev,cur) => prev.concat(Array.isArray(cur) ? newArr(cur) : cur),[]);
   console.log(newArr(arr)); //[0,1,2,3,4,5,6,7]
   ```

   5.5 对象里的属性求和

   ```js
   let result = [
       {
           subject: 'math',
           score: 10
       },
       {
           subject: 'chinese',
           score: 20
       },
       {
           subject: 'english',
           score: 30
       }
   ];
   
   let sum = result.reduce((prev,cur)=>{
       return cur.score + prev;
   },0);
   console.log(sum); // 60
   ```

   

- 参考

  https://www.jianshu.com/p/e375ba1cfc47