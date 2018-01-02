```
                                  ___                   ___  
  ________   _____ _____    _____/  |__           _    / /      _____    ___
  \_  __  \_/ __  \\__  \ _/ ___\   ___\  __  __ | |  / /__    /___  \  / _ \
    | | \/  \ ___/ / ___ \ \ \___|  |_    \ \/ / | | |  \  | _   _/ _/ | |_| |
    |_|      \___//_/   \_\ \___/ \___|    \__/  |_|  \___/ |_|/_____\  \___/
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








- ### 使用PropTypes检查类型

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


