---
title: node
date: 2023-05-05 21:30:42
tags:  
---
现在支持es6语法,只需要在配置中设置
```json
{
type:'module'
}
```
## 引入方式
```typescript
// 默认导入,必须指定名字,名字可以随便取
// commonjs的模块引入方式
const express = require('express')
// es6的引入方式
import express from 'express'
// 加{}的导入
import {default as express} from 'express'

// 命名导入
// 方法一 别名导入
import {name as tom} from 'express';
// 方法二
import {name} from 'express'
// 方法三 全部导入 需要一个集合名 如这里的items,使用时候items.属性名来使用
import * as items from 'express'
// 方法四 同时导入默认导出和命名导出
import tom ,{ name} from 'express'
import { tom as default ,name} from 'express'
import tom , * as items from 'express'


```
对于这种不写具体路径的引入都是从`node_modules`当中 
## 导出方式
```typescript
// 默认导出
// 一个模块只能有一个default
// 方法一
export default 'Tom';
// 方法二
const name = 'Tom';
export default name;
// 方法三
const name = 'Tom';
export {name as default}; // 大括号必不可少

// 命名导出
// 方法一
export const name='tom';
// 方法二
const name ='tom'
export {name}//哪怕只有一个值都需要{}
// 使用别名导出
export {name as tom}

```