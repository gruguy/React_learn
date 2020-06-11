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

在元素渲染的章节中，我们只了解了一种更新UI界面的方法，通过调用ReactDOM.render() 来修改我们想要渲染的元素：

```
function tick(){
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  )
  
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);

```

本章节中，我们将学习如何封装真正可复用的clock组件。它将设置自己的计时器并每秒更新一次：

```
function Clock(props){
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  )
}

function tick(){
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

上面代码封装了Clock组件，但它忽略了一个关键的技术细节，Clock组建须要设置一个计时器，并且需要每秒更新UI。

理想情况下，我们希望只编写一次代码，便可以让Clock组件自我更新：

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

所以，我们需要在Clock组建中添加state来实现这个功能。

State与props 类似，但是state是私有的，并且完全受控于当前组件。

---

#### 将函数组件转换为class 组件
通过下面5步将Clock的函数组件z转换成class组件：
* 1. 创建一个同名的ES6 class，并且集成于 React.Component。
* 2. 添加一个空的render() 方法。
* 3. 将函数体移动到render()方法之中。
* 4. 在render()方法中使用this.props替换props。
* 5. 删除剩余的空函数声明。

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello，world！</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

现在 Clock组件被定义为 class，而不是函数。

每次组件更新时render方法都会被调用，但只要在相同的DOM节点中渲染<code><Clock /> </code>,就近有一个Clock组件的class实例被创建使用。这就使得我们可以使用如state或者生命周期方法等很多其它特性。

---

#### 向class组建中添加局部的state

我们通过三步将props移动到state中：
1. 吧render() 方法中的this.props.date 替换成 this.state.date:

```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello，world！</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2. 添加一个class构造函数，然后在该函数中为this.state附初始值：

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {state: new Date()};
  }
  render() {
    return (
      <div>
        <h1>Hello，world！</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

如上面的代码所示：Class组件应该始终使用 props 参数来调用父类的构造函数。

3. 移除 <code><Clock /></code> 元素中的 date 属性：

```
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
接下来，我们会设置Clock 的计数器并每秒更新它。

---

#### 将生命周期方法添加到Class中

当Clock组建第一次被渲染到DOM中的时候，就为其设置一个定时器。在React中被称为“挂在（mount）”。

同时，当DOM中Clock组件被删除的时候，应该清除定时器。这在React中被称为“卸载（unmount）”。

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    // 这个是挂载的生命周期方法
  }

  componentWillUnmount() {
    // 即将卸载时候生命周期
  }

  render {
    return (
      ...
    )
  }
}
```
上面两个方法叫做“生命周期方法”。

componentDidMount() 方法会在组件已经渲染到DOM后运行，所以最好这个时候设置定时器：
```
componentDidMount() {
  this.timerID = setInterval(
    () => this.tick(),
    1000
  );
}
```

接下来把计时器的ID保存到this之中（this.timerID）。

我们会在 componentWillUnmount() 生命周期方法中清除计时器：
```
componentWillUnmount() {
  clearInterval(this.timerID);
}
```
最后，我们会实现一个叫tick的方法， Clock组件每秒都会调用它，使用 this.setState() 来适可更新组件的state:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  componentDidMount(){
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    })
  }

  render() {
    return (
      <div>
        <h1>Hello world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    )
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

现在时钟每秒都会刷新。我们看下发生了什么以及这些方法的调用顺序：

1. 当 Clock 组件被传给 ReactDom.render()的时候，React会调用Clock组建的构造函数。因为Clock 需要显示当前的时间，所以它会用一个包含当前时间的对象来初始化 this.state。
2. 之后React会调用组建的render() 方法。这就是React确定该页面上展示什么的方式。然后React更新DOM来匹配Clock渲染的输出。

3. 当 Clock 的输出被插入到DOM中之后，React会调用ComponentDidMount() 生命周期方法，此方法中，Clock逐渐向浏览器请求设置一个计时器来每秒调用一次组件的tick() 方法。

4. 浏览器每秒都会调用一次tick()，Clock组件会通过调用setState()来计划一次UI更新。然后会重新调用render()方法来确认页面上盖显示什么。每一次的this.state.date就不一样了，React也会相应更新DOM。

5. 一旦Clock组件从DOM中移除，React就会调用ComponentWillUnmount()生命周期方法，这样即使其就停止了。

---

#### 正确的使用 state

1. 不要直接修改 state

```
// Wrong
this.state.comment = 'hoho!';
```
这样的代码不会重新渲染组件，而是应该使用setState():
```
// Correct
this.setState({comment: 'Hello'});
```
构造函数是唯一可以给this.state赋值的地方。

2. state的更新可能是异步的

出于性能考虑，React可能会把多个setState() 调用合并成一个调用。
因为this.props和 this.state可能回忆不更新，所以不要依赖他们的值来更新下一个状态。
以下代码可能无法更新计数器：
```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
})
```
要解决这个问题，可以让setState() 接收一个函数而不是一个对象。这个函数用上一个state作为第一个参数，将此次更新被应用时的props作为第二个参数：
```
// Correct
this.setState((state, props) => {
  counter: state.counter + props.increment
})
```

上面使用箭头函数，普通函数也是可以的。

3. State的更新会被合并

当你调用setState()的时候，React会吧你提供的对象合并到当前的state。

比如 state包含几个独立的变量：

```
constructor(props){
  super(props);
  this.state = {
    posts: [],
    comments: []
  }
}
```
然后可以分别调用setState()来单独的更新它们：

```
componentDidMount(){
  fetchPosts().then(response => {
    this.setState({
      post: response.posts
    });
  });

  fetchPosts().then(response => {
    this.setState({
      comments: response.comments
    })
  })
}
```
这里的合并是浅合并，所以this.setState({comments})完整保留了this.state.posts, 但是完全替换了this.state.commnets。

---

4. 数据时向下流的

不管是父组件或者子组件都无法知道某个组件是有状态还是无状态的，并且它们也并不关心它是函数组件还是class组件。

这就是为什么称state为局部或者封装的原因。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的state作为props向下传递到它的子组件中：
```
<FormattedDate date={this.state.date} />
```
FormattedDate子组件会在其props中接受参数date，但是组件本身无法知道它是来自于Clock的state，或者是Clock的props，还是手动输入的：
```
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```
state 只能从当前组件传递到影响书中“低于”它们的组件。

## 事件处理

React元素的事件处理和DOM元素的很相似，但是有一点语法上的不同：
* React时间的命名采用小驼峰式(camelCase)，而不是纯小写。
* 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

```
// 传统的HTML
<button onclick="activeLasers()">
 Active Lasers
</button>


// React中
<buttton onClick={activeLasers}>
  Active Lasers
</button>
```

在React中另一个不同点是不能通过返回false的方式组织默认行为。你必须现时的使用preventDefault。

```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

