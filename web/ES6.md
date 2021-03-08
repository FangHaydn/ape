# ES6

[ES6入门](https://es6.ruanyifeng.com/)

## let  const



## 解构赋值

```js
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

```



## Array

```javascript
Array.property.find()
Array.property.findIndex()
Array.property.includes()
Array.property.fill()
Array.property.keys()
Array.property.values()
Array.from()
Array.of()
//扩展运算
```

## 字符串

### 模板字符串

### 字符串遍历器接口



## Set  Map

Set为非重复的类数组

```javascript
var st = new Set([1,2,2]);
[...new Set('abccsda')].join('');

st.size //
st.has()
st.add()
st.delete()
st.clear()

st.foreach(cb)
st.keys  st.values  st.entries
```

传统Object只能是字符串作为key， Map提供了任意数据类型的key可能

```javascript
var mp = new Map()
mp.set({}, 1)
mp.get({})
mp.has()
mp.delete()
mp.clear()

mp.foreach()
mp.keys  mp.values  mp.entries
```



## Proxy Reflect

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。



## [Promise](./promise.md)



## Class



## Module



## Decorator