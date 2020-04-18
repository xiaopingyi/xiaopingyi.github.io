---
layout: post
title: 树形结构查找问题
categories: 数据结构
description: 子节点查询父节点
keywords: 数据结构、树形结构
---

#### 树形结构转给列表
```javascript
let tree = [{
		"id" : 1,
		"address" : "重庆",
		"pid" : 0,
		"children" : [{
				"id" : 6,
				"address" : "重庆a",
				"pid" : 1
			}, {
				"id" : 7,
				"address" : "重庆b",
				"pid" : 1,
				"children" : [{
						"id" : 13,
						"address" : "重庆bb",
						"pid" : 7
					}, {
						"id" : 13,
						"address" : "重庆bb",
						"pid" : 7
					}
				]
			}, {
				"id" : 6,
				"address" : "重庆a",
				"pid" : 1
			}, {
				"id" : 7,
				"address" : "重庆b",
				"pid" : 1,
				"children" : [{
						"id" : 13,
						"address" : "重庆bb",
						"pid" : 7
					}, {
						"id" : 13,
						"address" : "重庆bb",
						"pid" : 7
					}
				]
			}
		]
	}, {
		"id" : 2,
		"address" : "成都",
		"pid" : 0,
		"children" : [{
				"id" : 8,
				"address" : "成都a",
				"pid" : 2
			}, {
				"id" : 8,
				"address" : "成都a",
				"pid" : 2
			}
		]
	}, {
		"id" : 3,
		"address" : "贵州",
		"pid" : 0,
		"children" : [{
				"id" : 10,
				"address" : "贵州a",
				"pid" : 3
			}, {
				"id" : 10,
				"address" : "贵州a",
				"pid" : 3
			}
		]
	}
];


let treeList = [];
// 树形接口转成列表
let treeToList = (arr) =>{
  arr.map(item=>{
    if(item.children && item.children.length > 0){
      treeToList(item.children);
    }
    treeList.push(item);
  })
}

let treeToList = new Set(...treeToList); // 去除重复数据

```

#### 列表转为树形结构
```javascript
    let dataTree =[
        { id: 1, address: "重庆", pid: 0 },
        { id: 6, address: "重庆a", pid: 1 },
        { id: 12, address: "重庆aa", pid: 6 },
        { id: 14, address: "重庆aaa", pid: 12 },
        { id: 15, address: "重庆aaaa", pid: 14 },
        { id: 7, address: "重庆b", pid: 1 },
        { id: 13, address: "重庆bb", pid: 7 },
        { id: 2, address: "成都", pid: 0 },
        { id: 8, address: "成都a", pid: 2 },
        { id: 9, address: "成都b", pid: 2 },
        { id: 3, address: "贵州", pid: 0 },
        { id: 10, address: "贵州a", pid: 3 },
        { id: 11, address: "贵州b", pid: 3 }
    ];

    // 列表转为树形结构
    let listToTree = (arr) =>{
        let idItem = {};
        arr.map(item=>{
            idItem[item.id] = item;
        })

        let tree = [];
        arr.map(item=>{
            let parentItem = idItem[item.pid];
            if(parentItem){
                (parentItem.children || (parentItem.children = [])).push(item);
            }else{
                tree.push(item);
            }
        })
        return tree;
    }
```

#### 子节点查找所有父节点

1. 先将树形接口转成列表
2. 通过列表查询父节点

```javascript

let treeList = [];
// 树形接口转成列表
let treeToList = (arr) =>{
  arr.map(item=>{
    if(item.children && item.children.length > 0){
      treeToList(item.children);
    }
    treeList.push(item);
  })
}

let treeToList = new Set(...treeToList); // 去除重复数据

let parentIds = [];
// 查找所有的父节点
let findParentIds = (parentId,arr) =>{
    let arrTemp = arr.filter(itm => itm.id === parentId);
    let parentItem = arrTemp && arrTemp[0] || {};
    if(parentItem.children && parentItem.children.length>0){
      parentIds.push(parentItem.id);
      findParentIds(parentItem.parentId,arr);
    }
}
```

#### 父节点查询子节点
```javascript
let findChildrenByParentId = (arr,parentId)=>{
  arr.map(item=>{
    if(item.pid === parentId){
      return item;
    }else{
      item.children && item.children.length > 0 && findChildrenByParentId(item.children,parentId);
    }
  })
}

```
