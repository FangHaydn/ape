# React中函数this绑定问题

React的class组件中绑定this有多种方式：

### bind

需要在构造函数里面绑定所有函数，书写麻烦，代码混乱可读性差

```javascript
class App extends React.PureComponent {
    constructor(props) {
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick(e){

    }

    render() {
        return <div onClick={this.handleClick}></div>
    }
}
```

### class公有类字段（public fields）

该方式使用了还在提案阶段的特性，好在可以使用babel做转换实现，书写比较方便美观

某些情况存在多占用内存情况，每个类实例都会持有一个该函数

[constructor中bind与class公有类字段定义箭头函数对比]('https://medium.com/dailyjs/demystifying-memory-usage-using-es6-react-classes-d9d904bc4557')

> 引用翻译：使用bind方法时，在大量重复使用组件时可以有一点点的内存优化，如果是个别使用这样做并不值得

```javascript
class App extends React.PureComponent {

    handleClick = (e) => {

    }
    render() {
        return <div onClick={this.handleClick}></div>
    }
}
```

### 使用时绑定

这种方式在每次渲染时都会重复绑定，没法利用react的数据对比渲染优化

```javascript
class App extends React.PureComponent {

    handleClick(e) {

    }
    render() {
        return <div onClick={this.handleClick.bind(this)}></div>
    }
}
```

### 使用时包箭头函数

这种方式在每次渲染时都会重复生成箭头函数，没法利用react的数据对比渲染优化

```javascript
class App extends React.PureComponent {

    handleClick(e) {

    }
    render() {
        return <div onClick={(e) => this.handleClick(e)}></div>
    }
}
```