
## RequireJs使用教程
> 参考：http://www.runoob.com/w3cnote/requirejs-tutorial-1.html

<br/><br/>

### 什么是require.js

> RequireJS is a JavaScript file and module loader. It is optimized for in-browser use, but it can be used in other JavaScript environments, like Rhino and Node. Using a modular script loader like RequireJS will improve the speed and quality of your code.

RequireJS是一个轻量的JavaScript模块载入框架，是AMD规范最好的实现者之一，它以另一种方式引入js模块文件。最新版本的RequireJS压缩后只有14K。同时它还可以与其他框架协同工作，RequireJS可以使前端代码质量得以提升。

传统项目中，javascript代码都写在一个文件里面，只要加载这一个文件就够了。例如下面的代码依次加载多个js文件：
```html
<script src="1.js"></script>
<script src="2.js"></script>
<script src="3.js"></script>
<script src="4.js"></script>
```

这种写法的缺点在于：

- 加载时浏览器会停止网页渲染。加载文件越多，网页失去响应的时间就越长
- 由于js文件之间存在依赖关系，必须严格保证js文件的加载顺序。依赖性最大的模块必须放到最后加载，当依赖关系很复杂时，代码的编写和维护会变得困难。

require.js的出现很好地解决了这些问题。使用require.js具有如下优点：

- 实现js文件的异步加载，避免阻塞页面渲染
- 使用程序调用的方式加载js，防止出现上述大量加载js文件的现象
- 管理模块之间的依赖性，便于代码的编写和维护
<br/><br/>

### 如何使用require.js

1. **引入require.js文件**

首先需要到官网下载[require.js](http://requirejs.org/docs/download.html)的最新版本。然后放到本地指定目录（如/js）下，便可以加载使用。
```html
<script src="js/require.js"></script>
```
但是这种加载方式也会造成网页失去响应。解决办法有两个：

- 将require.js放到网页底部加载
- 指定异步加载属性 async
```html
    <script src="js/require.js" defer async="true" ></script>
```
async 属性表明该文件需要异步加载，避免网页失去响应。因为IE不支持该属性，只支持 defer ，所以需要加上 defer 属性。

require.js文件加载完成之后，就开始加载其他js文件了。假设需要加载的js文件是 main.js ，和 require.js 在同一目录下，那么main.js的加载只需要写成：
```html
<script src='js/require.js' data-main='js/main'></script>
```
这里 data-main 属性的作用是指定网页的主模块，即 main.js 文件。该文件会首先被require.js加载。

> 注意：由于require.js默认文件后缀名是js，所以可以把main.js简写成main。

2. **主模块 main.js 的写法**

我们可以把本章中讨论的main.js称为主模块，它是整个页面的入口代码。
如果js代码不依赖于其他任何模块，则可以直接写入js代码：
```javascript
//main.js
alert('loaded success!');
```
更多情况下js代码都会依赖于其他模块，所以此时就需要用到AMD规范定义的 require() 函数
```javascript
// main.js
require(['module1','module2','module3'], function(){arg1,arg2,arg3}{
    //...
})
```
require()函数接收两个参数：

- 数组array。表示所依赖的模块，即使只有一个值，也要写成数组的形式
- 回调函数callback。当前面指定的依赖模块都加载成功后便调用回调函数，加载的模块会以参数的形式传入该函数从而在回调函数内使用这些模块

看一个实例：
```javascript
require(['jquery','underscore','backbone'], function($, _, Backbone){
    //...
})
```
此时require.js会先加载jquery、underscore、backbone，然后再运行回调函数。主模块的代码写在回调函数中。

3. **模块的加载**

上面的示例中主模块依赖['jquery','underscore','backbone']，默认情况下require假定这三个模块与main.js在同一个目录下，文件名分别是 jquery.js、underscore.js、backbone.js，然后自动加载。

#### require.config()
使用require.config()方法可以自定义模块的加载，它位于主模块main.js头部，接受一个对象类型的参数，改参数的path属性指定各个模块的加载路径。
```javascript
require.config({
    paths:{
        'jquery': 'js/module/jquery.min',
        'underscore': 'js/module/underscore.min',
        'backbone': 'js/module/backbone.min'
    }
});
```
这里逐一指定了各个模块的路径名。还有一种写法是直接使用baseUrl属性改变基目录：
```javascript
require.config({
    baseUrl: 'js/module',
    paths:{
        'jquery': 'jquery.min',
        'underscore': 'underscore.min',
        'backbone': 'backbone'
    }
});
```
如果需要加载的js来自本地服务器、其他网站或CDN，可以这样配置：
```javascript
require.config({
    paths: {
        'jquery': 'http://libs.baidu.com/jquery/2.0.3/jquery',
        //...
    }
})
```


- 全局配置


- 第三方模块



