##  react知识点-文档

### 安装

#### 初识React

**React** 是一个用于构建用户界面的 JavaScript 库

一切都可以用js表示

### 核心概念

#### Hello World

```
ReactDOM.render(<h1>Hello,world!</h1>,document.getElementById('root'))
```

#### JSX简介

```
const element = <h1>Hello,world!</h1>
```

这个有趣的标签语法既不是字符串也不是 HTML。

它被称为 JSX，是一个 JavaScript 的语法扩展

我们建议在 React 中配合使用 JSX，JSX 可以很好地描述 UI 应该呈现出它应有交互的本质形式。JSX 可能会使人联想到模版语言，但它**具有 JavaScript 的全部功能**。

##### 为什么使用JSX?

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，

在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。

##### 在JSX中嵌入表达式

在下面的例子中，我们声明了一个名为 `name` 的变量，然后在 JSX 中使用它，并将它包裹在大括号中：

```react
const name = 'aking';
const element = <h1>Hello,{name}</h1>
ReactDOM.render(element,document.getElementById('root'));
```

在 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。

在下面的示例中，我们将调用 JavaScript 函数 `formatName(user)` 的结果，并将结果嵌入到 `<h1>` 元素中。

```react
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

为了便于阅读，我们会将 JSX 拆分为多行。同时，我们建议将内容包裹在括号中，但是这可以避免遇到[自动插入分号](http://stackoverflow.com/q/2846283)陷阱。

##### JSX 也是一个表达式

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```react
function getGreeting(user){
  if(user){
    return <h1>Hello,{{ formatName(user) }}!</h1>
	}
  return <h1>Hello,Stranger.</h1>
}
```

##### JSX 特定属性

你可以通过使用引号，来将属性值指定为字符串字面量

```
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个JavaScript表达式：

```
const element = <img src={user.avatarUrl}></img>;
```

>**警告：**
>
>因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。
>
>例如，JSX 里的 `class` 变成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)，而 `tabindex` 则变为 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。

##### 使用JSX指定子元素

假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 XML 语法一样：

```
const element = <img src={user.avatarUrl} />;
```

JSX 标签里能够包含很多子元素:

```
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

##### JSX 表示对象

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

以下两种示例代码完全等效：

```react
const element = {
  <h1 className="greeting">
    Hello,world!
  </h1>
}
```

```react
const element = React.createElement(
  'h1'，
  {className:'greeting'},
  'Hello,world'
)
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```react
// 注意：这是简化过的结构
const element = {
  type:'h1',
  props:{
    className:'greeting',
    children:'Hello world'
  }
}
```

这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。

#### 元素渲染

元素是构成React应用的最小砖块

```react
const element = <h1>Hello,world</h1>
```

与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。React DOM 会负责更新 DOM 来与 React 元素保持一致。

> 你可能会将元素与另一个被熟知的概念——“组件”混淆起来。我们会在[下一个章节](https://react.docschina.org/docs/components-and-props.html)介绍组件。组件是由元素构成的。我们强烈建议你不要觉得繁琐而跳过本章节，应当深入阅读这一章节。

##### 将一个元素渲染为DOM

假设你的 HTML 文件某处有一个 `<div>`：

```
<div id="root"></div>
```

我们将其称为“根” DOM 节点，因为该节点内的所有内容都将由 React DOM 管理。

仅使用 React 构建的应用通常只有单一的根 DOM 节点。仅使用 React 构建的应用通常只有单一的根 DOM 节点。如果你在将 React 集成进一个已有应用，那么你可以在应用中包含任意多的独立根 DOM 节点。

想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 [`ReactDOM.render()`](https://react.docschina.org/docs/react-dom.html#render)：

```react
const element = <h1>Hello world</h1>;
ReactDOM.render(element,document.getElementById('root'))
```

##### 更新已渲染的元素

React 元素是不可变对象。一旦被创建，你就无法更改它的子元素或者属性。一个元素就像电影的单帧：它代表了某个特定时刻的 UI。

根据我们已有的知识，更新 UI 唯一的方式是创建一个全新的元素，并将其传入 [`ReactDOM.render()`](https://react.docschina.org/docs/react-dom.html#render)。

考虑一个计时器的例子：

```react
function tick(){
  const element = (
    <div>
      <h1>Hello,world</h1>
      <h2>It is {new Date().toLocaleTimeString()}</h2>
    </div>
  );
  ReactDOM.render(element,document.getElementById('root'));
}
setInterval(tick,1000);

```

这个例子会在 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) 回调函数，每秒都调用 [`ReactDOM.render()`](https://react.docschina.org/docs/react-dom.html#render)。

> **注意：**
>
> 在实践中，大多数 React 应用只会调用一次 [`ReactDOM.render()`](https://react.docschina.org/docs/react-dom.html#render)。在下一个章节，我们将学习如何将这些代码封装到[有状态组件](https://react.docschina.org/docs/state-and-lifecycle.html)中。
>
> 我们建议你不要跳跃着阅读，因为每个话题都是紧密联系的。

##### React 只更新它需要更新的部分

React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态。

尽管每一秒我们都会新建一个描述整个 UI 树的元素，React DOM 只会更新实际改变了的内容，也就是例子中的文本节点。

根据我们的经验，考虑 UI 在任意给定时刻的状态，而不是随时间变化的过程，能够消灭一整类的 bug。

#### 组件&Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

##### 函数组件与class组件

定义组件最简单的方式就是编写JavaScript函数：

```react
function Welcome(props){
  return <h1>Hello,{props.name}</h1>
}
```

该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

你同时还可以使用 [ES6 的 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 来定义组件：

```react
class Welcome extends React.Component{
  render(){
    return <h1>Hello,{this.props.name}</h1>
  }
}
```

上述两个组件在 React 里是等效的。

