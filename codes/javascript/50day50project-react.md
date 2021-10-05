# 1.Expanding Cards

效果如图所示:

![1.png](https://eveningwater.github.io/codes/javascript/50project-images/1.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/6/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/6/)

> 学到了什么?

1. React的函数组件中建立数据通信，我们通常使用`useState`方法。它的使用方式采用数组解构的方式，如下:

```js
const [state,setState] = React.useState(false);//useState方法的参数可以是任意的JavaScript数据类型
```

解构的第一个参数是我们定义并且访问的数据状态，第二个参数则是当我们需要变动数据状态时所调用的方法，其作用类似类组件中的`this.setState`。

更详细的使用方式参考文档 [useState API](https://reactjs.org/docs/hooks-state.html)。

2.与类组件类似的钩子函数，或者也可以理解为是函数组件的生命周期`useEffect`。它接收一个副作用函数`effect`作为参数，如下所示:

```js
useEffect(() => {});//里面的箭头函数即副作用函数
```

以上示例只是做了一个简单的更换文档标题，事实上在这个副作用函数中，我们可以做很多事情，详情参考文档 [useEffect API](https://reactjs.org/docs/hooks-effect.html)。

3.React内部有自己的一套事件机制，我们称之为[合成事件](https://reactjs.org/docs/events.html)。它与正常的`JavaScript`绑定事件很类似，但是它将事件命名方式采用了驼峰式写法，并且它也对事件对象做了一层包装，其名为`SyntheticEvent`。注意，这是一种跨浏览器端的包装器，我们如果想要使用原生的事件对象，可以使用`nativeEvent`属性，这在后面的示例中可能会涉及到。比如我们以上的事件绑定:

```js
onClick={ onChangeHandler.bind(this,index) }
```

注意这里，我们通常需要调用`bind`方法来将`this`绑定到该组件实例上，为什么要使用`bind`方法来绑定`this`呢，这是因为绑定事件的回调函数(如这里的:`onChangeHandler`),它是作为一个中间变量的，在调用该回调函数的时候`this`指向会丢失，所以需要显示的绑定`this`,这也是受`JavaScript`自身特性所限制的。详情可参考[React绑定this的原因](https://blog.csdn.net/qq_34829447/article/details/81705977)中的解释。与之类似的是在类组件中绑定合成事件，我们也一样需要显示的绑定`this`指向。

4.`map`方法的原理。`map`方法迭代一个数组，然后根据返回值来对数组项做处理，并返回处理后的数组,请注意该方法不会改变原数组。比如:

```js
[1,2,3].map(i => i <= 1);//返回[1]
```

`jsx`中渲染列表也正是利用了`map`方法的这一特性，并且我们需要注意的是渲染列表的时候必须要指定一个`key`,这是为了方便`DIFF`算法更好的比对虚拟DOM。

5.React中给标签添加类名，我们是写成`className`的，这是因为`class`被`JavaScript`当做关键字。而如果我们需要动态绑定类名，可以看到，我们使用了模板字符串，在这里更像是写JavaScript，比如我们可以利用三元表达式做判断。

6.React中给标签绑定style样式，我们通常可以绑定一个对象，在React中，我们绑定动态数据就是写一对`{}`花括号，然后style里面的样式我们通常声明成对象来表示，比如:

```js
{
    background:"#fff"
}
```

这代表它是一个样式对象，然后React会在内部去将样式对象转换成样式字符串，然后添加到DOM的style对象中。

# 2.Progress Steps


效果如图所示:

![2.png](https://eveningwater.github.io/codes/javascript/50project-images/2.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/7/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/7/)


> 学到了什么?

与第一个示例所用到的知识点很类似，相关的不必做说明。接下来我们来看不一样的。

1. 父组件向子组件传递数据，我们通常都是使用`props`。可以看到以上的示例，我们暴露了4个props，即:

```js
width
stepItems
currentActive 
onClickItem
```

width就是设置步骤条的容器宽度，这个没什么可说的,stepItems就是步骤条的子组件，是一个数组，也可以在数组项中写jsx。而`currentActive`则是传入的当前是哪一步，是一个索引值，通常应该是数值。至于`onClickItem`则是子组件暴露给父组件的方法。

2. 类组件的生命周期。在这个类组件当中，我们使用到了`constructor,componentDidMount,render`的生命周期钩子函数。我们可以根据语义来推测，当一个类组件被初始化时所经历的生命周期钩子函数执行顺序一定是`constructor => render => componentDidMount`。从语义上来将`constructor`是一个构造函数，用于初始化状态，然后初始化完成之后，我们就会渲染组件，然后才是准备挂载组件。

额外的，我们扩展一下，根据[文档](https://reactjs.org/docs/react-component.html)说明，我们可以知道详细的生命周期。React组件生命周期包含3个阶段:

`挂载 => 更新 => 卸载`

在组件挂载之前执行的有:

```
constructor => static getDerivedStateFromProps => render => componentWillMount(即将过时) => componentDidMount
``` 
在组件state变更之后,也就是更新之后，执行的有:

```
static getDerivedStateFromProps => shouldComponentUpdate => render => getSnapshotBeforeUpdate => componentWillReceiveProps(即将过时) => componentWillUpdate(即将过时) => componentDidUpdate
```

在组件卸载之后，执行的有:

```
componentWillUnmount
```

另还有错误处理的生命周期，也就是在渲染过程，生命周期，或子组件的构造函数发生错误时，会执行的钩子函数:

```
static getDerivedFromStateError => componentDidCatch
```

这里面有些生命周期我们并没有用到，有些则是几乎涵盖了我们后续的所有示例，所以我们一定要牢记组件的生命周期的顺序。

但是就这个示例而言，我们学会的应该是如何封装一个组件。

# 3.Rotating Navigation Animation

效果如图所示:

![3.png](https://eveningwater.github.io/codes/javascript/50project-images/3.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/8/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/8/)

> 学到了什么?

一些相同同前面示例相同的知识点自不必说，我们来看不一样的知识点。

1.模块组合导出

```js
//components目录下的index.js文件
export { default as Content } from './Content';
export { default as LeftMenu } from './LeftMenu';
export { default as NavList } from "./NavList";
```

可以看到，我们可以将组件如上面那样导出，然后我们就可以单独引入一个js文件，再引入相关的组件即可使用。如下:

```js
import { Content, NavList, LeftMenu } from './components/index';
```

2.react组件如何渲染html字符串

react提供了一个`dangerouslySetInnerHTML`属性，这个属性的属性值是一个以`__html`作为属性，值是`html`字符串的对象，然后，我们就可以将`html`字符串渲染成真实的`DOM`元素。如下所示:

```js
<p dangerouslySetInnerHTML={{ __html:"<span>测试代码</span>" }}></p>
//<p><span>测试代码</span></p>
```

# 4.hidden-search-widget

效果如图所示:

![4.png](https://eveningwater.github.io/codes/javascript/50project-images/4.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/9/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/9/)

> 学到了什么?

本示例同样的与前面的示例有一样的知识点，相关的不会做说明，只会做不同的知识点介绍，后续的同理。

1.判断数据的类型。利用对象原型链上的`toString`方法，我们可以得到一个字符串值，这个字符串值的格式类似`[object Object]`这样，也就是说我们可以通过这个字符串值来判断一个数据的类型。例如:

```js
const getType = v => Object.prototype.toString.call(v).slice(8,-1).toLowerCase();
const isArray = v => getType(v) === "array";
isArray([1,2,3]);//true
isArray("");//false
```

这里我们应该知道`Object.prototype.toString.call(v)`返回的就是类似`[object Object]`这样的值。所以我们通过截取开始索引为8,结束索引为该字符串长度减1，也就是这里的-1,我们就可以获取到第二个值`Object`,然后再调用`toLowerCase()`方法来将全部字母转换成小写，然后，我们就可以知道数据的类型了。比如这里的判断是否是数组，那么只需要判断该值是否等于"array"即可。

2.React.createRef API

有时候，我们恰恰需要操作一些原生DOM元素的API，例如这个示例的输入框的关注焦点事件。这时候这个API就有了用武之地，我们相当于使用这个API创建一个与DOM元素通信的桥梁，通过这个访问这个API的实例的current属性，我们就可以访问到相应的DOM元素。

更详细的可以参考文档[createRef API](https://reactjs.org/docs/react-api.html#reactcreateref)。

3.如何封装一个input组件。

这个示例也教会了我们如何封装一个input组件。

# 5.Blurry Loading

效果如图所示:

![5.png](https://eveningwater.github.io/codes/javascript/50project-images/5.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/10/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/10/)

> 学到了什么?

1. 在`componentDidMount`生命周期中创建定时器以及在`componentWillUnmount`中清除定时器。

2. 类组件中的`this.setState`更新状态。该方法接收2个参数,第一个参数则是我们的react状态，它可以是一个函数，也可以是一个对象。第二个参数则是一个回调函数。谈到这里，可能就会提到一个问题，那就是setState到底是异步还是同步? 答案如下:


答:react中的setState在合成事件和钩子函数中是"异步"的，而在原生事件和setTimeout中则是同步的。

之所以是"异步"，并不代表在react内部中是"异步"代码实现的，事实上，它仍然是同步执行的一个过程。

只是合成事件和钩子函数的调用顺序在更新之前，导致在合成函数和钩子函数中没法立即拿到更新后的值，所以就形成了所谓的"异步"。

事实上，我们可以通过制定第二个参数即callback(回调函数)来获取到更新后的值。

react.js对setState的源码实现也不是很复杂，它将传入的参数作为值添加到`updater`也就是更新器的一个定义好的队列（即:enqueueSetState）中。

react中的批量更新优化也是建立在合成事件和钩子函数(也就是"异步")之上的，在原生事件和setTimeout中则不会进行批量更新。

比如在"异步"中对同一个值进行多次setState,依据批量更新则会对其进行策略覆盖，而如果是对不同的多个值setState,则会利用批量更新策略对其进行合并然后批量更新。

更详细的可以参考文档[ setState ](https://reactjs.org/docs/faq-state.html#what-does-setstate-do)。

除此之外，这里也有一个非常重要的工具函数(这在js的实现中也提及过)，如下所示:

```js
const scale = (n,inMin,inMax,outerMin,outerMax) => (n - inMin) * (outerMax - outerMin) / (inMax - inMin) + outerMin;
```

这个工具函数的作用就是将一个范围数字映射到另一个数字范围。比方说，将`1 ~ 100`的数字范围映射到`0 ~ 1`之间。
[详情](https://stackoverflow.com/questions/10756313/javascript-jquery-map-a-range-of-numbers-to-another-range-of-numbers)。

# 6.Scroll Animation

效果如图所示:

![6.png](https://eveningwater.github.io/codes/javascript/50project-images/6.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/11)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/11)

> 学到了什么?

1.动态组件

我们通过props传递一个值用来确定组件名。这里以`Title`组件为例，如下所示:

```js
import React from "react";
const TitleComponent = (props) => {
    let TagName = `h${ props.level }`;
    return (
        <React.Fragment>
            <TagName>{ props.children }</TagName>
        </React.Fragment>
    )
}
export default TitleComponent;
```

虽然核心代码十分简单，这里需要注意，React组件需要首字母大写，这算是一个约定的规则，其次我们通过props传递一个level用来确定我们使用的是`h1~h6`哪个来做标签，事实上这里我们应该对level做一个限制，只允许值为`1~6`。

2.Fragment元素

这个元素类似于一个占位符节点，我们知道，当两个元素并列在一个React组件中，是不被允许的，React组件需要提供一个根节点，但有时候，我们不需要一个实际的元素作为根节点包裹它们，所以我们就可以使用`Fragment`元素来包裹它们。该元素还有一个简写`<></>`,事实上在Vue2.x中也有这个限制，这是受虚拟DOM DIFF算法所限制的。

更详细的可以见文档[Fragment](https://reactjs.org/docs/fragments.html#gatsby-focus-wrapper)。

3.函数防抖

```js
export const throttle = (fn, time = 1000) => {
    let done = false;
    return (...args) => {
        if (!done) {
            fn.call(this, ...args);
            done = true;
        }
        setTimeout(() => {
            done = false;
        }, time)
    }
}
```

函数防抖与节流可参考[ 这篇文章 ](https://juejin.cn/post/6844903669389885453)。

4.监听滚动事件，事实上这里的实现原理同JavaScript实现版本一致，只不过稍微转换一下思维即可。


# 7. Split Landing Page

效果如图所示:

![7.png](https://eveningwater.github.io/codes/javascript/50project-images/7.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/12)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/12)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 8.Form Wave

效果如图所示:

![8.png](https://eveningwater.github.io/codes/javascript/50project-images/8.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/13)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/13)

> 学到了什么?

1. setState更新对象，如果state是一个对象，我们有2种方式来更新。
    1.1 利用Object.assign方法来更新。
    1.2  直接覆盖整个对象。

以上2种方式伪代码如下:

 ```js
//  第1种方式
const loginInfo = Object.assign({},this.state.loginInfo,{
    [key]:updateValue
})
this.setState({
    loginInfo
});
// 第2种方式
let { loginInfo } = this.state;
loginInfo[key] = updateValue;
this.setState({ loginInfo });
```

# 9.Sound Board

效果如图所示:

![9.png](https://eveningwater.github.io/codes/javascript/50project-images/9.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/14)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/14)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 10. Dad Jokes

效果如图所示:


![10.png](https://eveningwater.github.io/codes/javascript/50project-images/10.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/15)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/15)

> 学到了什么?

1. Suspense组件的使用。该组件可以指定一个加载指示器组件，用来实现组件的懒加载。更详细的文档见[ Suspense ](https://reactjs.org/docs/react-api.html#reactsuspense)。

2. 接口请求通常都是在componentDidMount钩子函数中完成的。由于不能直接在该钩子函数中更改状态(react.js会给出一个警告)。所以我们需要让接口请求变成异步。

# 11. Event Keycodes

效果如图所示:

![11.png](https://eveningwater.github.io/codes/javascript/50project-images/11.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/16)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/16)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 12. Faq Collapse

效果如图所示:

![12.png](https://eveningwater.github.io/codes/javascript/50project-images/12.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/17)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/17)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 13. Random Choice Picker

效果如图所示:


![13.png](https://eveningwater.github.io/codes/javascript/50project-images/13.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/18)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/18)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 14. Animated Navigation

效果如图所示:


![14.png](https://eveningwater.github.io/codes/javascript/50project-images/14.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/19)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/19)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 15. Incrementing Counter

效果如图所示:


![15.png](https://eveningwater.github.io/codes/javascript/50project-images/15.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/20)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/20)

> 学到了什么?

1. react.js如何更新数组的某一项？在这里我是更新整个数组的，或许这不是一种好的方式。也希望有大佬能提供思路。我的代码如下:

```js
startCounter() {
    const counterList = [...this.state.counterList];
    // https://stackoverflow.com/questions/29537299/react-how-to-update-state-item1-in-state-using-setstate
    // https://stackoverflow.com/questions/26253351/correct-modification-of-state-arrays-in-react-js
    counterList.forEach(counter => {
      const updateCounter = () => {
          const value = counter.value;
          const increment = value / 100;
          if (counter.initValue < value) {
            counter.initValue = Math.ceil(counter.initValue + increment);
            // use setTimeout or setInterval ?
            counter.timer = setTimeout(updateCounter, 60);
          } else {
            counter.initValue = value;
            clearTimeout(counter.timer);
          }
          // update the array,maybe it's not a best way,is there any other way?
          this.setState({ counterList });
      }
      updateCounter();
    })
}
```

# 16. Drink Water

效果如图所示:

![16.png](https://eveningwater.github.io/codes/javascript/50project-images/16.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/21)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/21)

> 学到了什么?

1. 这里踩了一个坑，如果使用new Array().fill()来初始化状态，会导致意想不到的渲染效果。所以这里直接初始化了所有的数组项。

[ 详见源码 ](https://github.com/eveningwater/my-web-projects/blob/master/react/21/src/App.js)。

# 17. movie-app

效果如图所示:

![17.png](https://eveningwater.github.io/codes/javascript/50project-images/17.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/22)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/22)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

> PS:这个示例效果由于接口访问受限，需要你懂的访问。

# 18. background-slider

效果如图所示:

![18.png](https://eveningwater.github.io/codes/javascript/50project-images/18.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/23)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/23)

> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 19. theme-clock

效果如图所示:

![19.png](https://eveningwater.github.io/codes/javascript/50project-images/19.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/24)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/24)

> 学到了什么?

1. 中英文切换是通过定义一个对象来完成的。其它的没什么好说的，都是前面提及过的知识点。

> PS:本示例也用到了与示例5一样的工具函数`scale`函数

# 20. button-ripple-effect

效果如图所示:

![20.png](https://eveningwater.github.io/codes/javascript/50project-images/20.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/25)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/25)

> 学到了什么?

1. 可以通过调用函数来渲染一个组件。

# 21. drawing-app

效果如图所示:

![21.png](https://eveningwater.github.io/codes/javascript/50project-images/21.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/26)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/26)

> 学到了什么?

1. 在react.js中使用`ew-color-picker`。
2. 这里踩了一个坑，也就是说必须要设置线条的样式。

```js
this.ctx.lineCap = "round";
```
否则线条的样式不对劲，虽然我也没有搞清楚这里面的原因。毕竟js版本的实现也没有需要显示的设置这个线条的样式。

# 22. drag-n-drop

效果如图所示:

![22.png](https://eveningwater.github.io/codes/javascript/50project-images/22.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/27)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/27)

> 学到了什么?

1. 这里也踩了一个坑，[ 详见源码注释 ](https://github.com/eveningwater/my-web-projects/blob/master/react/27/src/App.js)。

# 23. content-placeholder

效果如图所示:

![23.png](https://eveningwater.github.io/codes/javascript/50project-images/23.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/28)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/28)

> 学到了什么?

1. 一些判断react组件的工具函数。如下:

```js
import React from "react";
export function isString(value){
    return typeof value === "string";
}
export function isClassComponent(component) {
    return typeof component === 'function' && !!component.prototype.isReactComponent;
}

export function isFunctionComponent(component) {
    return typeof component === 'function' && String(component).indexOf('return React.createElement') > -1;
}

export function isReactComponent(component) {
    return isClassComponent(component) || isFunctionComponent(component);
}

export function isElement(element) {
    return React.isValidElement(element);
}

export function isDOMTypeElement(element) {
    return isElement(element) && typeof element.type === 'string';
}

export function isCompositeTypeElement(element) {
    return isElement(element) && typeof element.type === 'function';
}
```

2. 如何封装一个卡片组件。

# 24. kinetic-loader

效果如图所示:

![24.png](https://eveningwater.github.io/codes/javascript/50project-images/24.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/29)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/29)


> 学到了什么?

这个示例涉及到的知识点前面的示例都提及过，所以这里不必赘述。

# 25. sticky-navbar

效果如图所示:

![25.png](https://eveningwater.github.io/codes/javascript/50project-images/25.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/30)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/30)


> 学到了什么?

这个示例涉及到的知识点除了一个工具函数，其它的知识点前面的示例都提及过，所以这里不必赘述。

```js
export function marked(template){
    return template.replace(/.+?[\s]/g,v => `<p>${v}</p>`);
}
```

这个工具函数的作用就是匹配以空格结束的任意字符，然后替换成p标签包裹这些内容。

> PS:这里也做了移动端的布局。

# 26. double-slider

效果如图所示:

![26.png](https://eveningwater.github.io/codes/javascript/50project-images/26.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/31)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/31)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 27. toast-notification

效果如图所示:

![27.png](https://eveningwater.github.io/codes/javascript/50project-images/27.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/32)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/32)


> 学到了什么?

1. 可以利用`ReactDOM.render`来封装一个函数调用的`toast`组件。

2. `ReactDOM.unmountComponentAtNode API`的用法这个方法会将从DOM中卸载组件，包括事件处理器和state。详见[ 文档 ](https://reactjs.org/docs/react-dom.html#unmountcomponentatnode)。

3. getDerivedStateFromProps 静态函数。详见 [文档](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)。

# 28. github-profiles

效果如图所示:

![28.png](https://eveningwater.github.io/codes/javascript/50project-images/28.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/33)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/33)

> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 29. double-click-heart

效果如图所示:

![29.png](https://eveningwater.github.io/codes/javascript/50project-images/29.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/34)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/34)


> 学到了什么?

1. 从合成事件对象中读取原生的事件对象。即`nativeEvent`属性。

# 30. auto-text-effect

效果如图所示:

![30.png](https://eveningwater.github.io/codes/javascript/50project-images/30.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/35)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/35)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 31. password generator

效果如图所示:


![31.png](https://eveningwater.github.io/codes/javascript/50project-images/31.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/36)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/36)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 32. good-cheap-fast


效果如图所示:

![32.png](https://eveningwater.github.io/codes/javascript/50project-images/32.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/37)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/37)


> 学到了什么?

1. 如何封装一个switch组件，即开关小组件。

# 33. notes-app


效果如图所示:

![33.png](https://eveningwater.github.io/codes/javascript/50project-images/33.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/38)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/38)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。


# 34. animated-countdown


效果如图所示:


![34.png](https://eveningwater.github.io/codes/javascript/50project-images/34.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/39)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/39)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 35. image-carousel


效果如图所示:


![35.png](https://eveningwater.github.io/codes/javascript/50project-images/35.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/40)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/40)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 36. hover board

效果如图所示:

![36.png](https://eveningwater.github.io/codes/javascript/50project-images/36.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/41)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/41)


> 学到了什么?

1. react.js合成事件中的setState是同步的，所以这里使用原生的监听事件来实现。[ 详见源码 ](https://github.com/eveningwater/my-web-projects/blob/master/react/41/src/App.js)。

# 37. pokedex


效果如图所示:


![37.png](https://eveningwater.github.io/codes/javascript/50project-images/37.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/42)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/42)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 38. mobile-tab-navigation


效果如图所示:

![38.png](https://eveningwater.github.io/codes/javascript/50project-images/38.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/43)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/43)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 39. password-strength-background


效果如图所示:

![39.png](https://eveningwater.github.io/codes/javascript/50project-images/39.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/44)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/44)


> 学到了什么?

1. 照着[ 文档 ](https://tailwindcss.com/docs/guides/create-react-app)一步步将`tailwindcss`添加到`create-react-app`脚手架中。

# 40. 3D-background-boxes


效果如图所示:

![40.png](https://eveningwater.github.io/codes/javascript/50project-images/40.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/45)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/45)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 41. Verify Your Account

效果如图所示:

![41.png](https://eveningwater.github.io/codes/javascript/50project-images/41.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/46)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/46)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 42. live-user-filter


效果如图所示:

![42.png](https://eveningwater.github.io/codes/javascript/50project-images/42.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/47)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/47)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 43. feedback-ui-design


效果如图所示:

![43.png](https://eveningwater.github.io/codes/javascript/50project-images/43.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/48)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/48)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 44. custom-range-slider

效果如图所示:

![44.png](https://eveningwater.github.io/codes/javascript/50project-images/44.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/49)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/49)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 45. netflix-mobile-navigation


效果如图所示:

![45.png](https://eveningwater.github.io/codes/javascript/50project-images/45.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/50)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/50)


> 学到了什么?

1. 为如何实现一个递归组件提供了思路。

# 46. quiz-app


效果如图所示:

![46.png](https://eveningwater.github.io/codes/javascript/50project-images/46.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/51)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/51)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 47. testimonial-box-switcher


效果如图所示:

![47.png](https://eveningwater.github.io/codes/javascript/50project-images/47.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/52)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/52)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 48. random-image-feed


效果如图所示:

![48.png](https://eveningwater.github.io/codes/javascript/50project-images/48.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/53)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/53)


> 学到了什么?

1. 实现一个简易版本的预览图片。

# 49. todo-list


效果如图所示:

![49.png](https://eveningwater.github.io/codes/javascript/50project-images/49.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/54)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/54)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

# 50. insect-catch-game


效果如图所示:

![50.png](https://eveningwater.github.io/codes/javascript/50project-images/50.png)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/react/55/)
* [在线示例](https://www.eveningwater.com/my-web-projects/react/55/)


> 学到了什么?

这个示例涉及到的知识点，前面的示例都提及过，所以这里不必赘述。

> 特别说明:这个示例算是一个比较综合的示例了。




