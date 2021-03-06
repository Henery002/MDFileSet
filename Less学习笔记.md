## Less 学习笔记
> 作    者：Henery(henery_002@163.com) <br/>
> 时    间：2017/12/17  
> 参考文章：Less官方网站[lesscss.cn](http://lesscss.cn/features/)

> 转载请注明出处。
<br/>

## 目录：
1. [简介](#简介)
2. [less原理及使用方式背景](#less原理及使用方式背景)
3. [语法](#语法)
    1. [Variables](#variables)
    2. [Mixins](#mixins)
    3. [Nested Rules（嵌套规则）](#nested-rules嵌套规则)
    4. [Operations 及 Functions](#operations-及-functions)
    5. [Comments](#comments)
    6. [Less vs Sass](#less-vs-sass)
4. [结束语](#结束语)
5. [相关主题](#相关主题)
<br/><br/>

## 正文：
### 简介
css的语法相对简单，对使用者的要求较低，但同时也带来一些问题：css是一门非程序式语言，没有变量、函数、scope（作用域）等概念，因此需要书写大量看似没有逻辑的代码，不方便维护及扩展，不利于复用，很难写出组织良好且易于维护的css代码。
less在css的语法基础之上，引入了变量、Mixin、运算以及函数等功能，大大简化了css的编写，并且降低了css的维护成本。

### less原理及使用方式背景
本质上，less 包含一套自定义的语法及一个解析器，用户根据这些语法定义自己的样式规则，这些规则最终会通过解析器，编译生成对应的 css 文件。less 并没有裁剪 css 原有的特性，更不是用来取代 css 的，而是在现有 css 语法的基础上，为其加入程序式语言的特性。下面是一个简单的例子：
``` less
@color: #4D926F;  
#header { 
    color: @color; 
} 
h2 { 
    color: @color; 
}
```
经过编译生成的 css 文件如下：
``` css
#header { 
    color: #4D926F; 
} 
h2 { 
    color: #4D926F; 
}
```
less 可以直接在客户端使用，也可以在服务器端使用。在实际项目开发中，推荐使用第三种方式，即将 less 文件编译生成静态 css 文件，并在 html 文档中应用。
* #### 客户端
从 http://lesscss.org下载 less.js 源文件后就开展以直接在客户端使用，然后在 html 中引入 less 源文件：
``` html 
<link rel="stylesheet/less" type="text/css" href="styles.less">
```
less 源文件的引入方式与标准同 css。
需要注意的是，在引入 .less 文件时，rel 属性要设置为“stylesheet/less”且 less 源文件一定要在 less.js 引入之前引入，这样才能保证 less 源文件正确编译解析。
* #### 服务器端
less 在服务器端的使用借助于 less 编译器，将 less 源文件编译生成最终的 css 文件。可以使用 node 包管理器 npm 安装 less，安装成功后就可以在 node 环境中对 less 源文件进行编译。
在项目开发初期，无论采用客户端或服务器端用法，都要想办法将用到的 css 或 less 文件引入到 html 页面或桥接文件中，less 中使用 [Importing](http://lesscss.cn/features/#features-overview-feature-importing)。可以通过 @import 引入需要的 .less 或 .css 文件。 如：
``` js
@import “variables.less”;
```
.less 文件也可以省略后缀名，像这样：
``` js
@import “variables”;
```
引入 css 同 less 文件一样，只是 .css 后缀名不能省略。
* #### 使用编译生成的静态css文件
通过 less 编译器，将 less 文件编译成为 css 文件，在 html 文章中引入使用。这里要强调的一点，less 完全兼容 css 语法，也就是说可以将标准的 css 文件直接改成 .less 格式，less 编译器可以完全识别。

### 语法
1. #### [Variables](http://lesscss.cn/features/#variables-feature)
less 允许开发者自定义变量，变量可以在全局样式中使用，变量使得样式修改更简便。
``` less
@border-color : #b5bcc7; 
.mythemes tableBorder{ 
    border : 1px solid @border-color; 
}
```
经过编译生成的 css 文件如下：
``` css
.mythemes tableBorder { 
    border: 1px solid #b5bcc7; 
}
```
从上述代码中可以看出，变量是 value（值）级别的复用，可以将相同的值定义成变量统一管理。
该特性适用于定义主题，如将背景颜色、字体颜色、边框属性等常规样式进行统一定义，这样不同的主题只需要定义不同的变量文件即可。该特性也同样适用于 CSS RESET（重置样式表），在 web 开发中，有时需要屏蔽浏览器默认的样式行为而需要重新定义样式表来覆盖浏览器的默认行为，使用 less 的变量特性就可以在不同的项目间重用样式表，仅需要在不同的项目样式表中根据需求重新给变量赋值即可。
less 中的变量和其他编程语言一样，可以实现值的复用，同样它也有作用域 [Scope](http://lesscss.cn/features/#features-overview-feature-scope)，即局部变量 or 全局变量的概念，查找变量的顺序是先在局部定义中找，如果找不到，则查找上级定义，直至全局。
``` less
@width : 20px; 
#homeDiv { 
    @width : 30px; 
    #centerDiv{ 
        width : @width;// 此处应该取最近定义的变量 width 的值 30px 
    } 
} 
#leftDiv { 
    width : @width; // 此处应该取最上面定义的变量 width 的值 20px 
}
```
经过编译生成的 css 文件如下：
``` css
#homeDiv #centerDiv { 
    width: 30px; 
} 
#leftDiv { 
    width: 20px; 
}
```
2. #### [Mixins](http://lesscss.cn/features/#mixins-feature)
同 Sass 一样，很多动态语言都支持 mixins 特性，它是多重继承的一种实现。在 less 中，mixins 是指在一个 class 中引入另外一个已经定义的 class，就像在当前 class 中增加一个属性一样。

mixins 在 less 中的使用：
``` less
// 定义一个样式选择器
 .roundedCorners(@radius:5px) { 
     -moz-border-radius: @radius; 
     -webkit-border-radius: @radius; 
     border-radius: @radius; 
 } 
 // 在另外的样式选择器中使用
 #header { 
    .roundedCorners; 
 } 
 #footer { 
    .roundedCorners(10px); 
 }
```
经过编译生成的 css 文件如下：
``` css
#header { 
    -moz-border-radius:5px; 
    -webkit-border-radius:5px; 
    border-radius:5px; 
} 
#footer { 
    -moz-border-radius:10px; 
    -webkit-border-radius:10px; 
    border-radius:10px; 
}
```
从上面的代码我们可以看出：mixins 其实是一种嵌套，它允许将一个类嵌入到另外一个类中使用，被嵌入的类也可以称作变量。

- mixins 还有一种形式叫做 [Parametric Mixins](http://lesscss.cn/features/#mixins-parametric-feature)（混入参数），less 也支持这一特性：
``` less
// 定义一个样式选择器
 .borderRadius(@radius){ 
     -moz-border-radius: @radius; 
     -webkit-border-radius: @radius; 
     border-radius: @radius; 
 } 
 // 使用已定义的样式选择器
 #header { 
    .borderRadius(10px); // 把 10px 作为参数传递给样式选择器
 } 
 .btn { 
    .borderRadius(3px);// // 把 3px 作为参数传递给样式选择器
 }
```
经过编译生成的 css 文件如下：
``` css
#header { 
    -moz-border-radius: 10px; 
    -webkit-border-radius: 10px; 
    border-radius: 10px; 
} 
.btn { 
    -moz-border-radius: 3px; 
    -webkit-border-radius: 3px; 
    border-radius: 3px; 
}
```
- 还可以给 mixins 的参数定义一个默认值，如
``` less
.borderRadius(@radius:5px){ 
     -moz-border-radius: @radius; 
     -webkit-border-radius: @radius; 
     border-radius: @radius; 
 } 
 .btn { 
    .borderRadius; 
 }
```
经过编译生成的 css 文件如下：
``` css
.btn { 
    -moz-border-radius: 5px; 
    -webkit-border-radius: 5px; 
    border-radius: 5px; 
}
```
- 像 js 中 arguments 一样，mixins 也有 [@arguments](http://lesscss.cn/features/#mixins-parametric-feature-the-arguments-variable) 变量。当 mixins 引用这个参数时，该参数表示所有的变量。很多情况下该参数可以省去很多代码。
``` less
.boxShadow(@x:0,@y:0,@blur:1px,@color:#000){ 
    -moz-box-shadow: @arguments; 
    -webkit-box-shadow: @arguments; 
    box-shadow: @arguments; 
} 
#header { 
    .boxShadow(2px,2px,3px,#f36); 
}
```
经过编译生成的 css 文件如下：
``` css
#header { 
    -moz-box-shadow: 2px 2px 3px #FF36; 
    -webkit-box-shadow: 2px 2px 3px #FF36; 
    box-shadow: 2px 2px 3px #FF36; 
}
```
- 为了解决在团队开发中使用大量选择器时解决选择器之间的重名问题，less 在 mixins 的基础上做了扩展，采用命名空间（[Namespaces](http://lesscss.cn/features/#features-overview-feature-namespaces-and-accessors)）的方法避免重名：
``` less
#mynamespace { 
    .home {...} 
    .user {...} 
}
```
这样就定义了一个名为 mynamespace 的命名空间，如果要复用 user 这个选择器，只需要在需要混入这个选择器的地方这样使用即可。
```
#mynamespace > .user。
```
3. #### [Nested Rules（嵌套规则）](http://lesscss.cn/features/#features-overview-feature-nested-rules)
css 中会采用从外到内的选择器嵌套定义或给特定元素加 class 或 id 的方式解决多层元素嵌套的情况。在 less 中可以这样写：
``` html
<div id="home"> 
    <div id="top">top</div> 
    <div id="center"> 
        <div id="left">left</div> 
        <div id="right">right</div> 
    </div> 
</div>
```
``` less
#home{ 
  color : blue; 
  width : 600px; 
  height : 500px; 
  border:outset; 
  #top{ 
       border:outset; 
       width : 90%; 
  } 
  #center{ 
       border:outset; 
       height : 300px; 
       width : 90%; 
       #left{ 
            border:outset; 
            float : left; 
            width : 40%; 
       } 
       #right{ 
            border:outset; 
            float : left; 
            width : 40%; 
       } 
   } 
}
```
经过编译生成的 css 文件如下：
``` css
#home { 
    color: blue; 
    width: 600px; 
    height: 500px; 
    border: outset; 
} 
#home #top { 
    border: outset; 
    width: 90%; 
} 
#home #center { 
    border: outset; 
    height: 300px; 
    width: 90%; 
} 
#home #center #left { 
    border: outset; 
    float: left; 
    width: 40%; 
} 
#home #center #right { 
    border: outset; 
    float: left; 
    width: 40%; 
}
```
从上面的代码中可以看出，less 嵌套规则的写法是 html 中的 DOM 结构相对应的，这样样式表书写更加简洁，可读性更好。同时嵌套规则使得对伪元素的操作更为方便。
``` less
a { 
    color: red; 
    text-decoration: none; 
    &:hover {                           // 有 & 时解析的是同一个元素或此元素的伪类，没有 & 解析是后代元素
        color: black; 
        text-decoration: underline; 
    } 
}
```
经过编译生成的 css 文件如下：
``` css
a { 
    color: red; 
    text-decoration: none; 
} 
a:hover { 
    color: black; 
    text-decoration: underline; 
}
```
4. #### [Operations](http://lesscss.cn/features/#features-overview-feature-operations) 及 [Functions](http://lesscss.cn/features/#features-overview-feature-functions)
css 中数值型 value 如 color、padding、margin 等在某些情况下有着一定关系。less 中是这样组织处理这些数值之间的关系的：
``` less
@init: #111111; 
@transition: @init*2; 
.switchColor { 
    color: @transition; 
}
```
经过编译生成的 css 文件如下：
``` css
.switchColor { 
 color: #222222; 
}
```
使用 less 的 operations 特性可以对数值型的 value（数字、颜色、变量等）进行加减乘除四则运算。同时 less 针对 color 操作提供了一组函数：
``` less
lighten(@color, 10%);           // return a color which is 10% *lighter* than @color 
darken(@color, 10%);            // return a color which is 10% *darker* than @color 
saturate(@color, 10%);          // return a color 10% *more* saturated than @color 
desaturate(@color, 10%);        // return a color 10% *less* saturated than @color 
fadein(@color, 10%);            // return a color 10% *less* transparent than @color 
fadeout(@color, 10%);           // return a color 10% *more* transparent than @color 
spin(@color, 10);               // return a color with a 10 degree larger in hue than @color 
spin(@color, -10);              // return a color with a 10 degree smaller hue than @color
```
PS: 上述代码引自 less css 官方网站，详情请参见 [http://lesscss.org/functions/#color-definition]( http://lesscss.org/functions/#color-definition)
这些函数的使用方法同 js 中函数的使用。
``` less
init: #f04615; 
#body { 
    background-color: fadein(@init, 10%); 
}
```
经过编译生成的 css 文件如下：
``` css
#body { 
    background-color: #f04615; 
}
```
less 中的这些函数同样可以被调用和传递参数。其主要作用是提供颜色变换的功能，先把颜色转换成 HSL 色，然后在此基础上进行操作。less 还提供了获取颜色值的方法，这里不一一赘述。
less 提供的运算及函数特性适用于实现页面组件特性，比如组件切换时的渐入渐出。

5. #### [Comments](http://lesscss.cn/features/#features-overview-feature-comments)
适当的注释是保证代码可读性的必要手段，less 对注释的支持有两种方式：单行注释和多行注释。类似与 js 中的注释，但有一点不同的是：less 中单行注释的内容(// 单行注释 ) 在编译后的 css 中不显示，需要加以注意。

6. #### Less vs Sass
同类框架还有 Sass : [http://sass-lang.com/](http://sass-lang.com/)。与 less 相比，两者都属于 css 预处理器，功能上大同小异，都是使用类似程序式语言的方式书写 css, 都具有变量、mixins、嵌套、继承等特性，最终目的都是方便 css 的书写及维护。
less 和 Sass 互相促进互相影响，相比之下 less 更接近 css 语法。两者之间的详细比较可以参阅 [https://gist.github.com/674726](https://gist.github.com/674726)。
### 结束语
本文针对 less 的基本功能做了概述，更多高级功能如：字符串插值、服务器端使用配置、js 表达式、避免编译等可以参见 less 官网。
### 相关主题
* [Less 官方网站](http://lesscss.org/)
* [Sass 官方网站](http://sass-lang.com/)
* [Sass/Less Comparison](https://www.cnblogs.com/wangpenghui522/p/5467560.html)
