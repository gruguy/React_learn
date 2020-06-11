# 安装

开始

想系统地学习一下react框架，所以边看边写下学习心得，以便后期回顾。

## 在网页中添加React

根据需要选择性的使用React

在网页中像使用一般库一样引入React相关的js文件

```javascript
<!-- 加载React -->
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin> </script>
<script src="https://unpkg.com/react@16/umd/react-dom.development.js" crossorigin> </script>

<!-- 加在我们的React组件 -->
<script src="like_button.js">
```

前两个标签加载React，最后一个加载你的组件代码

> 注意：以上代码定义了一个名为LikeButton的React组件

```javascript
const domContainer = document.querySelector("#like_button_container");
ReactDom.render(e(LikeButton), domContainer);
```
这样，我们就把第一个React组件添加到了网页里，这是直接在网页里面的用法

```
const e = React.createElement;

// 显示一个like <button>
return e(
  'button',
  {onClick: () => {
    this.setState({ liked: reue })
  }}
)
```

但是，React还提供了一种用JSX来代替实现的选择：

```
return (
  <button onClick={() => {this.setState({ like: true })}}>
)
```

上面两段代码是等效的，JSX是完全可选的，很多人觉得这样写UI代码更方便

我们现在构建项目大部分是基于node.js，让我们去创建一个React的单页面应用，官方例子是 Create React App.

```
npx create-react-app my-app
cd my-app
npm start
```

> npx 是npm5.2+ 附带的package运行工具。

运行以上代码，就可以成功的跑起来具有结构的React单页面应用。
