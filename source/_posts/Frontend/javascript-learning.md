---
title: JavaScript
date: 2026-07-30
top_img: false
tags:
  - JavaScript
  - 前端开发
categories:
  - 前端开发
    - JavaScript
---

### 1.变量类型分为`基础类型(原始类型)`和`引用类型(对象类型)`,分别是哪些? 区别是什么?
  - 基础类型: `Number`、`String`、`Boolean`、`Null`、`Undefined`、`Symbol`
  - 引用类型: `Object`、`Array`、`Function`、`Date`、`RegExp`
  - 区别: 
    - 基础类型是存放在栈中,引用类型是存放在堆中
    - 值的比较: 基础类型是直接比较值,引用类型是比较引用地址
      ```javascript
      // 基础类型
      const a = 10;
      const b = 10;
      console.log(a === b); // true
      // 引用类型
      const c = { a: 10 };
      const d = { a: 10 };
      console.log(c === d); // false
      ```
    - 赋值(复制)行为: 基础类型是直接赋值,引用类型是赋值引用地址
      ```javascript
      // 基础类型
      const a = 10;
      const b = a;
      console.log(b); // 10
      // 引用类型
      const c = { a: 10 };
      const d = c;
      console.log(d); // { a: 10 }
      ```
      基础类型赋值后,不会改变原值,引用类型赋值后,指向的是同一个对象,改变一个,另一个也会改变

### 2. 深拷贝和浅拷贝的方法?
   基础变量没有“浅拷贝/深拷贝”这一说，因为这两个术语是专门用来描述“复制对象（引用类型）”的, 所以深拷贝和浅拷贝只适用于引用类型变量
   - 浅拷贝: 对象只有一层(所有属性都是基础类型)
     - 方法: `Object.assign({}, obj)` `{...obj}` 
   - 深拷贝: 对象有多个层级(有嵌套对象)
     - 方法: `JSON.stringify.parse(obj)` `structuredClone(obj)` `lodash.cloneDeep(obj)`
     - 注意: `JSON.stringify.parse(obj)` 不支持循环引用,不支持Function类型,不支持Data/RegExp/Map/Set类型
            `structuredClone(obj)`和`lodash.cloneDeep(obj)` 支持循环引用,支持Function类型,支持Data/RegExp/Map/Set类型

### 3. 判断变量类型的方法?
  - `Object.prototype.toString.call(obj)`
     最准确的,最全面的方法,返回一个`"[object 类型名]"`格式的字符串
  - `typeof`
     最简单的判断方法,只能判断基础类型,不支持判断引用类型对象,null,{},[]
     - 注意: `typeof null` 返回 `object`
  - `Array.isArray(obj)`
     判断是否为数组,返回一个布尔值,只能判断数组类型,不支持判断其他引用类型对象
     - 注意: `Array.isArray(null)` 返回 `false`
  - `instanceof`
     判断是否为实例,返回一个布尔值,只能判断引用类型对象,不支持判断基础类型
     - 注意: `instanceof null` 返回 `false`
  - `constructor`
     很少使用,了解即可