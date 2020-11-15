## 引言

消息提示框在实际应用场景当中比较常见，最常用的就是[element ui](https://element.eleme.cn/#/zh-CN/component/message)的消息提示框，我们通常都是直接使用它们,但是我们有没有尝试过去探究其实现原理，并自己动手实现呢？为了提升我们的个人能力和竞争力，我们可以尝试来实现这样一个消息提示框。

## 实现效果
 我们来查看一下最终实现效果，如下图所示:
 
![](https://user-gold-cdn.xitu.io/2020/5/10/171fc6a1c41abfa3?w=1093&h=507&f=png&s=15512)

## 准备工作

### 搭建基本的项目结构

我们创建一个message文件夹，然后创建一个index.html文件，以及message.js和message.css文件，如下所示:

![](https://user-gold-cdn.xitu.io/2020/5/10/171fc6d34214cb63?w=1248&h=778&f=png&s=416981)

### 对消息提示框进行布局

在html文件中，我们可以先来实现一个静态的消息提示框，代码如下:

```html
<div class="message">
    <p>这是一个提示框</p>
    <i class="message-close-btn">&times;</i>
</div>
```
然后再message.css我们写上基本的css代码:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
/* 消息提示框容器样式 */
.message {
    position: fixed;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    align-items: center;
    min-width: 300px;
    background-color: #edf2fc;
    border: 1px solid #edf2fc;
    padding: 16px 17px;
    top: 25px;
    border-radius: 6px;
    overflow: hidden;
    z-index: 1000;
}
/* 关闭按钮样式 */
.message > .message-close-btn {
    position: absolute;
    right: 15px;
    top: 50%;
    transform: translateY(-50%);
    color: #c0c0c4;
    font-size: 17px;
    cursor: pointer;
}
.message > .message-close-btn:hover,.message > .message-close-btn:active {
    color: #909399;
}
/* 消息提示框内容样式 */
.message p {
    line-height: 1;
    font-size:14px;
    color: #909399;
}
```

有四种提示框，以及让内容居中，我们不外乎是多加一个类名来写css样式，比如内容居中，我们只需要在html消息提示框容器元素加上一个类名，代码如下:

```html
<div class="message message-center">
    <p>这是一个提示框</p>
    <i class="message-close-btn">&times;</i>
</div> 
```
然后再css文件中加一段如下的css代码即可:

```css
/* 内容居中 */
.message.message-center {
    justify-content: center;
}
```

四种类型的提示框不外乎也是同样的原理，增加一个类名，然后改变的是背景色和字体色，所以html代码如下:

```html
<div class="message message-success">
    <p>这是一个成功提示框</p>
    <i class="message-close-btn">&times;</i>
</div>
<div class="message message-warning">
    <p>这是一个警告提示框</p>
    <i class="message-close-btn">&times;</i>
</div>
<div class="message message-error">
    <p>这是一个错误提示框</p>
    <i class="message-close-btn">&times;</i>
</div>
```
css代码如下:

```css
/* 成功提示框样式 */
.message.message-success {
    background-color: #e1f3d8;
    border-color:#e1f3d8;
}
.message.message-success p {
    color: #67c23a;
}
/* 警告提示框样式 */
.message.message-warning{
    background-color: #fdfce6;
    border-color: #fdfce6;
}
.message.message-warning p{
    color: #e6a23c;
}
/* 错误提示框样式 */
.message.message-error {
    background-color: #fef0f0;
    border-color: #fef0f0;
}
.message.message-error p {
    color: #f56c6c;
}
```

这样一来，准备工作就完成了，接下来就是我们的重头戏，JavaScript代码，尝试将如上代码注释掉。

## 动态实现创建提示框

### 定义四种类型的消息提示框

我们通过定义一个对象来表示消息提示框的类型，如下所示:

```js
// 消息提示框的四种类型
let typeMap = {
    info: "info",
    warning: "warning",
    success: "success",
    error: "error"
}
```

### 添加默认配置项

我们分析一下需要传入的配置项有内容(content),关闭提示框时间(closeTime),是否显示关闭提示框按钮(showClose),内容居中(center)以及消息提示框类型(type)。所以定义配置项如下:

```js
// 提示框的默认配置项
let messageOption = {
    type: "info",
    closeTime: 600,
    center: false,
    showClose: false,
    content: "默认内容"
}
```

### 创建一个消息提示框类

我们通过面向对象的编程思维将消息提示框当做是一个类对象，所以我们只需要创建一个类。虽然可以使用es6的class语法来创建，但是为了方便，我们使用构造函数来实现。创建一个构造函数Message，如下所示:

```js
function Message(option) {
    //这里做了一次初始化
    this.init(option);
}
```
### 初始化

创建了消息提示框构造函数之后，我们需要传入配置项，并且我们在函数里做了初始化操作，接下来我们来实现初始化的操作。

```js
Message.prototype.init = function (option) {
    //这里创建了提示框元素，并将整个提示框容器元素添加到页面中
    document.body.appendChild(this.create(option));
    //这里设置提示框的top
    this.setTop(document.querySelectorAll('.message'));
    //判断如果传入的closeTime大于0，则默认关闭提示框
    if (option.closeTime > 0) {
        this.close(option.container, option.closeTime);
    }
    //点击关闭按钮关闭提示框
    if (option.close) {
        option.close.onclick = (e) => {
            this.close(e.currentTarget.parentElement, 0);
        }
    }
}
```

### 创建提示框

在前面的初始化操作中，我们做了几个功能，首先创建提示框容器元素，并将提示框容器元素添加到页面bod中。我们还做了动态计算提示框的top以及判断传入的默认关闭时间来关闭提示框，点击关闭按钮关闭提示框。我们来看创建提示框的方法，即create方法的编写操作。如下:

```js
Message.prototype.create = function (option) {
    //这里做了一个判断，表示如果设置showClose为false即不显示关闭按钮并且closeTime也为0，即无自动关闭提示框，我们就显示关闭按钮
    if(!option.showClose && option.closeTime <=0)option.showClose = true;
    //创建容器元素
    let element = document.createElement('div');
    //设置类名
    element.className = `message message-${option.type}`;
    if (option.center) element.classList.add('message-center');
    //创建关闭按钮元素以及设置类名和内容
    let closeBtn = document.createElement('i');
    closeBtn.className = 'message-close-btn';
    closeBtn.innerHTML = '&times;';
    //创建内容元素
    let contentElement = document.createElement('p');
    contentElement.innerHTML = option.content;
    //判断如果显示关闭按钮，则将关闭按钮元素添加到提示框容器元素中
    if (closeBtn && option.showClose) element.appendChild(closeBtn);
    //将内容元素添加到提示框容器中
    element.appendChild(contentElement);
    //在配置项对象中存储提示框容器元素以及关闭按钮元素
    option.container = element;
    option.close = closeBtn;
    //返回提示框容器元素
    return element;
}
```

### 实现提示框的关闭

我们可以看到，我们创建了一个close方法，并传入提示框容器元素，来实现关闭一个提示框，接下来我们来实现这个关闭方法。如下所示:

```js
Message.prototype.close = function (messageElement, time) {
    //根据传入的时间来延迟关闭，实际上也就是移除元素
    setTimeout(() => {
        //判断如果传入了提示框容器元素，并且分两种情况，如果是多个提示框容器元素则循环遍历删除，如果是单个提示框容器元素，则直接删除
        if (messageElement && messageElement.length) {
            for (let i = 0; i < messageElement.length; i++) {
                if (messageElement[i].parentElement) {
                    messageElement[i].parentElement.removeChild(messageElement[i]);
                }
            }
        } else if (messageElement) {
            if (messageElement.parentElement) {
                messageElement.parentElement.removeChild(messageElement);
            }
        }
        //关闭了提示框容器元素之后，我们重新设置提示框的top值
        this.setTop(document.querySelectorAll('.message'));
    }, time * 10);
}
```

### 实现提示框的动态top

最后我们需要实现的是动态计算消息提示框的top，然后不让消息提示框重叠在一起。代码如下:

```js
Message.prototype.setTop = function (messageElement) {
    //这里做一个判断的原因就是当点击页面中最后一个提示框的时候，会重新调用一次，这时获取不到提示框容器元素，所以就不执行后续的设置top
    if (!messageElement || !messageElement.length) return;
    //由于每个提示框的高度一样，所以我们只需获取第一个提示框元素的高度即可
    const height = messageElement[0].offsetHeight;
    for (let i = 0; i < messageElement.length; i++) {
        //每个提示框的top由一个固定值加上它的高度，并且我们要乘以它的一个索引值
        messageElement[i].style.top = (25 * (i + 1) + height * i) + 'px';
    }
}
```

## 最后，实现封装，让我们可以如同调用element ui那样来调用

我们想要这样调用`$message()`或者`$message.info()`，那么我们可以实现如下:

```js
let $message = {};
window['$message'] = $message = function (option) {
    let newMessageOption = null;
    if (typeof option === 'string') {
        newMessageOption = Object.assign(messageOption, { content: option });
    } else if (typeof option === 'object' && !!option) {
        newMessageOption = Object.assign(messageOption, option);
    }
    return new Message(newMessageOption);
}
for (let key in typeMap) {
    window['$message'][key] = function (option) {
        let newMessageOption = null;
        if (typeof option === 'string') {
            newMessageOption = Object.assign(messageOption, { content: option,type:typeMap[key] });
        } else if (typeof option === 'object' && !!option) {
            newMessageOption = Object.assign(JSON.parse(JSON.stringify(messageOption)),option,{ type:typeMap[key] });
        }
        return new Message(newMessageOption);
    }
}
```

整个逻辑也十分简单，无非就是判断传入的配置项，然后进行合并，并传入实例化的Message中。

如此一来，我们就完成了一个消息提示框。

## 录制视频

如果以上分析还不懂的话，可以查看我录制的一个视频:

[![从零开始实现一个消息提示框](https://user-gold-cdn.xitu.io/2020/5/10/171fc8ce6c4f382f?w=200&h=112&f=jpeg&s=6147)](https://v.youku.com/v_show/id_XNDY2Njg4Mjk0NA==.html)

