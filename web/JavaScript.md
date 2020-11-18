### 原型链

#### 怎么理解原型和原型链

JS是基于原型的编程，属于OOP一种，JS通过原型实现实例对象创建，通过原型链实现继承

#### 原型链继承

```javascript
// 寄生组合
function A() { }
function B() {
	A.call(this)
}
B.prototype = Object.create(A.prototype)
B.prototype.constructor = B
```



### 闭包

#### 怎么理解闭包

解释1：函数对象通过作用域链相互关联起来，函数内部变量都可以保持在函数的作用域中，有权访问另一个函数作用域中的变量

解释2：一个绑定执行环境的函数



### 函数式编程



### ES6

#### Promise

[Promise自定义](./Promise.md)



### TypeScript



### JS

#### 修改this指向

bind(), call(), apply()

#### eventloop

