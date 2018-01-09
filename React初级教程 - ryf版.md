```
                                 ___                   __
  ________  _____ _____    _____/  |__           _    / /      _____    ___
  \__  _  \/ __  \\__  \ _/ ___\   ___\  __  __ | |  / /__    /___  \  / _ \
    | / \/ | ___/ / ___ \ \ \___|  |_    \ \/ / | | |  \  | _  __/ _/ | |_| |
    |_|    \____//_/   \_\ \___/ \___|    \__/  |_|  \___/ |_|/_____\  \___/

```
## React入门教程
> 参考文章：http://www.ruanyifeng.com/blog/2015/03/react

### HTML模板
React页面源码的大致结构为：
```html
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      //...
    </script>
  </body>
</html>
```
需要注意的是：
1. 最后一个 <script> 标签的 type 属性为 text/babel 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel" 。
2. 上面代码一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，react.js 是 React 的核心库，react-dom.js 是提供与 DOM 相关的功能，Browser.js 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

### 元素渲染
要将元素添加到DOM中，首先在一个 HTML 页面中添加一个 id="root" 的 <div>:
```html
<div id="root"></div>
```
在此 div 中的所有内容都将由 React DOM 来管理，所以我们将其称之为 “根” DOM 节点。

我们用React 开发应用时一般只会定义一个根节点。但如果你是在一个已有的项目当中引入 React 的话，你可能会需要在不同的部分单独定义 React 根节点。

要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上：
```javascript
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
上述代码会将一个h1标签插入到root节点上展示“Hello world”内容。













