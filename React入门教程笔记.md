```
                                 ___                   __
  ________  _____ _____    _____/  |__           _    / /      _____    ___
  \__  _  \/ __  \\__  \ _/ ___\   ___\  __  __ | |  / /__    /___  \  / _ \
    | / \/ | ___/ / ___ \ \ \___|  |_    \ \/ / | | |  \  | _  __/ _/ | |_| |
    |_|    \____//_/   \_\ \___/ \___|    \__/  |_|  \___/ |_|/_____\  \___/
```
## React 入门实例教程
> 参考：https://doc.react-china.org/docs/hello-world.html 、http://www.ruanyifeng.com/blog/2015/03/react <br/>
> 时间：2017/12/25

## 第一篇
- ### 安装

- ### Hello World

- ### JSX简介

- ### 元素渲染

- ### 组件 & Props

- ### State & 生命周期

- ### 事件处理

- ### 条件渲染
> 在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后还可以根据应用的状态变化只渲染其中的一部分。
React 中的条件渲染和 JavaScript 中的一致，使用 JavaScript 操作符 if 或条件运算符来创建表示当前状态的元素，然后让 React 根据它们来更新 UI。
``` javascript
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}
function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```
我们将创建一个 Greeting 组件，它会根据用户是否登录来显示其中之一：

此示例根据 isLoggedIn 的值渲染不同的问候语。



- ### 列表 & Keys

- ### 表单

- ### 状态提升

- ### 组合 & 继承

- ### React理念

<br/>
<br/>

## 第二篇
- ### 深入JSX
> 本质上来讲，JSX 只是为 React.createElement(component, props, ...children) 方法提供的语法糖。比如下面的代码：
```html
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```
编译为：
```javascript
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```
使用[在线 Babel 编译器](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&code=function%20hello()%20%7B%0A%20%20return%20%3Cdiv%3EHello%20world!%3C%2Fdiv%3E%3B%0A%7D)验证 JSX 是如何转换为 JavaScript 的。

----
### 指定React元素类型
JSX 的标签名决定了 React 元素的类型。
大写开头的 JSX 标签表示一个 React 组件。这些标签将会被编译为同名变量并被引用，所以如果你使用了 <Foo /> 表达式，则必须在作用域中先声明 Foo 变量。 
#### React必须声明
由于 JSX 编译后会调用 React.createElement 方法，所以在你的 JSX 代码中必须首先声明 React 变量。<br/>
比如，下面两个导入都是必须的，尽管 React 和 CustomButton 都没有在代码中被直接调用。
```javascript
import React from 'react';
import CustomButton from './CustomButton';
function WarningButton() {
  // 返回 React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```
如果你使用script标签加载 React，它将作用于全局。

#### 点表示法
还可以使用 JSX 中的点表示法来引用 React 组件。你可以方便地从一个模块中导出许多 React 组件。例如，有一个名为 MyComponents.DataPicker 的组件，你可以直接在 JSX 中使用它：
```javascript
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

#### 首字母大写
当元素类型以小写字母开头时，它表示一个内置的组件，如 <div> 或 <span>，并将字符串 ‘div’ 或 ‘span’ 传 递给 React.createElement。 以大写字母开头的类型，如 <Foo /> 编译为 React.createElement(Foo)，并它正对应于你在 JavaScript 文件中定义或导入的组件。<br/>
我们建议用大写开头命名组件。如果你的组件以小写字母开头，请在 JSX 中使用之前其赋值给大写开头的变量。<br/
例如，下面的代码将无法按预期运行：
```javascript
import React from 'react';

// 错误！组件名应该首字母大写:
function hello(props) {
  // 正确！div 是有效的 HTML 标签:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 错误！React 会将小写开头的标签名认为是 HTML 原生标签:
  return <hello toWhat="World" />;
}
```
为了解决这个问题，我们将 hello 重命名为 Hello，然后使用 <Hello /> 引用。

#### 在运行时选择类型
你不能使用表达式来作为 React 元素的标签。如果你的确想通过表达式来确定 React 元素的类型，请先将其赋值给大写开头的变量。这种情况一般会在你想通过属性值条件渲染组件时出现：
```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 错误！JSX 标签名不能为一个表达式。
  return <components[props.storyType] story={props.story} />;
}
```
要解决这个问题，我们需要先将类型赋值给大写开头的变量。
```javascript
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 标签名可以为大写开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```
----

### 属性
在 JSX 中有几种不同的方式来指定属性。
##### 使用 JavaScript 表达式
```html
<MyComponent foo={1 + 2 + 3 + 4} />
```
if 语句和 for 循环在 JavaScript 中不是表达式，因此它们不能直接在 JSX 中使用，所以你可以将它们放在周围的代码中。
```javascript
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

#### 字符串常量
你可以将字符串常量作为属性值传递。下面这两个 JSX 表达式是等价的：
```javascript
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

#### 默认为 True
如果你没有给属性传值，它默认为 true。因此下面两个 JSX 是等价的：
```javascript
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```
一般情况下，我们不建议这样使用，因为它会与 ES6 对象简洁表示法 混淆。比如 {foo} 是 {foo: foo} 的简写，而不是 {foo: true}。这里能这样用，是因为它符合 HTML 的做法。

#### 扩展属性
如果你已经有了个 props 对象，并且想在 JSX 中传递它，你可以使用 ... 作为扩展操作符来传递整个属性对象。下面两个组件是等效的：
```javascript
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```
当你构建通用容器时，扩展属性会非常有用。然而，这样做也可能让很多不相关的属性，传递到不需要它们的组件中使代码变得混乱。我们建议你谨慎使用此语法。

----

### 子代
#### 字符串常量
你可以在开始和结束标签之间放入一个字符串，则 props.children 就是那个字符串。这对于许多内置 HTML 元素很有用。例如：
```html
<MyComponent>Hello world!</MyComponent>
```
这是有效的 JSX，并且 MyComponent 的 props.children 值将会直接是 "hello world!"。因为 HTML 未转义，所以你可以像写 HTML 一样写 JSX：
```html
<div>This is valid HTML &amp; JSX at the same time.</div>
```
JSX 会移除行空行和开始和结尾处的空格。标签邻近的新行也会被移除，字符串常量内部的换行会被压缩成一个空格，所以下面这些都等价：
```html
<div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

#### JSX
你可以通过子代嵌入更多的 JSX 元素，这对于嵌套显示组件非常有用：
```html
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```
你可以混合不同类型的子元素，同时用字符串常量和 JSX 子元素，这是 JSX 类似 HTML 的另一种形式，这在 JSX 和 HTML 中都是有效的：
```html
<div>
  Here is a list:
  <ul>
    <li>Item 1</li>
    <li>Item 2</li>
  </ul>
</div>
```
React 组件也可以通过数组的形式返回多个元素：
```javascript
render() {
  // 不需要使用额外的元素包裹数组中的元素
  return [
    // 不要忘记 key :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```

#### JavaScript 表达式
你可以将任何 {} 包裹的 JavaScript 表达式作为子代传递。例如，下面这些表达式是等价的：
```html
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```
这对于渲染任意长度的 JSX 表达式的列表很有用。例如，下面将会渲染一个 HTML 列表：
```javascript
function Item(props) {
  return <li>{props.message}</li>;
}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}
    </ul>
  );
}
```
JavsScript 表达式可以与其他类型的子代混合使用。这通常对于字符串模板非常有用：
```javascript
function Hello(props) {
  return <div>Hello {props.addressee}!</div>;
}
```
#### 函数
通常情况下，插入 JSX 中的 JavsScript 表达式将被认作字符串、React 元素或这些内容的列表。然而，props.children 可以像其它属性一样传递任何数据，而不仅仅是 React 元素。例如，如果你使用自定义组件，则可以将调用 props.children 来获得传递的子代：
```javascript
// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```
传递给自定义组件的子代可以是任何元素，只要该组件在 React 渲染前将其转换成 React 能够理解的东西。这个用法并不常见，但当你想扩展 JSX 时可以使用。

#### 布尔值、Null 和 Undefined 被忽略
false、null、undefined 和 true 都是有效的子代，但它们不会直接被渲染。下面的表达式是等价的：
```html   
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```  
这在根据条件来确定是否渲染React元素时非常有用。以下的JSX只会在showHeader为true时渲染<Header />组件。
```html
 <div>
   {showHeader && <Header />}
   <Content />
 </div>
```
值得注意的是，JavaScript 中的一些 “falsy” 值(比如数字0)，它们依然会被渲染。例如，下面的代码不会像你预期的那样运行，因为当 props.message 为空数组时，它会打印0:
```html
 <div>
   {props.messages.length &&
     <MessageList messages={props.messages} />
   }
 </div>
```
要解决这个问题，请确保 && 前面的表达式始终为布尔值：
```html
 <div>
   {props.messages.length > 0 &&
     <MessageList messages={props.messages} />
   }
 </div>
``` 
相反，如果你想让类似 false、true、null 或 undefined 出现在输出中，你必须先把它转换成字符串 :
```html
 <div>
   My JavaScript variable is {String(myVariable)}.
 </div>
``` 
<br/>
<br/>

- ### 使用PropTypes检查类型
> 注意: React.PropTypes 自 React v15.5 起已弃用。请使用 prop-types 库代替。

随着应用日渐庞大，你可以通过类型检查捕获大量错误。 对于某些应用来说，你还可以使用 Flow 或 TypeScript 这样的 JavsScript 扩展来对整个应用程序进行类型检查。然而即使你不用它们，React 也有一些内置的类型检查功能。要检查组件的属性，你需要配置特殊的 propTypes 属性：
```javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```
PropTypes 包含一整套验证器，可用于确保你接收的数据是有效的。在这个示例中，我们使用了 PropTypes.string。当你给属性传递了无效值时，JavsScript 控制台将会打印警告。出于性能原因，propTypes 只在开发模式下进行检查。

----

### PropTypes
下面是使用不同验证器的例子：
```javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为以下 JS 原生类型
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

  // 你也可以声明属性为某个类的实例，这里使用 JS 的
  // instanceof 操作符实现。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你也可以限制你的属性值是某个特定值之一
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

  // 你也可以在任何 PropTypes 属性后面加上 `isRequired`
  // 后缀，这样如果这个属性父组件没有提供时，会打印警告信息
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你也可以指定一个自定义验证器。它应该在验证失败时返回
  // 一个 Error 对象而不是 `console.warn` 或抛出异常。
  // 不过在 `oneOfType` 中它不起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 不过你可以提供一个自定义的 `arrayOf` 或 `objectOf`
  // 验证器，它应该在验证失败时返回一个 Error 对象。 它被用
  // 于验证数组或对象的每个值。验证器前两个参数的第一个是数组
  // 或对象本身，第二个是它们对应的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```
----

### 限制单个子代
使用 PropTypes.element 你可以指定只传递一个子代
```javascript
import PropTypes from 'prop-types';

 class MyComponent extends React.Component {
   render() {
     // This must be exactly one element or it will warn.
     const children = this.props.children;
     return (
       <div>
         {children}
       </div>
     );
   }
 }

 MyComponent.propTypes = {
   children: PropTypes.element.isRequired
 };
 ```
----

### 属性默认值
你可以通过配置 defaultProps 为 props定义默认值：
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
 如果你在使用像 transform-class-properties 的 Babel 转换器，你也可以在React 组件类中声明 defaultProps 作为静态属性。这个语法还没有最终通过，在浏览器中需要一步编译工作。更多信息，查看类字段提议。
 ```javascript
 class Greeting extends React.Component {
    static defaultProps = {
      name: 'stranger'
    }

    render() {
      return (
        <div>Hello, {this.props.name}</div>
      )
    }
  }
  ```
  defaultProps 用来确保 this.props.name 在父组件没有特别指定的情况下，有一个初始值。类型检查发生在 defaultProps 赋值之后，所以类型检查也会应用在 defaultProps 上面。

----
<br/><br/>

- ### 静态类型检查

- ### Refs & DOM

- ### 非受控组件

- ### 性能优化

- ### 不使用ES6

- ### 不是用JSX

- ### Reconciliation

- ### Context

- ### Fragments

- ### Portals

- ### Error boundaries

- ### Web components

- ### 高阶组件

- ### 与第三方库协同

- ### 可访问性


