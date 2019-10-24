---
title: 将扁平化的数据转化为多维数组
tags:
 - es6
 - 算法
---

### 一、扁平数据

```js
const data = {
    parent:[
        {
            name: "文件1",
            pid: 1,
            id: 10001
        },
        {
            name: "文件2",
            pid: 1,
            id: 10002
        },
        {
            name: "文件2-1",
            pid: 2,
            id: 10003
        },
        {
            name: "文件2-2",
            pid: 2,
            id: 10004
        },
        {
            name: "文件1-1-1",
            pid: 4,
            id: 10005
        },
        {
            name: "文件2-1-1",
            pid: 5,
            id: 10006
        }
    ],
    child:[
        {
            name: "文件夹1",
            pid: 0,
            id: 1
        },
        {
            name: "文件夹2",
            pid: 0,
            id: 2
        },
        {
            name: "文件夹3",
            pid: 0,
            id: 3
        },
        {
            name: "文件夹1-1",
            pid: 1,
            id: 4
        },
        {
            name: "文件夹2-1",
            pid: 2,
            id: 5
        }
    ]
}
```

<!-- more -->

### 二、转为多维数组

```js
//1. 将所有的数据混合在一起
let allData = [...data.parent, ...data.child];
//2. 生成一个映射表（将id抽取出来，以对象的键值对形式展示）
//{1:{name: "文件夹1", pid: 0, id: 1}}
let treeMapList = allData.reduce((obj,cur)=>{
	obj[cur["id"]] = cur;
	return obj;
},{});
//3. 生成一个新的数组，根据pid在映射表中寻找并判断，相应的生成多层数组
let result = allData.reduce((arr,cur)=>{
    let pid = cur.pid;
    let parent = treeMapList[pid];
    if(parent){
        parent.children ? parent.children.push(cur) : parent.children = [cur];
    }else if(pid === 0){ //这是根文件夹
        arr.push(cur);
    }
    return arr;
},[]);
console.log(result);
```
![](https://user-gold-cdn.xitu.io/2019/10/9/16daf99e3a85194d?w=492&h=120&f=png&s=9905)

![](https://user-gold-cdn.xitu.io/2019/10/9/16daf9ae43e561b2?w=510&h=654&f=png&s=46801)