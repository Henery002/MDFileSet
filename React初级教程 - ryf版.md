```
                                 ___                   __
  ________  _____ _____    _____/  |__           _    / /      _____    ___
  \__  _  \/ __  \\__  \ _/ ___\   ___\  __  __ | |  / /__    /___  \  / _ \
    | / \/ | ___/ / ___ \ \ \___|  |_    \ \/ / | | |  \  | _  __/ _/ | |_| |
    |_|    \____//_/   \_\ \___/ \___|    \__/  |_|  \___/ |_|/_____\  \___/

```
# React入门笔记
> 作    者：Henery <br/>
> 邮    箱：henery_002@163.com <br/>
> 参考文章：http://www.ruanyifeng.com/blog/2015/03/react 、http://www.runoob.com/react/react-tutorial.html

## 目录
1. [HTML模板](#1-html模板)
2. [JSX语法](#2-jsx语法)
3. [元素渲染](#3-元素渲染)
4. [组件 & Props](#4-组件--props)
5. [State & 生命周期](#5-state--生命周期)
6. [Refs & DOM](#6-refs--dom)
7. [PropTypes类型检查](#7-proptypes类型检查)
8. [列表 & Keys](#8-列表--keys)
9. [事件处理](#9-事件处理)
10. [表单](#10-表单)
11. 其他
<br/><br/>

## 正文
### 1. HTML模板
React页面源码的大致结构为：
```html
'!DOCTYPE html>
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
1. 最后一个 script 标签的 type 属性为 text/babel 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel" 。
2. 上面代码一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，react.js 是 React 的核心库，react-dom.js 是提供与 DOM 相关的功能，Browser.js 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

### 2. JSX语法
JSX是JavaScript的一种语法扩展，它可以将html语言直接写在js语言中，例如JSX使用这种语法声明变量：
```javascript
const elememt = <h1>Hello world!</h1>;
```
还可以在JSX中使用js表达式，表达式要包含在大括号中：
```javascript
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

经过编译之后，JSX会被转化为普通的js对象。也就是说可以在 if 或者 for 语句里使用 JSX，将它赋值给变量，当作参数传入，作为返回值：
```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```
#### 2.1 JSX属性
可以使用引号来定义以字符串为值的属性，也可以使用大括号来定义以 JavaScript 表达式为值的属性：
```javascript
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```
需要注意的是，使用了大括号包裹的 JavaScript 表达式时不能再到外面套引号。JSX 会将引号当中的内容识别为字符串而不是表达式。
#### 2.2 JSX嵌套
JSX 标签同样可以相互嵌套：
```javascript
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```
> 因为 JSX 的特性更接近 JavaScript 而不是 HTML , 所以 React DOM 使用 camelCase 小驼峰命名 来定义属性的名称，而不是使用 HTML 的属性名称。
> 例如，class 变成了 className，而 tabindex 则对应着 tabIndex.


### 3. 元素渲染
要将元素添加到DOM中，首先在一个 HTML 页面中添加一个 id="root" 的 div:
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

### 4. 组件 & props
React中的组件有两种定义方式：函数定义、ES6 class。
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该种方式直接使用js函数定义组件。该函数是一个有效的React组件，它接收一个单一的“props”对象并返回了一个React元素。
项目中常使用ES6 class定义组件：
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
#### 4.1 组件的渲染
React元素除了使用常用的DOM标签，还可以是用户自定义的组件
```javascript
const element = <Welcome name="Sara" />;
```
当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件,这个对象称之为“props”。
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
> 注意：组件名称必须以大写字母开头。例如：<div /> 表示一个DOM标签，但 <Welcome /> 表示一个组件，并且在使用该组件时你必须定义或引入它。

#### 4.2 Props属性
组件可以在它的输出中引用其它组件，这就可以用同一组件来抽象出任意层次的细节。在React应用中，按钮、表单、对话框等通常都被表示为组件。
可以往组件中加入任何属性，比如 <Welcome name="Sara"> ，就是 Welcome 组件加入一个 name 属性，值为 Sara。
组件的属性可以在组件类的 this.props 对象上获取，比如 name 属性就可以通过 this.props.name 读取。
例如，创建一个App组件，用来多次渲染Welcome组件。<br/>
```javascript
class Welcome extends React.Component{
  render(){
    return <h1>Hello, {this.props.name}</h1>;
  }
}

class App extends React.Component{
  render(){
    return (
      <div>
        <Welcome name="Sara" />
        <Welcome name="Cahal" />
        <Welcome name="Edite" />
      </div>
    );
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
> 注意：组件的返回值只能有一个根元素。因此需要用一个div包裹所有Welcome元素。

#### 4.3 this.props.children
this.props对象的属性与组件的属性一一对应，但 this.props.children 属性除外。它表示组件的所有子节点。
参看下面的例子：
```javascript
class NotesList extends React.Component {
  render() {
    return (
      <ol>
        {React.Children.map(this.props.children, (child) => <li>{child}</li>)}
      </ol>
    );
  }
}

ReactDOM.render(
  <NotesList>
    <span>hello</span>
    <span>world</span>
  </NotesList>,
  document.body
);
```
该例中，NotesList 组件有两个 span 子节点，它们都可以通过 this.props.children 被读取到。最后的输出结果(html结构)是：
```html
<ol>
  <li>
    <span>hello</span>
  </li>
  <li>
    <span>world</span>
  </li>
</ol>
```
> 注意：this.props.children 的值有三种取值类型：
> - 如果当前组件没有子节点，其值为 undefined
> - 如果有一个子节点，其值为 object 类型
> - 如果有多个子节点，其值为 array 类型
>
> 因此在处理 this.props.children 时需要注意。

React 提供了一个工具方法 [**React.Children**](https://doc.react-china.org/docs/react-api.html#reactchildren) 来处理 this.props.children 。
所以可以使用 React.Children.map 来遍历子节点，而不用担心 this.props.children 的数据类型是 undefined 还是 object。

### 5. State & 生命周期
#### 5.1 State
React把组件看成一个状态机（State Machines）。通过与用户交互，实现不同状态的转换，然后渲染UI，让用户界面和数据保持一致。<br/>
查看下面的例子：
```javascript
class LikeButton extends React.Component{
  constructor(props){
    super(props);
    this.state = {liked:false};
  }
  handleClick = (event) => {
    this.setState({
      liked: !this.state.liked
    });
  }
  render() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>You {text} this. Click to toggle.</p>
    );
  }
};

ReactDOM.render(
  <LikeButton />,
  document.getElementById('example')
);
```
上面定义了一个 LikeButton 组件，它的 getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。当用户点击组件，导致状态变化，this.setState 方法就修改状态值，每次修改以后自动调用 this.render 方法，再次渲染组件。

> 由于 this.props 和 this.state 都用于描述组件的特性，可能会产生混淆。一个简单的区分方法是，this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。

#### 5.2 生命周期
组件的生命周期可分为三个状态：
- Mounting：已插入真实DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实DOM

三种状态共计五中处理函数：
- componentWillMount： 在组件将要渲染前调用，在客户端也在服务端。
- componentDidMount： 在第一次渲染后调用，只在客户端。
之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。如果想和其他js框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。
- componentWillReceiveProps： 在组件接收到一个新的prop时被调用。这个方法在初始化render时不会被调用。
- componentDidUpdate： 在组件完成更新后立即调用。在初始化时不会被调用。
- componentWillUnmount： 在组件从 DOM 中移除的时候立刻被调用。

此外，React还提供了两种特殊状态的处理函数：
- shouldComponentUpdate： 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。 
- componentWillUpdate： 在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。

参考以下示例：
```javascript
class Hello extends React.Component{
  constructor(){
    super();
    this.state = {opacity:1.0}
  }

  componentDidMount = () => {
    this.timer = setInterval(function () {
      var opacity = this.state.opacity;
      opacity -= .05;
      if (opacity < 0.1) {
        opacity = 1.0;
      }
      this.setState({
        opacity: opacity
      });
    }.bind(this), 100);
  },

  render () {
    return (
      <div style={{opacity: this.state.opacity}}>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <Hello name="world"/>,
  document.body
);
```
上面代码在hello组件加载以后，通过 componentDidMount 方法设置一个定时器，每隔100毫秒就重新设置组件的透明度，从而引发重新渲染。
> 注意：该例中，组件的style属性的设置方式不能写成：style="opacity:{this.state.opacity};"，应该写成：style={{opacity:this.state.opacity}}
> 因为 React 组件样式是一个对象，所以第一层大括号表示这是 js 语法，第二层大括号表示样式对象。

### 6. Refs & DOM
在React中，组件并不是真实的DOM节点，而是存在于内存中的一种数据结构，叫做虚拟DOM（virtual DOM）。只有当它插入文档之后才会成为真实DOM。
根据React的设计，所有的DOM变动，都先在虚拟DOM上发生，然后再将实际发生变动的部分反应在真实DOM上，这种算法叫 [DOM diff](https://calendar.perfplanet.com/2013/diff/)。它能够极大提高网页的性能。

但是有时需要从组件获取到DOM节点，就需要用到 ref 属性。

使用时，先将一个ref属性绑定到render返回值上：
```html
<input ref="myInput" />
```
在其他部分代码中，通过 this.refs 获取属性
```javascript
var input = this.refs.myInput;      //input为DOM对象
var inputValue = input.value;
```
查看完整实例：
```javascript
class MyComponent extends React.Componet{
  handleClick = () => {
    // 使用原生的 DOM API 获取焦点
    this.refs.myInput.focus();
  }
  render () {
    //  当组件插入到 DOM 后，ref 属性添加一个组件的引用到 this.refs
    return (
      <div>
        <input type="text" ref="myInput" />
        <input
          type="button"
          value="点击"
          onClick={this.handleClick}
        />
      </div>
    );
  }
}
 
ReactDOM.render(
  <MyComponent />,
  document.getElementById('example')
)
```
需要注意的是，由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错。

上面代码中，通过为组件指定 Click 事件的回调函数，确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName] 属性。

此外，也可以使用 [**getDOMNode()**](https://segmentfault.com/q/1010000006198939) 方法获取DOM元素。

更多关于 Refs 的内容，参见[官方文档](https://doc.react-china.org/docs/refs-and-the-dom.html)。

> 注意：ref 属性无法应用在函数式组件上，因为函数式组件没有实例，应该将其转化为 class 组件。

### 7. PropTypes类型检查
> 注意：React.PropTypes 自 React v15.5 起已弃用。请使用 prop-types 库代替。

#### 7.1 PropTypes
我们知道，组件的属性可以接受任意类型的值，如字符串、对象、函数。
这就需要一种机制来验证当使用组件时提供的参数是否符合要求。组件的PropTypes属性就是用作验证组件实例的属性是否符合要求的。

要检查组件的属性，就需要配置特殊的propTypes属性：
```javascript
import PropTypes from 'prop-types';
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

//类型检查
Greeting.propTypes = {
  name: PropTypes.string
};
```
该例中使用了PropTypes.string，它规定name属性必须是字符串类型值，当给属性传递了错误类型的值后js控制台会打印警告信息：
```javascript
Warning: Failed propType: Invalid prop `name` of type `string` supplied to `Greeting`, expected `string`.
```
其他使用类型检查验证器的例子：
```javascript
import PropTypes from 'prop-types';
MyComponent.propTypes = {
  // 可以将属性声明为以下 JS 原生类型
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、子元素或数组）。
  optionalNode: PropTypes.node,

  // 一个 React 元素
  optionalElement: PropTypes.element,

  // 也可以声明属性为某个类的实例，这里使用 JS 的 instanceof 操作符实现。
  optionalMessage: PropTypes.instanceOf(Message),

  // 也可以限制属性值是某个特定值之一
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 限制它为列举类型之一的对象
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个指定元素类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 一个指定类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个指定属性及其类型的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 也可以在任何 PropTypes 属性后面加上 `isRequired` 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,
};
```

#### 7.2 属性默认值
还可以使用 defaultProps 为组件props定义默认值：
```javascript
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 为属性指定默认值:
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染 "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```
这样一来，如果name属性未被赋值的话，它将会有默认值。
> 注意：defaultProps 用来确保 this.props.name 在父组件没有特别指定的情况下，有一个初始值。类型检查发生在 defaultProps 赋值之后，所以类型检查也会应用在 defaultProps 上面。

### 8. 列表 & Keys
#### 8.1 列表组件
在React中，使用map()方法将遍历数组，将数组转化为元素。来看下面的demo。
使用js中的 map() 方法遍历numbers数组，对数组中的每个元素返回li标签，得到一个数组 listItems ,最后将数组插入到ul元素中渲染近DOM中，就生成了一个列表：
```javascript
const numbers = [2,4,6,8,10];
const listItems = numbers.map((number) => 
  <li>{number}</li>
)

ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
)
```
通常情况下，会将这种渲染列表的需求封装成一个组件，这个组件会接收numbers数组作为参数，输出一个列表：
```javascript
class NumberList extends React.Component {
  render(){
    const listItems = this.props.numbers.map((number) => <li key={number.toString()}>{number}<li>);
    return
      <ul>{listItems}</ul>
  }
}

const numbers = [2,4,6,8,10];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.body
)
```
该例中，若最初没有给li元素添加key值，运行代码时会得到警告信息：
> a key should be provided for list items.

所以在创建元素时必须为其赋key属性。

#### 8.2 [Keys](https://segmentfault.com/a/1190000009149186)
React中的key属性是一个特殊的属性，它的出现不是给开发者用的（例如为一个组件设置key之后不能获取到组件的这个key值），而是给react自己用的。

简单来说，react利用key来识别组件，是一种身份标识，每个key对应一个组件，相同的key，react会认为是同一个组件，这样后续相同的key对应的组件都不会被创建，例如：
```javascript
this.state = {
  users:[
    {id:1, name:'Github'},
    {id:2, name:'CSDN'},
    {id:3, name:'StackOverflow'}
  ]
}

render(){
  return(
    <div>
      <h2>平台列表</h2>
      <ul>
        {this.state.users.map((val,idx) => 
          <li key={val.id}>{val.id} : {val.name}</li>
        )}
      </ul>
    </div>
  )
}
```
在上面的例子中，DOM渲染挂载结束后，平台列表只有GitHub、CSDN两项，StackOverflow并没有展示。主要是因为react根据key值认为CSDN和StackOverflow是同一个组件，导致第一个被渲染之后，后续的被丢弃。

如此一来，有了key值之后就与组件建立了一种对应关系，react根据key来决定销毁重新创建组件还是更新组件。

- **key值相同**：若组件属性有所变化，则react值更新对应组件对应的属性，没有变化则不更新。
- **key值不同**：则react先销毁该组件（有状态组件的componentWillUnmount会执行），然后重新创建该组件（有状态组件的constructor和componentWillUnmount都会执行）。

> 注意：key不是用来提升react性能的，但是用好key对性能的提升有帮助。

#### 8.3 key的使用场景
在项目开发中，**key属性大多应用在由数组动态创建子组件的情况下**，需要为每个子组件添加唯一的key属性值。
那么为什么有数组动态创建的组件必须使用key属性？这跟数组元素的动态性有关。
以上例为例，看一下babel对上述代码的转换情况：
```javascript
//转换前
const element = (
    <ul>
      {[<li key={1}>1:Github</li>, <li key={2}>2:CSDN</li>]}
    </ul>
);

//转换后
"use strict";

var element = React.createElement(
  "ul",
  null,
  [
    React.createElement("li",{ key: 1 },"1:Github"), 
    React.createElement("li",{ key: 2 },"2:CSDN")
  ]
);
```
由babel转换后React.createElement中的代码可以看出，key在其它元素中之所以不必须是因为不管组件的state或者props如何变化，这些元素始终占据着React.createElement固定的位置，这个位置就是天然的key。
而由数组创建的组件可能由于动态的操作导致重新渲染时，子组件的位置发生了变化，例如上面列表中新增一项，上面两项的位置可能变化为下面这样：
```javascript
var element = React.createElement(
  "ul",
  null,
  [
    React.createElement("li",{ key: 3 },"1:StackOverflow"), 
    React.createElement("li",{ key: 1 },"2:Github"), 
    React.createElement("li",{ key: 2 },"3:CSDN")
  ]
);
```
可以看出，数组创建子组件的位置并不固定，是动态改变的。这样有了key属性后，react就可以根据key值来判断是否为同一组件。

> 还有一种比较常见的场景是：为一个有复杂繁琐逻辑的组件添加key后，后续操作可以改变该组件的key属性值，从而达到先销毁之前的组件，再重新创建该组件的目的。

关于key属性更多详细信息，参见[React之Key详解](https://segmentfault.com/a/1190000009149186)。

### 9. 事件处理
React元素的事件处理和DOM元素的相似，但有语法上的不同。
- React事件绑定属性的命名采用驼峰式命名，不是小写
- 如果采用JSX的语法，需要传入一个函数作为事件处理函数，而不是一个字符串（DOM元素的写法）
```javascript
//传统的HTML
<button onclick="activateLasers()">
  //...
</button>

//React中的事件绑定
<button onClick={activateLasers}>
  //...
</button>
```
- 不能使用 return false 的方式阻止默认行为，必须明确的使用 preventDefault
```javascript
handleClick = (e) => {      //ES6箭头函数
  e.preventDefault();       //react中阻止默认行为的方法
  this.setState({
    value: 'value2'
  });
  console.log('The link was clicked.');
}
```
React 组件支持很多事件，除了 Click 事件外，还有 KeyDown 、Copy、Scroll 等，详细事件清单参见官方文档[支持的事件](https://doc.react-china.org/docs/events.html#支持的事件)。

### 10. 表单
用户在表单填入的内容，属于用户跟组件之间的交互，所以不能用 this.props 读取。

#### 10.1 简单表单
下面的例子中设置了输入框input的值value={this.state.data}，在输入框值发生变化的同时更新state。此处使用onChange事件来监听input值的变化并修改state：
```javascript
class HelloMessage extends React.Component{
  constructor(){
    super();
    this.state = {
      value:'Hello World!'
    }
  }

  handleChange = (event) => {
    this.setState({value: event.target.value});
  }

  render () {
    return (
      <div>
        <input 
          type="text" 
          value={this.state.value} 
          onChange={this.handleChange} 
        /> 
        <h4>{this.state.value}</h4>
      </div>
    );
  }
});

ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
)
```
该例渲染出一个input输入框，并通过onChange事件监听同步更新用户输入的值到state。

#### 10.2 在子组件上使用表单
下面的例子展示如何在子组件上使用表单。
```javascript
class Content extends React.Component{
  render () {
    return (
      <div>
        <input type="text" value={this.props.myDataProp} onChange={this.props.updateStateProp} />
        <h4>{this.props.myDataProp}</h4>
      </div>
    );
  }
}

class HelloMessage extends React.Component{
  constructor(){
    super();
    this.state = {
      value:"Hello World!"
    }
  }
  handleChange = (event) => {
    this.setState({value: event.target.value});
  }
  render() {
    let value = this.state.value;
    return (
      <div>
        <Content 
          myDataProp = {value} 
          updateStateProp = {this.handleChange}
        />
      </div>
    );
  }
});

ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
);
```
onChange方法将触发state的更新并将更新的值传递到组件的输入框的value上来重新渲染界面。

这里需要通过在父组件 HelloMessage 上创建事件句柄 handleChange ，并作为 prop(updateStateProp) 传递到子组件 Content 上。

### 11. 其他




