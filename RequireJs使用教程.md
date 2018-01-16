
## RequireJs使用教程
> 参考：http://www.runoob.com/w3cnote/requirejs-tutorial-1.html

### 什么是require.js

> RequireJS is a JavaScript file and module loader. It is optimized for in-browser use, but it can be used in other JavaScript environments, like Rhino and Node. Using a modular script loader like RequireJS will improve the speed and quality of your code.

RequireJS是一个轻量的JavaScript模块载入框架，是AMD规范最好的实现者之一。最新版本的RequireJS压缩后只有14K。同时它还可以与其他框架协同工作，RequireJS可以使前端代码质量得以提升。

传统项目中，javascript代码都写在一个文件里面，只要加载这一个文件就够了。例如下面的代码依次加载多个js文件：
```javascript
<script src="1.js"></script>
<script src="2.js"></script>
<script src="3.js"></script>
<script src="4.js"></script>
```

这种写法的缺点在于：

- 加载时浏览器会停止网页渲染。加载文件越多，网页失去响应的时间就越长
- 由于js文件之间存在依赖关系，必须严格保证js文件的加载顺序。依赖性最大的模块必须放到最后加载，当依赖关系很复杂时，代码的编写和维护会变得困难。

require.js的出现很好地解决了这些问题。使用require.js具有如下优点：

- 防止js加载阻塞页面渲染
- 使用程序调用的方式加载js，防止出现上述大量加载js文件的现象




### 如何使用require.js

- 加载文件


- 全局配置


- 第三方模块



