## 实现一个clickOutSide 指令

`clickOutside`，顾名思义，就是当点击一个元素之外的区域所触发的事件，我们可以借助`vue.js`的自定义指令来实现这样的一个功能。该指令的场景应用比较广泛，比如一个模态框，点击内容之外的遮罩层区域关闭模态框，再比如自己造的下拉组件或者提示组件等等都会用到这个指令，著名`vue ui`框架——`element ui`框架也多次运用到该指令。

### 判断vue是否运行在服务器环境上

首先我们需要知道`vue.js`这个框架不仅可以运行在`web浏览器`上，还可以运行在服务器环境中，`vue.js`提供了一个属性给我们作为判断，它就是绑定在原型上的`$isServer`属性，这个属性的值为一个布尔值，即如果为`true`则表示该`vue`实例运行在服务器上，反之则不是运行在服务器上，我们要实现一个点击一个元素之外的区域所触发的场景很明显不属于服务器环境，所以我们需要定义这个属性用作判断。

```js
    //判断vue实例是否运行在服务器环境上
    const isServer = Vue.prototype.$isServer;
```
