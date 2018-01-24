## Redux学习笔记
> 作    者: Henery(henery_002@163.com) <br/>
> 时    间: 2018/01/18 <br/>
> 参考文章： <br/>

> 注：本文参照[Redux中文文档](http://cn.redux.js.org/)整理编写，可移步官网查看完整版。

<br/><br/>

## 目录：





<br/><br/>

## 正文
### 关于Redux
#### 1. 核心概念

在Redux中，当使用普通对象俩描述运用的state时，例如：todo应用的state可能长这样：
```jajascript
{
    todos:[{
        text: 'Eat food',
        completed: true
    },{
        text: 'Exercise',
        completed: false
    }],
    visibilityFilter: 'SHOW_COMPLETED'
}
```
这个对象就像"Model"，区别在于它并没有setter修改器方法。所以其他的代码不能随意修改它，造成难以复现的bug。

如果想要更新state中的数据，需要发起一个action。Action就是一个普通的js对象，用来描述发生的事情。看下面这些action示例：
```javascript
{ type: "ADD_TODO", text: "Go to swimming pool" }
{ type: "TOGGLE_TODO", index: 1 }
{ type: "SET_VISIBILT_FILTER", filter: "SHOW_ALL" }
```
强制使用action来描述所有变化带来的好处是可以清晰的知道应用中到底发生了什么。最终，为了把action和state串起来开发一些函数，就形成了reducer。reducer只是一个接收state和action并返回新的state的函数。对于大型应用来说，可能很难开发这样的函数，所以会编写很多小函数来分别管理state的一部分：
```javascript
function visibilityFilter(state = 'SHOW_ALL', action){
    if(action.type == 'SET_VISIBILITY_FILTER'){
        return action.filter;
    } else {
        return state;
    }
}

function todos(state = [], action){
    switch (action.type){
        case 'ADD_TODO':
            return state.concat([{ text: action.text, completed: false }]);
        case 'TOGGLE_TODO':
            return state.map((todos, index) => 
                action.index === index ? { text:todo.text, completed: !todo.completed } : todo
            )
        default:
            return state;
    }
}
```
然后再开发一个reducer调用这两个reducer，进而来管理整个应用的state：
```javascript
function todoApp(state = {}, action){
    return {
        todos: todos(state.todos, action),
        visibilityFilter: visibilityFilter(state.visibilityFilter, action)
    };
}
```
这基本上就是Redux思想的全部。这里没有用到Redux的任何API。Redux里有一些工具来简化这种模式，但是主要思想是如何根据这些action对象来更新state，而且基本都是纯js代码，没涉及到Redux及其API。
<br/>

#### 2. 三大原则

Redux可以用以下三个基本原则来描述：

- **单一数据源**

> 在Redux中，整个应用的state被存储在一棵object tree中，并且这个object tree只存在于唯一一个store中。

这让同构应用开发变得容易，来自服务器端的state可以在无需编写更多代码的情况下并序列化并注入到客户端中。由于是单一的state，调试也变得非常容易。
```javascript
console.log(store.getState());

/* 输出
{
    visilbilityFilter: "SHOW_ALL",
    todos: [
        {
            text: "Consider using Redux.",
            cmpleted: true
        },
        {
            text: "Keep all state in a single tree.",
            completed: false
        }
    ]
}
*/
```

- **State只读**

> 唯一改变state的方法就是触发action。action是一个用以描述所发生的事件的普通对象。

这样确保了视图和网络请求都不能直接修改state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理且严格按照一个接一个的顺序执行，所以不用担心race condition的出现。Action就是一个普通对象，可以被日志打印、序列化、存储、后期调试或测试时回放。
```javascript
store.dispatch({
    type: 'COMPLETE_TODO',
    inde: 1
})

store.dispatch({
    type: 'SET_VISIBILITY_FILTER',
    filter: 'SHOW_COMPLETED'
})
```

- **使用纯函数执行修改**

> 为了描述action如何改变state tree，需要编写reducers。

Reducer只是一些纯函数，它接收之前的state和action并返回新的state。最初，reducer可能只有一个，随着应用变大，可以把reducer拆成多个小的reducers，分别用以独立操作state tree的不同部分。因为reducer只是函数，所以可以控制它们被调用的顺序，转入附加数据，甚至编写一个可复用的reducer来处理一些通用任务，如分页器。
```javascript
function visibilityFIlter(state = 'SHOW_ALL', action){
    switch(action.type){
        case 'SET_VISIBILITY_FILTER':
            return action.filter;
        default:
            reutn state;
    }
}

function todos(state = [], action){
    switch(action.type){
        case 'ADD_TODO':
            return [
                ...state,
                {
                    text: action.text,
                    completed: false
                }
            ];
        case 'COMPLETE_TODO':
            return state.map((todos, index) => {
                if(index === action.index){
                    return Object.assign({}, todo, {
                        completed: true
                    })
                }
                return todo;
            });
        default:
            return state;
    }
}

import { combineReducers, createStore } from 'redux';
let reducer = combineReducers({ visibilityFilter, todos });
let store = createStore(reducer);
```
<br/>

### Redux基本概念
#### 1. Action

#### 2. Reducer

#### 3. Store

#### 4. 数据流

#### 5. 搭配React

#### 6.示例：Todo List


### Redux高级内容
#### 1. 异步Action

#### 2. 异步数据流

#### 3. 中间件（Middleware）

#### 4. 搭配React Router





