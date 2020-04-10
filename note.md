# React学习笔记

[toc]

## 一、核心概念

### 1.1 State

1. State更新有可能会是异步的
因为this.props和this.state可能会是异步更新，所以不要依赖他们的值去更新下一个状态：

    ```js
    // 这是不对的，因为props和state可能是异步更新
    this.setState({
        counter: this.state.counter + this.props.increment,
    });
    ```

    解决这个问题的方法是**让setState函数接受一个函数作为参数，而不是一个对象**

    ```js
    // 函数内返回一个对象
    this.setState((state, props) => ({
        counter: state.counter + props.increment
    }));
    ```

2. 数据是向下流动的
子组件是不知道父组件传过来的属性（props）到底在父组件中是props还是state还是手工输入的。这通常称为**单向数据流**。任何state只属于特定组件，从state派生出去的数据只会影响到当前组件及其子组件。

### 1.2 事件处理

React元素事件处理和DOM元素很相似，只是语法上有两处不同：

- React事件属性采用小驼峰写法
- 事件属性中接受的是函数，而不是函数执行，并且包裹在{}中

```js
// DOM
<button onclick="activateLasers()">
    Activate Lasers
</button>
```

```js
// React，这里特别要注意传入的是函数，而不是函数执行！！！
<button onClick={activateLasers}>
    Activate Lasers
</button>
```

1. 事件阻止
在DOM和React中事件阻止分别如下：

    ```html
    <!-- 通过在事件函数中最后return false实现事件阻止 -->
    <a href="https://www.baidu.com" onclick="console.log('The link was clicked'); return false">
        Click me
    </a>
    ```

    ```js
    // e是一个合成事件，通过e.preventDefault()来阻止事件
    function ActiveLink() {
        function handleClick(e) {
            e.preventDefault();
            console.log('The link was clicked.');
        }

        return (
            <a href="https://www.baidu.com" onClick={handleClick}>
                Click me
            </a>
        );
    }
    ```

2. 事件函数this指向问题

    - 使用function定义的函数

    ```js
    constructor() {
        ...
        // 常规定义的函数必须加上this指向的绑定
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        ...
    }

    render() {
        return(
            // 构造器中绑定了this之后，
            //这里才可以调用this.handleClick找到“类”中的handleClick方法
            <button onClick={this.handleClick}>
                ...
            </button>
        );
    }
    ```

    - 使用“类箭头”(class fields语法)函数的写法

    ```js
    handleClick = () => {
        ...
    }
    ```

    - 不绑定、不写“类箭头”函数<font color='red'>（不推荐）</font>

    ```js
    handleClick(){
        ...
    }

    render() {
        return (
            // 在调用时使用箭头函数调用，
            // 使得箭头函数中的this与上下文（render函数）所在的词法环境相同
            // 如果回调函数（handleClick）传到子组件，会造成额外的重新渲染
            <button onClick={() => this.handleClick()}>
                ...
            </button>
        );
    }
    ```

3. 向事件处理函数传递参数

有两种方式（不管函数是如何形式定义的）：

```html
<!-- 如果在回调函数中用的到事件对象，对于箭头函数来说，必须显式传递e
而对于bind方式传值，事件对象e隐式传到回调函数中 -->
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

### 1.3 条件渲染

### 1.4 列表 & Key

### 1.5 表单

## 二、高级指引

## 三、API相关

## 四、Hook
