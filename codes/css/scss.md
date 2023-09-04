<!-- https://www.ruanyifeng.com/blog/2012/06/sass.html -->
# 详解sass用法

## 前言

作为一名前端开发者，相信都对[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS)有所认知。它并不是一门编程语言，因此它无任何逻辑可言。你可以用它美化网页，但是你不能用它完成一些复杂的逻辑功能。因为它没有循环，没有条件语句。因此，很快就有人想到为它添加编程特性，这就被叫做“[CSS预处理器(css preprocessor)](http://www.catswhocode.com/blog/8-css-preprocessors-to-speed-up-development-time)”。它的基本思想就是用一种专门的编程语言来进行网页的样式设计，最后再编译成正常的CSS。

而今比较流行的CSS预处理器有[scss](https://sass-lang.com/)、[less](https://lesscss.org/)和[stylus](https://stylus-lang.com/)。本文将不对后两者做介绍，只对scss做一个介绍，并且希望能帮助前端开发入门scss。有了这篇文章，日常开发应该都不需要去看[官方文档](https://sass-lang.com/documentation)了。

首先，我们需要知道一点，那就是sass与scss到底是什么关系?我的回答如下:

简而言之，scss是sass的一个升级版本，完全兼容sass之前的功能，又有了些新增能力。语法形式上有些许不同，最主要的就是sass是靠缩进表示嵌套关系，scss是花括号。如:

sass写法:

```sass
div
    width:100px;
    span
      color:#f0f0f0;
```

scss写法:

```scss
div {
    width:100px;
    span {
        color:#f0f0f0;
    }
}
```

因此，本文只对scss做详解，sass不做介绍。

## scss的安装与使用

### 安装

scss最初是使用Ruby语言写的，但是两者的语法没有关系。不懂Ruby照样可以使用scss，只不过需要先[安装Ruby](https://www.ruby-lang.org/zh_cn/downloads/)，然后再安装scss。假设你已经安装好了Ruby,在终端命令行中可以输入如下的命令:

```shell
gem install scss
```

当然如果不安装Ruby也可以，那就需要安装使用node编写的sass，也就是[node-sass](https://github.com/sass/node-sass)。当然如果node-sass比较难以安装，可以使用淘宝镜像源来安装（或者科学上网）。

> ps: 当然如果你使用[vite](https://vitejs.dev/)构建工具搭建项目，由于该构建工具内置了scss功能，因此你只需要安装sass依赖即可。如下所示:

```shell
npm install sass -D //yarn add sass
```

在webpack构建工具搭建的项目中，你需要安装node-sass和sass,如下所示:

```shell
npm install node-sass sass -D//yarn add node-sass sass
```

安装完成之后，你就可以使用了。

对于react项目或者其他项目，我们通常都是创建一个scss文件，格式为《文件名》 + ".scss"，然后就可以在这个文件里面写scss语法呢。编写完成之后，可以在命令终端输入:

```shell
sass 文件名.scss
```

这样就可以在屏幕上显示转化后的css代码。当然如果你想要编译并输出为css文件，则可以输入以下命令:


```shell
sass 文件名.scss 文件名.css
```

比如你的文件名是common.scss,你想要输出为common.css,命令就可以写成如下这样:

```shell
sass common.scss common.css
```

实际上，在webpack的项目当中，我们都可以安装sass和sass-loader即可使用scss语法。接下来，我们来看scss的核心语法。

## scss的核心语法

### 1.变量

变量的使用也很简单，基本格式为

```scss
$ + 变量名（命名规范和JavaScript变量命名规范基本一致）
```

通常我们将页面中最常用到的样式配置在变量中，这样我们再具体页面当中使用的时候就是使用该变量，然后我们可以只改变量就可以达到更改整个布局中的常用样式，最常用的就是更换主题。比如页面中我们假设所有字体的颜色都是白色。

> variable.scss

```scss
$white:#fff;
```

> 页面一:page1.scss

```js
//假定页面的祖先父元素拥有一个container的类名
.container {
    color:$white;
}
```

> 页面二:page2.scss

```js
//假定页面的祖先父元素拥有一个container的类名
.container {
    color:$white;
}
```

......

可以看到，我们将字体颜色配置在变量中，假如后期需要将字体更换为#535455类似的字体色，那么我们只需要修改variable.scss文件中$white的值即可。

事实上，我们还会在项目当中给所有的元素类名添加一个前缀，典型的代表为element ui的ui组件库，这个组件库的元素类名有一个基本的前缀，即el-，因此可以定义一个基本的变量当做全局前缀。如下所示:

> variable.scss

```scss
$baseSelector:el-;
```

### 2. 插值

在页面当中使用该前缀名的时候实际上就相当于es6中的模板字符串那样使用变量来拼接。因此，比如页面有一个el-input的input框，在input.scss中，我们可以写成如下的样式。

```scss
//这里的结果会被编译成
//.el-input { //input框的样式 }
.#{$baseSelector}input {
    //input框的样式
}
```

可以看到，这种写法在scss中被叫做插值，语法格式为:

```scss
#{变量名} //也就是#加一对大括号，大括号中加入变量名
```

事实上插值的用法不仅可以用在类名中，还可以用在属性中。例如:

```scss
.el-input {
    $w:width;
    #{$w}:150 + px;
}
//以上代码会被编译成
// .el-input {
//     width:150px;
// }
```

### 3.嵌套

嵌套应该是每一个sass开发者都最常用的功能，就和标签嵌套类似。例如页面有这样的结构:

```html
<div class="container">
    <span class="fs-14">字体大小为14px</span>
</div>
```

则我们可以使用嵌套的语法为span标签加上样式，如下所示:

```scss
.container {
    .fs-14 {
        font-size:14px;
    }
}
```

在嵌套中，如果我们要对嵌套的最顶层根元素设置样式，我们可以使用`&`符号。如下所示:

```scss
.container {
    //这里的&指代的就是.container
    & {
        display:flex;
    }
    .fs-14 {
        font-size:14px;
    }
}
```

事实上，如果嵌套的子元素的类名由父元素的类名前缀构成，则`&`还可以代表子元素。例如页面有这样的结构:

```html
<div class="container">
    <span class="container-fs-14">字体大小为14px</span>
</div>
```

我们则可以写如下的样式为span元素设置样式。

```scss
.container {
    //&-fs-14指的就是container-fs-14
    &-fs-14 {
        font-size:14px;
    }
}
```

### 4.

