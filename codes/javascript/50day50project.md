# 1.Expanding Cards

效果如图所示:

![1.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/284576a9dece49eeb3011432276362d6~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/59)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/59)

> 知识点总结:

1. CSS:弹性盒子布局中的`flex`属性:让所有弹性盒模型对象的子元素都有相同的长度，且忽略它们内部的内容。
2. JavaScript:利用`[].filter.call()`方法可快速实现简单的选项卡切换。如上述示例源码:

```js
const panelItems = document.querySelectorAll(".container > .panel");
panelItems.forEach(item => {
    item.addEventListener('click',() => {
        [].filter.call(item.parentElement.children,el => el !== item).forEach(el => el.classList.remove('active'));
        item.classList.add('active')
    });
});

```

# 2.Progress Steps

效果如图所示:

![2.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7cfc00bec03b4588a204dd901c006f84~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/60)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/60)

> 知识点总结:

1. CSS:`CSS`变量的用法以及弹性盒子垂直水平居中外加伪元素的用法。
2. JavaScript:计算进度条的宽度，类名的操作。如上述示例部分源码如下:

```js
function handleClass(el){
    let methods = {
        addClass,
        removeClass
    };
    function addClass(c){
        el.classList.add(c);
        return methods;
    };
    function removeClass(c){
        el.classList.remove(c);
        return methods;
    }
    return methods
}
```
# 3.Rotating Navigation Animation

效果如图所示:


![3.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56cdf0c1582b426784d213c330e4b8c8~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/61)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/61)

> 知识点总结:

1. CSS:CSS弹性盒子布局加`rotate`动画。
2. JavaScript:添加和移除类名的操作。如上述示例部分源码如下:

```js
const addClass = (el,className) => el.classList.add(className);
const removeClass = (el,className) => el.classList.remove(className);
```

# 4.hidden-search-widget

效果如图所示:


![4.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac9c30dc348f4aa1bd3781199de59eea~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/62)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/62)

> 知识点总结:

1. CSS:CSS过渡动画 + 宽度的更改 + `input`的`placeholder`样式。
2. JavaScript:添加和移除类名的操作。如上述示例部分源码如下:

```css
.search.active > .input {
    width: 240px;
}
.search.active > .search-btn {
    transform: translateX(238px);
}
.search > .search-btn,.search > .input {
    border: none;
    width: 45px;
    height: 45px;
    outline: none;
    transition: all .3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    border-radius: 8px;
}
```

```js
searchBtn.addEventListener('click',() => {
    searchContainer.classList.toggle('active');
    searchInput.focus();
})
```

# 5.Blurry Loading

效果如图所示:


![5.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3734818e0d9844bdb98b8e975945ed99~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/63)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/63)

> 知识点总结:

1. CSS:`CSS filter`属性的用法;。
2. JavaScript:定时器加动态设置样式。如上述示例部分源码如下:


```js
let load = 0;
let timer = null;
let blurryLoadingHandler = function(){
    load++;
    if(load > 99){
        clearTimeout(timer)
    }else{
        timer = setTimeout(blurryLoadingHandler,20);
    }
    text.textContent = `页面加载${ load }%`;
    text.style.opacity = scale(load,0,100,1,0);
    bg.style.filter = `blur(${scale(load,0,100,20,0)}px)`;
}
blurryLoadingHandler();
```

这里有一个非常重要的工具函数(后续有好几个示例都用到了这个工具函数)，如下所示:

```js
const scale = (n,inMin,inMax,outerMin,outerMax) => (n - inMin) * (outerMax - outerMin) / (inMax - inMin) + outerMin;
```

这个工具函数的作用就是将一个范围数字映射到另一个数字范围。比方说，将`1 ~ 100`的数字范围映射到`0 ~ 1`之间。
[详情](https://stackoverflow.com/questions/10756313/javascript-jquery-map-a-range-of-numbers-to-another-range-of-numbers)。

# 6.Scroll Animation
效果如图所示:

![6.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dec05e2090714cf984166342c683b86b~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/64)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/64)

> 知识点总结:

1. CSS:`CSS`位移动画。
2. JavaScript:动态创建元素+元素滚动事件监听+视图可见区域的判断 + 防抖函数。如上述示例部分源码如下:


```js
function debounce(fn,time = 100){
    let timer = null;
    return function(){
        if(timer)clearTimeout(timer);
        timer = setTimeout(fn,time);
    }
}
let triggerBottom = window.innerHeight / 5 * 4;
boxElements.forEach((box,index) => {
   const top = box.getBoundingClientRect().top;
   if(top <= triggerBottom){
       box.classList.add('show');
   }else{
      box.classList.remove('show');
   }
})
```

# 7. Split Landing Page

效果如图所示:


![7.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/726230c209504ea39fae7b6092e7d3a3~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/65)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/65)

> 知识点总结:

1. CSS:`CSS`过渡特效 + 弹性盒子垂直水平居中 + `CSS`定位 + 宽度的更改。
2. JavaScript:鼠标悬浮事件 + 类名的操作。如上述示例部分源码如下:


```js
HTMLElement.prototype.addClass = function(className) {
    this.classList.add(className);
};
HTMLElement.prototype.removeClass = function(className){
    this.classList.remove(className);
}
const on = (el,type,handler,useCapture = false) => {
    if(el && type && handler) {
        el.addEventListener(type,handler,useCapture);
    }
}
on(leftSplit,'mouseenter',() => container.addClass('hover-left'));
on(leftSplit,'mouseleave',() => container.removeClass('hover-left'));
on(rightSplit,'mouseenter',() => container.addClass('hover-right'));
on(rightSplit,'mouseleave',() => container.removeClass('hover-right'));
```

从这个示例，我也知道了`mouseenter、mouseleave`和`mouseover、mouseout`的区别，总结如下:

> 1. enter只触发1次，只有等到鼠标离开了目标元素之后再进入才会继续触发，同理leave也是如此理解。而over与out就是不断的触发。
> 2. enter进入子元素，通过e.target也无法区分是移入/移出子元素还是父元素，而over与out则可以区分。

# 8.Form Wave

效果如图所示:

![8.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/47a8afb5441b462b9678c4a5dd4bd721~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/66)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/66)

> 知识点总结:

1. CSS:`CSS`渐变 + 弹性盒子垂直水平居中 + `CSS`过渡动画 + `CSS`位移变换 + 关注焦点伪类选择器与同级元素选择器的用法。
2. JavaScript:字符串替换成标签 + 动态创建元素。如上述示例部分源码如下:


```js
const createLetter = v => v.split("").map((letter,idx) => `<span style="transition-delay:${ idx * 50 }ms">${ letter }</span>`).join("");
```

# 9.Sound Board

效果如图所示:


![9.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e3a26c5fd6914806a22c6863fadb451f~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/67)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/67)

> 知识点总结:

1. CSS:`CSS`渐变 + 弹性盒子垂直水平居中 + 基本样式。
2. JavaScript:动态创建元素 + 播放音频(`audio`标签)。如上述示例部分源码如下:


```js
sounds.forEach(sound => {
    const btn = create('button');
    btn.textContent = sound;
    btn.type = "button";
    const audio = create('audio');
    audio.src = "./audio/" + sound + '.mp3';
    audio.id = sound;
    btn.addEventListener('click',() => {
        stopSounds();
        $('#' + sound).play();
    });
    buttonContainer.appendChild(btn);
    buttonContainer.insertAdjacentElement('beforebegin',audio);
});
```

# 10. Dad Jokes

效果如图所示:


![10.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7feaa758b4314fc2b838e12d39b34f18~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/68)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/68)

> 知识点总结:

1. CSS:`CSS`渐变 + 弹性盒子垂直水平居中 + 基本样式。
2. JavaScript:事件监听 + `fetch API`请求接口。如上述示例部分源码如下:


```js
const api = 'https://icanhazdadjoke.com';
const on = (el,type,handler,useCapture = false) => {
    if(el && type && handler){
        el.addEventListener(type,handler,useCapture);
    }
}
on(getJokeBtn,'click',request);
request();
async function request(){
    const res = await fetch(api,headerConfig);
    const data = await res.json();
    content.innerHTML = data.joke;
}
```

# 11. Event Keycodes

效果如图所示:


![11.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a7b656a9f96144d3b99e09765dcbb9e1~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/69)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/69)

> 知识点总结:

1. CSS:`CSS`渐变 + 弹性盒子垂直水平居中 + 基本样式。
2. JavaScript:键盘事件监听 + 事件对象的属性。如上述示例部分源码如下:


```js
const container = document.querySelector('#container');
window.addEventListener("keydown",event => {
    createKeyCodeTemplate(event);
});

function createKeyCodeTemplate(e){
    const { key,keyCode,code } = e;
    let template = "";
    [
        {
            title:"event.key",
            content:key === " " ? "Space" : key
        },
        {
            title:"event.keyCode",
            content:keyCode
        },
        {
            title:"event.code",
            content:code
        }
    ].forEach(value => {
        template += `<div class="key"><small>${ value.title }</small>${ value.content }</div>`;
    });
    container.innerHTML = template;
}
```

# 12. Faq Collapse

效果如图所示:

![12.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/39bba9c930ff4d72897853d59e2e3bff~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/70)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/70)

> 知识点总结:

1. CSS:`font-awesome`字体库的使用 + 伪元素选择器 + 定位 + `CSS`渐变 + 基本样式。
2. JavaScript:动态创建元素 + 类名的切换 + 事件代理。如上述示例部分源码如下:


```js
const faqs = document.querySelectorAll('.faq-container > .faq');
container.addEventListener('click',e => {
   if(e.target.className.indexOf('faq-icon') > -1){
      faqs[[].indexOf.call(faqs,e.target.parentElement)].classList.toggle('active');
   }
});
```

# 13. Random Choice Picker

效果如图所示:


![13.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/19a6874d18cc43bbbd6b3a8a90c24450~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/71)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/71)

> 知识点总结:

1. CSS:基本样式。
2. JavaScript:动态创建元素 + 类名的切换 + 延迟定时器的用法 + 随机数 + 键盘事件的监听。如上述示例部分源码如下:


```js
function createTags(value,splitSymbol){
    if(!value || !value.length)return;
    const words = value.split(splitSymbol).map(v => v.trim()).filter(v => v);
    tags.innerHTML = '';
    words.forEach(word => {
        const tag = document.createElement('span');
        tag.classList.add('tag');
        tag.innerText = word;
        tags.appendChild(tag);
    })
}
function randomTag(){
    const time = 50;
    let timer = null;
    let randomHighLight = () => {
        const randomTagElement = pickerRandomTag();
        highLightTag(randomTagElement);
        timer = setTimeout(randomHighLight,100);
        setTimeout(() => {
            unHighLightTag(randomTagElement);
        },100);
    }
    randomHighLight();
    setTimeout(() => {
        clearTimeout(timer);
        setTimeout(() => {
            const randomTagElement = pickerRandomTag();
            highLightTag(randomTagElement);
        },100);
    },time * 100);
}
function pickerRandomTag(){
    const tagElements = document.querySelectorAll('#tags .tag');
    return tagElements[Math.floor(Math.random() * tagElements.length)];
}
```

# 14. Animated Navigation

效果如图所示:


![14.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/847bf226147146a59e83eb1c90fd4d06~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/72)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/72)

> 知识点总结:

1. CSS:基本样式 + 定位 + 位移变换以及角度旋转。
2. JavaScript:类名的切换。如上述示例部分源码如下:


```js
toggle.addEventListener('click',() => nav.classList.toggle('active'));
```

# 15. Incrementing Counter

效果如图所示:


![15.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b5ed7b0da0a4007b0f0e6cfdbed487b~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/73)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/73)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `font-awesome`字体库的使用。
2. JavaScript:动态创建元素 + 定时器实现增量相加。如上述示例部分源码如下:

```js
function updateCounterHandler() {
    const counters_elements = document.querySelectorAll('.counter');
    counters_elements.forEach(element => {
        element.textContent = '0';
        const updateCounter = () => {
            const value = +element.getAttribute('data-count');
            const textContent = +element.textContent;
            const increment = value / 100;
            if (textContent < value) {
                element.textContent = `${Math.ceil(increment + textContent)}`;
                setTimeout(updateCounter, 10);
            } else {
                element.textContent = value;
            }
        }
        updateCounter();
    });
}
```
# 16. Drink Water

效果如图所示:

![16.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c33188cbb90e480a8da1f4069c0d5b2c~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/74)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/74)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `CSS`过渡特效。
2. JavaScript:正则表达式 + 循环 + 样式与内容的设置。如上述示例部分源码如下:

```js
if (actives.length === l) {
                setHeightVisible('0', 'hidden', '350px', 'visible');
                setTextContent("100%", "0L");
            } else if (actives.length === 0) {
                setHeightVisible('350px', 'visible', '0', 'hidden');
                setTextContent("12.5%", "2L");
            } else {
                const h1 = unitHei * (l - actives.length) + 'px';
                const h2 = unitHei * actives.length + 'px';
                setHeightVisible(h1, 'visible', h2, 'visible');
                const t1 = (unitHei * actives.length / 350) * 100 + "%";
                const t2 = (cups[i].textContent.replace(/ml/, "").replace(/\s+/, "") - 0) * (l - actives.length) / 1000 + 'L';
                setTextContent(t1, t2);
}
```

# 17. movie-app

效果如图所示:

![17.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee7e72c965f4404983083abcbf87f49a~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/75)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/75)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `CSS`过渡特效 + 清除浮动。
2. JavaScript:接口请求 + 键盘事件的监听。如上述示例部分源码如下:

```js
search.addEventListener('keydown',e => {
        if(e.keyCode === 13){
            let value = e.target.value.replace(/\s+/,"");
            if(value){
                getMovies(SEARCH_API + value);
                setTimeout(() => {
                    e.target.value = "";
                },1000);
            }else{
                window.location.reload(true);
            }
        }
    })
```

> PS:这个示例效果由于接口访问受限，需要翻墙访问。

# 18. background-slider

效果如图所示:

![18.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/400ba732de6240d5a19c47ecb5d1a8e0~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/76)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/76)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + 阴影效果 + 定位 + 伪元素选择器。
2. JavaScript:背景设置与类名的操作。如上述示例部分源码如下:


```js
let currentActive = 0;
function setBackgroundImage(){
    const url = slideItems[currentActive].style.backgroundImage;
    background.style.backgroundImage = url;
}
function setSlideItem(){
    const currentSlide = slideItems[currentActive];
    const siblings = [].filter.call(slideItems,slide => slide !== currentSlide);
    currentSlide.classList.add('active');
    siblings.forEach(slide => slide.classList.remove('active'));
}
```

# 19. theme-clock

效果如图所示:

![19.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d7c888ed0cf04567a0dcb5e05020bf00~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/77)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/77)

> 知识点总结:

1. CSS:基本样式 + `CSS`变量 + 阴影效果 + 定位。
2. JavaScript:中英文的切换以及主题模式的切换，还有日期对象的处理。如上述示例部分源码如下:


```js
function setCurrentDate(){
    const date = new Date();
    const month = date.getMonth();
    const day = date.getDay();
    const time = date.getDate();
    const hour = date.getHours();
    const hourForClock = hour % 12;
    const minute = date.getMinutes();
    const second = date.getSeconds();
    const amPm = hour >= 12 ? langText[currentLang]['time-after-text'] : langText[currentLang]['time-before-text'];
    const w = currentLang === 'zh' ? dayZHs : days;
    const m = currentLang === 'zh' ? monthZHs : months;
    const values = [
        `translate(-50%,-100%) rotate(${ scale(hourForClock,0,11,0,360) }deg)`,
        `translate(-50%,-100%) rotate(${ scale(minute,0,59,0,360) }deg)`,
        `translate(-50%,-100%) rotate(${ scale(second,0,59,0,360) }deg)`
    ];
    [hourEl,minuteEl,secondEl].forEach((item,index) => setTransForm(item,values[index]));
    timeEl.innerHTML = `${ hour }:${ minute >= 10 ? minute : '0' + minute }&nbsp;${ amPm }`;
    dateEl.innerHTML = `${w[day]},${ m[month]}<span class="circle">${ time }</span>${ langText[currentLang]['date-day-text'] }`;
    timer = setTimeout(setCurrentDate,1000);
}
```

> PS:本示例也用到了与示例5一样的工具函数`scale`函数

# 20. button-ripple-effect

效果如图所示:

![20.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dbaa67e77a494125a2cb71d6ca006f27~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/78)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/78)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `CSS`动画。
2. JavaScript:坐标的计算 + 偏移 + 定时器。如上述示例部分源码如下:


```js
const x = e.clientX;
const y = e.clientY;
const left = this.offsetLeft;
const top = this.offsetTop;

const circleX = x - left;
const circleY = y - top;
const span = document.createElement('span');
span.classList.add('circle');
span.style.left = circleX + 'px';
span.style.top = circleY + 'px';
this.appendChild(span);
setTimeout(() => span.remove(),1000);
```

# 21. drawing-app

效果如图所示:

![21.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91dd780d17b147efaab4c3078b08baa8~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/79)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/79)

> 知识点总结:

1. CSS:基本样式。
2. JavaScript:`canvas API` + `mouse`事件以及计算`x`与`y`坐标 + `ewColorPicker`的用法。如上述示例部分源码如下:


```js
function mouseDownHandler(e){
    isPressed = true;
    x = e.offsetX;
    y = e.offsetY;
}
function throttle(fn,wait = 100){
    let done = false;
    return (...args) => {
        if(!done){
            fn.call(this,args);
            done = true;
        }
        setTimeout(() => {
            done = false;
        },wait);
    }
}
```
# 22. drag-n-drop

效果如图所示:


![22.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b133ab102e94d10af0fe764710c0004~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/80)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/80)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `CSS`弹性盒子布局。
2. JavaScript:`drag event API` + 随机生成图片。如上述示例部分源码如下:


```js
function onDragStart(){
    this.classList.add('drag-move');
    setTimeout(() => this.className = "invisible",200);
}
function onDragEnd(){
    this.classList.add("drag-fill");
}

function onDragOver(e){
    e.preventDefault();
}
function onDragEnter(e){
    e.preventDefault();
    this.classList.add('drag-active');
}
function onDragLeave(){
    this.className = "drag-item";
}
function onDrop(){
    this.className = "drag-item";
    this.appendChild(dragFill);
}
```

# 23. content-placholder

效果如图所示:

![23.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbd2386b00154a00a5bcf86215e3eccc~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/81)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/81)

> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + `CSS`卡片样式。
2. JavaScript:骨架屏加载效果。如上述示例部分源码如下:

```js
animated_bgs.forEach(bg => bg.classList.remove("animated-bg"));
animated_bgs_texts.forEach(text => text.classList.remove("animated-bg-text"));
```

# 24. sticky-navbar

效果如图所示:

![24.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/942be7751c724247a854634ef7a3664d~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/82)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/82)


> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + 固定定位导航。
2. JavaScript:滚动事件。如上述示例部分源码如下:

```js
window.addEventListener("scroll",e => {
    if(window.scrollY > nav.offsetHeight + 100) {
        nav.classList.add("active");
    }else{
        nav.classList.remove("active");
    }
})
```

> PS:这里也做了移动端的布局。

# 25. double-slider

效果如图所示:

![25.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d6468e9459ba440b9c27dd5546927ceb~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/83)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/83)


> 知识点总结:

1. CSS:基本样式 + `CSS`渐变 + 位移变换。
2. JavaScript:轮播图的实现思路，主要还是利用`transformY`移动端使用`transformX`。如上述示例部分源码如下:

```js
function setTransform(){
    let translate = isHorizontal() ? "translateX" : "translateY";
    leftSlide.style.transform = `${ translate }(${position * currentIndex}px)`;
    rightSlide.style.transform = `${ translate }(${-(position * currentIndex)}px)`;
}
```

# 26. toast-notification

效果如图所示:

![26.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f92a6fa1fc904f5f8b72d0c8ffdbe53a~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/84)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/84)


> 知识点总结:

1. CSS:基本样式 + 消息提示框的基本样式。
2. JavaScript:封装一个随机创建消息提示框。如上述示例部分源码如下:

```js
function createNotification({message = null,type = null,auto = false,autoTime = 1000,left = 0,top = 0}){
    const toastItem = createEle("div");
    let closeItem = null;
    if(!auto){
        closeItem = createEle("span");
        closeItem.innerHTML = "&times;";
        closeItem.className = "toast-close-btn";
    }
    toastItem.className = `toast toast-${type}`;
    toastItem.textContent = message;
    if(closeItem)toastItem.appendChild(closeItem);
    container.appendChild(toastItem);
    const leftValue = (left - toastItem.clientWidth) <= 0 ? 0 : left - toastItem.clientWidth - 30;
    const topValue = (top - toastItem.clientHeight) <= 0 ? 0 : top - toastItem.clientHeight - 30;
    toastItem.style.left = leftValue + 'px';
    toastItem.style.top = topValue + 'px';
    if(auto){
        setTimeout(() => {
            toastItem.remove();
        },autoTime);
    }else{
        closeItem.addEventListener("click",() => {
            toastItem.remove();
        });
    }
    
}
```

消息提示框实现思路可以参考[这篇文章](https://juejin.cn/post/6844904152393318413)。

# 27. github-profiles

效果如图所示:

![27.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4b456a2e61054e8bbdbd839a755359a5~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/85)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/85)

> 知识点总结:

1. CSS:基本样式。
2. JavaScript:`try-catch`处理异常语法 + `axios API`请求`github API` + `async-await`语法。如上述示例部分源码如下:

```js
async function getRepoList(username){
    try {
        const { data } = await axios(githubAPI + username + '/repos?sort=created');
        addRepoList(data);
    } catch (error) {
        if(error.response.status == 404){
            createErrorCard("查找仓库出错!");
        }
    }
}
```

# 28. double-click-heart

效果如图所示:

![28.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e74a6ea77f5241cb86d3e9371e768720~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/86)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/86)


> 知识点总结:

1. CSS:基本样式 + `CSS`画爱心。
2. JavaScript:事件次数的计算。如上述示例部分源码如下:

```js
function clickHandler(e){
    if(clickTime === 0){
        clickTime = Date.now();
    }else{
        if(Date.now() - clickTime < 400){
            createHeart(e);
            clickTime = 0;
        }else{
            clickTime = Date.now();
        }
    }
}
```

# 29. auto-text-effect


效果如图所示:


![29.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/40acc74bf03745c7966ce5209b8e5332~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/87)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/87)


> 知识点总结:

1. CSS:基本样式。
2. JavaScript:定时器 + 定时器时间的计算。如上述示例部分源码如下:

```js
let time = 300 / speed.value;
writeText();
function writeText(){
    text.innerHTML = string.slice(0,idx);
    idx++;
    if(idx > string.length){
        idx = 1;
    }
    setTimeout(writeText,time);
}
speed.addEventListener("input",e => time = 300 / e.target.value);
```

# 30. password generator

效果如图所示:


![30.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c925deefdbe445438957859c56c62307~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/88)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/88)


> 知识点总结:

1. CSS:基本样式布局。
2. JavaScript:中英文切换 + 选择框事件监听 + 随机数 + `copy API`。如上述示例部分源码如下:

```js
function getRandomLower(){
    // a ~ z 的code为 97 ~ 122
    // 可根据charCodeAt()方法获取
    return String.fromCharCode(Math.floor(Math.random() * 26) + 97);
}
function getRandomUpper(){
    // A ~ Z 的code为 65 ~ 90
    // 可根据charCodeAt()方法获取
    return String.fromCharCode(Math.floor(Math.random() * 26) + 65);
}
function getRandomNumber(){
    // 0 ~ 9的code为48 ~ 57
    // 可根据charCodeAt()方法获取
    return String.fromCharCode(Math.floor(Math.random() * 10) + 48);
}
```

# 31. good-cheap-fast


效果如图所示:

![31.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/912aa5d48ee4400c8515c4991e4d218f~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/89)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/89)


> 知识点总结:

1. CSS:`CSS`实现开关小组件样式 + 基本布局样式。
2. JavaScript:`input`的`change`事件的监听。如上述示例部分源码如下:

```js
const checkBoxElements = document.querySelectorAll(".switch-container input[type='checkbox']");
checkBoxElements.forEach(item => item.addEventListener("change",e => {
    const index = Array.from(checkBoxElements).indexOf(e.target);
    if(Array.from(checkBoxElements).every(v => v.checked)){
        if(index === 0){
            checkBoxElements[2].checked = false;
        }else if(index === 1){
            checkBoxElements[0].checked = false;
        }else{
            checkBoxElements[1].checked = false;
        }
    }
}));
```

# 32. notes-app


效果如图所示:

![32.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3ce021abf2404606a04bea3b02618f09~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/90)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/90)


> 知识点总结:

1. CSS:基本样式 + 卡片布局。
2. JavaScript:`marked.js`的使用 + `localStorage API`存储数据 + 鼠标光标位置的计算。如上述示例部分源码如下:

```js
 on($(".edit", note), "click", e => {
      const isFocus = textarea.getAttribute("data-focus");
      if (isFocus === "false") {
            textarea.setAttribute("data-focus","true");
            if(textarea.classList.contains("hidden")){
                textarea.classList.remove("hidden");
            }
            if(!focusStatus){
                textarea.value = notes[index].text;
            }
            const text = textarea.value.trim();
            // console.log(text);
            if (textarea.setSelectionRange) {
                textarea.focus();
                textarea.setSelectionRange(text.length, text.length);
            }else if (textarea.createTextRange) {
                const range = textarea.createTextRange();
                range.collapse(true);
                range.moveEnd('character', text.length);
                range.moveStart('character', text.length);
                range.select();
            }
     } else {
        textarea.setAttribute("data-focus","false");
        notes[index].text = textarea.value;
        main.innerHTML = marked(notes[index].text);
        setData("notes", notes);
    }
});
```

# 33. animated-countdown


效果如图所示:


![33.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4435d638702f4598bf6854bfb9e632fa~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/91)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/91)


> 知识点总结:

1. CSS:基本样式 + 位移、旋转、缩放动画。
2. JavaScript:动画事件 + 动画的创建与重置。如上述示例部分源码如下:

```js
function runAnimation(){
    const numArr = $$("span",numGroup);
    const nextToLast = numArr.length - 1;
    numArr[0].classList.add("in");
    numArr.forEach((num,index) => {
        num.addEventListener("animationend",e => {
            const {animationName} = e;
            if(animationName === "goIn" && index !== nextToLast){
                num.classList.remove("in");
                num.classList.add("out");
            }else if(animationName === "goOut" && num.nextElementSibling){
                num.nextElementSibling.classList.add("in");
            }else{
                counter.classList.add("hide");
                final.classList.add("show");
            }
        })
    })
}
function resetAnimation(){
    counter.classList.remove("hide");
    final.classList.remove("show");
    const numArr = $$("span",numGroup);
    if(numArr){
        numArr.forEach(num => num.classList.value = '');
        numArr[0].classList.add("in");
    }
}
```

# 34. image-carousel


效果如图所示:


![34.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da25bfc28ce044a8b363b8689e79c4c4~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/92)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/92)


> 知识点总结:

1. CSS:基本样式布局。
2. JavaScript:定时器实现轮播。如上述示例部分源码如下:

```js
function changeCarouselItem(){
    if(currentIndex > carouselItems.length - 1){
        currentIndex = 0;
    }else if(currentIndex < 0){
        currentIndex = carouselItems.length - 1;
    }
        carousel.style.transform = `translateX(-${ currentIndex * carouselContainer.offsetWidth     }px)`;
}
```

> PS:这里额外踩了一个定时器的坑，也就是说，比如我们使用setTimeout模拟实现setInterval方法在这里是会出现问题的，我在js代码里添加了注释说明。

```js
// let interval = mySetInterval(run,1000);
// Why use this method can't achieve the desired effect?
// Use this method as follow to replace window.setInterval,clicked the prev button more to get the stranger effect.
// Maybe this method does not conform to the specification,So make sure to use window.setInterval.
// function mySetInterval(handler,time = 1000){
//     let timer = null;
//     const interval = () => {
//         handler();
//         timer = setTimeout(interval,time);
//     }
//     interval();
//     return {
//         clearMyInterval(){
//             clearTimeout(timer);
//         }
//     }
// }
```

这是因为我们用`setTimeout`实现的定时器并不符合规范，`setInterval`默认会有`10ms`的延迟执行。
[参考规范](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers)。

# 35. hover board

效果如图所示:

![35.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/606ebf17e4e14acf864a8c4d709bec0a~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/93)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/93)


> 知识点总结:

1. CSS:基本样式 + `CSS`渐变。
2. JavaScript:动态创建元素 + 悬浮事件。如上述示例部分源码如下:

```js
function setColor(element){
    element.style.background = `linear-gradient(135deg, ${ randomColor() } 10%, ${ randomColor() } 100%)`;
    element.style.boxShadow = `0 0 2px ${ randomColor() },0 0 10px ${ randomColor() }`;
}
function removeColor(element){
    element.style.background = `linear-gradient(135deg, #1d064e 10%, #10031a 100%)`;
    element.style.boxShadow = `0 0 2px #736a85`;
}
```

# 36. pokedex


效果如图所示:


![36.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/722e945303454955af71d619e79874c8~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/94)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/94)


> 知识点总结:

1. CSS:基本样式布局 + `CSS`渐变。
2. JavaScript:`fetch API`接口请求 + 创建卡片。如上述示例部分源码如下:

```js
function createPokemon(pokemon){
    const pokemonItem = document.createElement("div");
    pokemonItem.classList.add("pokedex");

    const name = pokemon.name[0].toUpperCase() + pokemon.name.slice(1).toLowerCase();
    const id = pokemon.id.toString().padStart(3,"0");
    const poke_types = pokemon.types.map(type => type.type.name);
    const type = main_types.find(type => poke_types.indexOf(type) > -1);
    const color = colors[type];
    pokemonItem.style.background = `linear-gradient(135deg, ${ color } 10%, ${ randomColor() } 100%)`;
    pokemonItem.innerHTML = `
    <div class="pokedex-avatar">
        <img src="https://pokeres.bastionbot.org/images/pokemon/${pokemon.id}.png" alt="the pokemon">
    </div>
    <div class="info">
        <span class="number">#${ id }</span>
        <h3 class="name">${ name }</h3>
        <small class="type">Type:<span>${ type }</span></small>
    </div>`;
    container.appendChild(pokemonItem);
}
```

> 特别说明:接口似乎不太稳定，也许是我网络原因，图片没有加载出来。

# 37. mobile-tab-navigation


效果如图所示:

![37.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d841abeb5ff4db5b72a44cec20bf34d~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/95)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/95)


> 知识点总结:

1. CSS:基本样式布局 + `CSS`渐变实现手机布局。
2. JavaScript:感觉就是移动端实现一个轮播图切换，利用`opacity`的设置，没什么好说的。如上述示例部分源码如下:


```js
function hideAllElement(nodeList){
    nodeList.forEach(item => item.classList.remove("active"));
}
navItems.forEach((item,index) => {
    item.addEventListener("click",() => {
        hideAllElement(navItems);
        hideAllElement(carouselItems);
        item.classList.add("active");
        carouselItems[index].classList.add("active");
    })
})
```

# 38. password-strength-background


效果如图所示:

![38.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4835cc104d074d13bcd8be4c4044fdc8~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/96)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/96)


> 知识点总结:

1. CSS:其实主要是使用`tailwind.css`这个原子化`CSS`框架。
2. JavaScript:监听输入框事件，然后改变背景模糊度。如上述示例部分源码如下:

```js
password.addEventListener("input",e => {
    const { value } = e.target;
    const blur = 20 - value.length * 2;
    background.style.filter = `blur(${ blur }px)`;
});
```

# 39. 3D-background-boxes


效果如图所示:

![39.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/efede501a23b49f09032447000b0ecc0~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/97)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/97)


> 知识点总结:

1. CSS:`vw、vh`布局 + `skew`倾斜变换。
2. JavaScript:循环 + 背景图定位的设置。如上述示例部分源码如下:

```js
function createBox(){
    for(let i = 0;i < 4;i++){
        for(let j = 0;j < 4;j++){
            const box = document.createElement("div");
            box.classList.add("box");
            box.style.backgroundPosition = `${ -j * 15 }vw ${ -i * 15 }vh`;
            boxContainer.appendChild(box);
        }
    }
}
```

# 40. Verify Your Account

效果如图所示:

![40.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8df8e542e14d4451a38d55f49467d8e3~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/98)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/98)


> 知识点总结:

1. CSS:基本样式布局 + `input`的一些特别样式设置。
2. JavaScript:`JavaScript focus`事件。如上述示例部分源码如下:

```css
.code-container .code:focus {
    border-color: #2397ef;
}
.code::-webkit-outer-spin-button,
.code::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.code:valid {
    border-color: #034775;
    box-shadow: 0 10px 10px -5px rgba(0, 0, 0, 0.25);
}
```

```js
function onFocusHandler(nodeList){
    onFocus(nodeList[0]);
    nodeList.forEach((node,index) => {
        node.addEventListener("keydown",e => {
            // console.log(e.key);
            if(e.key >= 0 && e.key <= 9){
                nodeList[index].value = "";
                setTimeout(() => onFocus(nodeList[index + 1]),0);
            }else{
                setTimeout(() => onFocus(nodeList[index - 1]),0);
            }
        })
    });
}
function onFocus(node){
    if(!node)return;
    const { nodeName } = node;
    return nodeName && nodeName.toLowerCase() === "input" && node.focus();
}
```

# 41. live-user-filter


效果如图所示:

![41.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5dc2fc0531d54476b308181751ecbfcf~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/99)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/99)


> 知识点总结:

1. CSS:基本样式布局 + 滚动条样式。
2. JavaScript:`fetch API`接口请求 + `input`事件过滤数据。如上述示例部分源码如下:

```js
async function requestData(){
    const res = await fetch('https://randomuser.me/api?results=50');
    const { results } = await res.json();
    result.innerHTML = "";

    results.forEach(user => {
        const listItem = document.createElement("li");
        filterData.push(listItem);
        const { picture:{ large },name:{ first,last },location:{ city,country } } = user;
        listItem.innerHTML = `
            <img src="${ large }" alt="${ first + ' ' + last }" />
            <div class="user-info">
                <h4>${ first }  ${ last }</h4>
                <p>${ city },${ country }</p>
            </div>
        `;
        result.appendChild(listItem);
    })
}
```

# 42. feedback-ui-design


效果如图所示:

![42.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f3a1fda1b29f45e59eac426020a9919b~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/100)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/100)


> 知识点总结:

1. CSS:`CSS`画爱心 + 基本样式布局。
2. JavaScript:选项卡切换功能的实现。如上述示例部分源码如下:

```js
ratingItem.forEach(item => {
    item.addEventListener("click",e => {
        siblings(item).forEach(sib => sib.classList.remove("active"));
        item.classList.add("active");
        selectRating = item.innerText;
    })
});
send.addEventListener("click",() => {
    selectRatingItem.innerText = selectRating;
    sendPage.classList.add("hide");
    receivePage.classList.remove("hide");
});
back.addEventListener("click",() => {
    selectRating = $(".rating.active").innerText;
    selectRatingItem.innerText = $(".rating.active").innerText;
    sendPage.classList.remove("hide");
    receivePage.classList.add("hide");
});
```

# 43. custom-range-slider

效果如图所示:


![43.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/419928799a434ecbafb2ceb50df32f1b~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/101)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/101)


> 知识点总结:

1. CSS:`input`元素的样式设置(兼容写法) + 基本样式布局。
2. JavaScript:`input range`元素的`input`事件监听。如上述示例部分源码如下:

```css
input[type="range"]::-webkit-slider-runnable-track {
    background-image: linear-gradient(135deg, #E2B0FF 10%, #9F44D3 100%);
    width: 100%;
    height: 12px;
    cursor: pointer;
    border-radius: 4px;
}
input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background-image: linear-gradient(135deg, #d9e2e6 10%, #e4e1e4 100%);
    border: 1px solid #b233e4;
    cursor: pointer;
    margin-top: -6px;
}
```

```js
range.addEventListener("input",e => {
    // string to the number
    const value = +e.target.value;
    const label = e.target.nextElementSibling;

    const range_width = getStyle(e.target,"width");//XXpx
    const label_width = getStyle(label,"width");//XXpx

    // Due to contain px,slice the width
    const num_width = +range_width.slice(0,range_width.length - 2);
    const num_label_width = +label_width.slice(0,label_width.length - 2);

    const min = +e.target.min;
    const max = +e.target.max;

    const left = value * (num_width / max) - num_label_width / 2 + scale(value,min,max,10,-10);

    label.style.left = `${left}px`;
    label.innerHTML = value;
});
```

# 44. netflix-mobile-navigation


效果如图所示:

![44.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a31755e123f44a3caba0c30f4e6b2526~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/102)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/102)


> 知识点总结:

1. CSS:导航宽度的变化 + `CSS`渐变 + 基本样式布局。
2. JavaScript:点击按钮切换导航栏的显隐。如上述示例部分源码如下:

```js
function changeNav(type){
    navList.forEach(nav => nav.classList[type === "open" ? "add" : "remove"]("visible"));
}
```

# 45. quiz-app


效果如图所示:

![45.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a249e26e6b1466999ad7b1674ea8f83~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/103)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/103)


> 知识点总结:

1. CSS:卡片布局 + 基本样式。
2. JavaScript:表单提交 + 选择题的计算。如上述示例部分源码如下:

```js
submit.addEventListener("click",() => {
    const answer = getSelected();
    if(answer){
        if(answer === quizData[currentQuestion].correct){
            score++;
        }
        currentQuestion++;
        if(currentQuestion > quizData.length - 1){
            quizContainer.innerHTML = `
                <h2>You answered ${ score } / ${ quizData.length } questions correctly!</h2>
                <button type="button" class="btn" onclick="location.reload()">reload</button>
            `
        }else{
            loadQuiz();
        }
    }
})
```

# 46. testimonial-box-switcher


效果如图所示:

![46.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b238fc4381a54fe2b1a1fcf995eb8daf~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/105)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/105)


> 知识点总结:

1. CSS:`CSS`动画 + 宽度的改变实现进度条 + `font-awesome`字体库的使用 + 基本样式布局。
2. JavaScript:定时器的用法,注意定时器的执行时间与设置进度条的执行动画时间一样。如上述示例部分源码如下:

```css
.progress-bar {
    width: 100%;
    height: 4px;
    background-color: #fff;
    animation: grow 10s linear infinite;
    transform-origin: left;
}
@keyframes grow {
    0% {
        transform: scaleX(0);
    }
}
```

```js
function updateTestimonial(){
    const { text,name,position,photo } = testimonials[currentIndex];
    const renderElements = [testimonial,username,role];
    userImage.setAttribute("src",photo);
    order.innerHTML = currentIndex + 1;
    [text,name,position].forEach((item,index) => renderElements[index].innerHTML = item);
    currentIndex++;
    if(currentIndex > testimonials.length - 1){
        currentIndex = 0;
    }
}
```

> 特别说明:CSS也是可以实现进度条的。

# 47. random-image-feed


效果如图所示:

![47.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/40543ddf186e4d60875295a02fb3ea54~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/106)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/106)


> 知识点总结:

1. CSS:图片布局 + 基本样式布局 + `CSS`提示框的实现(定位 + 伪元素) + `CSS`实现回到顶部效果。
2. JavaScript:随机数 + (滚动事件的监听)控制回到顶部按钮的显隐。如上述示例部分源码如下:


```js
function onLoad(rows = 5) {
    container.innerHTML = "";
    for (let i = 0; i < rows * 3; i++) {
        const imageItem = document.createElement("img");
        imageItem.src = `${refreshURL}${getRandomSize()}`;
        imageItem.alt = "random image-" + i;
        imageItem.loading = "lazy";
        container.appendChild(imageItem);
    }
}
function getRandomSize() {
    return `${getRandomValue()}x${getRandomValue()}`;
}
function getRandomValue() {
    return Math.floor(Math.random() * 10) + 300;
}
window.onload = () => {
    changeBtn.addEventListener("click", () => onLoad());
    window.addEventListener("scroll", () => {
        const { scrollTop, scrollHeight } = document.documentElement || document.body;
        const { clientHeight } = flexCenter;
        back.style.display = scrollTop + clientHeight > scrollHeight ? 'block' : 'none';
    })
    onLoad();
}
```

# 48. todo-list


效果如图所示:

![48.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/466975360dda47c8bf29b1b18028a9d2~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/107)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/107)


> 知识点总结:

1. CSS:基本样式布局 + `CSS`渐变。
2. JavaScript:`keydown`与`contextmenu`事件的监听 + `localStorage API`存储数据。如上述示例部分源码如下:

```js
function addTodo(todo){
    let inputValue = input.value;
    if(todo){
        inputValue = todo.text;
    }
    if(inputValue){
        const liItem = document.createElement("li");
        liItem.innerText = inputValue;
        if(todo && todo.complete){
            liItem.classList.add("complete");
        }
        liItem.addEventListener("click",() => {
            liItem.classList.toggle("complete");
            updateList();
        });
        liItem.addEventListener("contextmenu",(e) => {
            e.preventDefault();
            liItem.remove();
            updateList();
        });
        todoList.appendChild(liItem);
        input.value = "";
        updateList();
    }
}
function updateList(){
    const listItem = $$("li",todoList);
    const saveTodoData = [];
    listItem.forEach(item => {
        saveTodoData.push({
            text:item.innerText,
            complete:item.classList.contains("complete")
        })
    });
    localStorage.setItem("todoData",JSON.stringify(saveTodoData));
}
```

# 49. insect-catch-game


效果如图所示:

![49.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5b47d53e48be4ec6992f0c62624ba9ee~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/js/108)
* [在线示例](https://www.eveningwater.com/my-web-projects/js/108)


> 知识点总结:

1. CSS:基本样式布局。
2. JavaScript:中英文切换 + 替换元素中的文本(不包含标签元素) + 定时器的用法。如上述示例部分源码如下:

```js
function replaceText(el,text,wrapSymbol = ""){
    let newText = [];
    if(wrapSymbol){
        // why not use split method?,because it can filter the wrap symbol.
        const getIndex = (txt) => txt.search(new RegExp("\\" + wrapSymbol));
        let searchIndex = getIndex(text),i = 0,len = text.length;
        while(searchIndex !== -1){
            newText.push(text.slice(i,searchIndex) + wrapSymbol);
            i = searchIndex;
            if(getIndex(text.slice(searchIndex + 1)) === -1){
                newText.push(text.slice(searchIndex + 1,len));
            }
            searchIndex = getIndex(text.slice(searchIndex + 1));
        }
    }
    const walk = (el,handler) => {
        const children = Array.from(el.childNodes);
        let wrapIndex = children.findIndex(node => node.nodeName.toLowerCase() === "br");
        children.forEach((node,index) => {
            if(node.nodeType === 3 && node.textContent.replace(/\s+/,"")){
                wrapSymbol ? handler(node,newText[index - wrapIndex < 0 ? 0 : index - wrapIndex]) : handler(node,text);;
            }
        });
    }
    walk(el,(node,txt) => {
        const parent = node.parentNode;
        parent.insertBefore(document.createTextNode(txt),node);
        parent.removeChild(node);
    });
}
```
以上工具函数的实现参考[这里来实现的](https://stackoverflow.com/questions/12919074/js-replace-only-text-without-html-tags-and-codes)。

> 特别说明:这个示例算是一个比较综合的示例了。

# 50. kinetic-loader

效果如图所示:

![50.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8d1c88073253407882f145a5226c6c45~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/CSS/6/)
* [在线示例](https://www.eveningwater.com/my-web-projects/CSS/6/)


> 知识点总结:

1. CSS:CSS旋转动画 + 基本样式布局。
如上述示例部分源码如下:

```js
@keyframes rotateA {
    0%,25% {
        transform: rotate(0deg);
    }
    50%,75% {
        transform: rotate(180deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
@keyframes rotateB {
    0%,25% {
        transform: rotate(90deg);
    }
    50%,75% {
        transform: rotate(270deg);
    }
    100% {
        transform: rotate(450deg);
    }
}
```

最后一天，额外的实现了一个`404`效果，算是特别结尾吧，是不是很花哨(`^_^`)。这算是一个`CSS`动画的综合使用，如下所示:

# 51. 404 page

效果如图所示:

![51.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/79d56f5547444a73942ef0d35b91524b~tplv-k3u1fbpfcp-watermark.image)

* [源码](https://github.com/eveningwater/my-web-projects/tree/master/CSS/7/)
* [在线示例](https://www.eveningwater.com/my-web-projects/CSS/7/)


> 知识点总结:

1. CSS:`CSS`动画的用法 + 基本样式布局 + `svg`图标元素的样式设置。
如上述示例部分源码如下:

```js
@keyframes background {
    from {
        background: linear-gradient(135deg,#e0e0e0 10%,#ffffff 90%);
    }
    to {
        background: linear-gradient(135deg,#ffffff 10%,#e0e0e0 90%);
    }
}
@keyframes stampSlide {
    0% {
        transform: rotateX(90deg) rotateZ(-90deg) translateZ(-200px) translateY(130px);
    }
    100% {
        transform: rotateX(90deg) rotateZ(-90deg) translateZ(-200px) translateY(-4000px);
    }
}
@keyframes slide {
    0% {
        transform: translateX(0);
    }
    100% {
        transform: translateX(-200px);
    }
}
@keyframes roll {
    0% {
        transform: rotateZ(0deg);
    }
    85% {
        transform: rotateZ(90deg);
    }
    87% {
        transform: rotateZ(88deg);
    }
    90% {
        transform: rotateZ(90deg);
    }
    100% {
        transform: rotateZ(90deg);
    }
}
@keyframes zeroFour {
    0% {
        content:"4";
    }
    100% {
        content:"0";
    }
}
@keyframes draw {
    0% {
        stroke-dasharray: 30 150;
        stroke-dashoffset: 42;
    }
    100% {
        stroke:rgba(8, 69, 131, 0.9);
        stroke-dasharray: 224;
        stroke-dashoffset: 0;
    }
}
```