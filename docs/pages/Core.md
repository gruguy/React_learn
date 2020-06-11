# 核心概念

## JSX简介

```
const element = <h1>Hello， world</h1>;
```
上面这段代码，既不是字符串，也不是HTML。

这段很像js和HTML混合起来的代码,就是JSX，它是一个Javascript的语法扩展。JSX可以很好地描述UI应该呈现出它应有的本质形式。JSX可能会使人联想到模板语言，但它具有Javascript的全部功能。

### 为什么要使用JSX？

React 认为渲染逻辑本质上与其他UI逻辑内在耦合，比如，在UI中需要绑定处理事件、在某些时刻状态发生变化时需要通知到UI，以及需要在UI中展示准备好的数据。

#### 在JSX中嵌入表达式

```
const name = 'Gruguy';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
在JSX语法中，你可以在大括号内放置任何有效的Javascript表达式。如： 2 + 2，user.firstName 或 formatName(user)的结果，并将结果嵌入到 h1 元素中。

```
function formatName(user){
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Gruguy',
  lastName: 'Chang'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
)

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

为了便于阅读，我们会将JSX拆分为多行。同时，我们建议将内容包裹在括号中，虽然不是强制要求的，但是可以避免遇到自动插入分好的陷阱。

#### JSX也是一个表达式

变异之后，JSX表达式会被转为普通的Javascript函数调用，并且对其取值后得到JavaScript对象。

也就是说，可以在if语句和for循环的代码块中使用JSX，将JSX赋值给变量，把JSX当做参数传入，以及从函数中返回JSX：

```
function getGreeting(user){
  if(user){
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger!</h1>
}
```

#### JSX特定属性
可以通过使用引号来将属性值指定为字符串字面量，也可以使用大括号，来将属性值中插入一个JavaScript表达式：
```
const e1 = <div tabIndex = "1">

const e2 = <img src={user.avatarUrl}></img>;
```
> 注意： 嵌入表达式时，不要在大括号外面加上引号。同一属性不能使用两种符号。 JSX中 class 变成了 className， tabindex 变成了tabIndex

## 元素渲染

元素是构成React应用的最小砖块。

与浏览器的DOM元素不同，React元素市创建开销极小的普通对象。React DOM会负责更新DOM来与React元素保持一致。

#### 将一个元素渲染为DOM

假设HTML文件某处有一个<code>div</code>:

```
<div id="root"></div>
```

上面的节点，称之为根 DOM节点，在该节点上的所有内容都将由React DOM 管理。想要将一个React元素渲染到根DOM节点，只需要把它们一起传进<code>ReactDOM.render()</code>:

```
const element = <h1>Hello, world</h1>;
ReactDom.render(element,document.getElementById('root'));
```

#### React 只更新它需要更新的部分

React DOM会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使DOM达到预期的状态。

## 组件 & Props

组件允许你将UI拆分为独立可复用的代码片段，并对每个片段进行独立构思。组件从概念上类似于JavaScript函数。它接收任意的入参，并返回用于描述页面展示内容的React元素。

#### 函数组件和class组件
定义一个JavaScript函数：
```
function Welcome(props){
  return <h1>Hello, {props.name}</h1>;
}
```
该函数是一个有效的React组件，因为它接收位以待有数据的props对象并返回一个React元素。这类组件成为“函数组件”，本质上就是JavaScript函数。

还可以使用ES6的class来定义组件：
```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上面两个组件是等效的。

#### 渲染组件

React元素可以使DOM标签，也可以是用户自定义的组件：

```
const element = <Welcome name="Sara" />;
```

当React元素为用户自定义组件时，它会将JSX所接受的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为“props”。

```
function Welcome(props){
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.remder(
  element,
  document.getElementById('root')
);
```
---
#### 组合组件

组件可以在其输出中引用其他组件。这就可以让我们用同意组建来抽象出任意层次的细节。按钮、表单、对话框，甚至整个屏幕的内容：在React应用程序中，这些通常都会以组件的形式表示。

```
function Welcome(props){
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Gigi" />
      <Welcome name="Adele" />
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

一般来说，每个新的React应用程序的顶层组件都是APP组件。但是，如果将React集成到现有的应用程序中，你可能需要使用像Button 这样的小组件，并自下而上的将这类组件逐步应用到视图层的每一处。

---


#### 提取组件

将组件拆分为更小的组件。

```
function Comment(props){
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar" src={props.author.avatarUrl} alt={props.author.name} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div class="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  )
}
```

上面组件描述了一个社交媒体网站上的评论功能，接收author，text，date作为props。

这种写法由于嵌套关系，变得难以维护，也很难复用，需要提取更小颗粒的功能出来：

首先将Avatar组件提取出来

```
function Avatar(props){
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  )
}
```

Avatar 不许知道它在Comment组建中是如何渲染的。因此我们给他的props其了一个更通用的名字：user，而不是author。

命名组建应该从组件自身角度命名props，而不是依赖上下文命名。

```
function Comment(props){
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author}>
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Commnet-date">
        {formatDate(props.date)}
      </div>
    </div>
  )
}
```

接下来提取UserInfo组件，该组件在用户名旁渲染Avatar 组件：

```
function UserInfo(props){
  return (
    <div className="UserInfo">
      <Avatar user={props.user}>
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  )
}
```

进一步简化Comment 组件：

```
function Comment(props){
  <div className="Comment">
    <UserInfo user={props.author} />
    <div className="Comment-text">
      {props.text}
    </div>
    <div className="Comment-date">
      {formatDate(props.date)}
    </div>
  </div>
}
```

---

#### Props 的只读性

* 所有React组件都必须像纯函数一样保护它们的props不被更改。

---

## State & 生命周期
