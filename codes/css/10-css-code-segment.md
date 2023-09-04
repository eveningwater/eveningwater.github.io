# 10 个 css 代码片段

以下这 10 个常用的 css 代码片段值得收藏，都可以用于平常的业务代码当中。

## 1. 点点点加载中效果

这是一个兼容性不错的用户体验很棒的点点点加载效果，实现思路如下:

- 使用自定义的标签元素 dot。
- 将 dot 元素设置为内联元素(display:inline-block),并设置溢出隐藏(overflow:hidden)，高度设置为 1em。
- 使用:before 伪元素结合\AUnicode 字符插入内容，并且使用 white-space:pre-wrap 保留换行效果，使用 css 动画。
- 使用 transform 和 translate 为...添加动画效果。

html 代码如下:

```html
<div class="loading">正在加载中<dot>...</dot></div>
```

css 代码如下:

```css
.loading {
  /**这里写自己自定义的样式 */
}
.loading > dot {
  height: 1em;
  overflow: hidden;
  display: inline-block;
  text-align: left;
  vertical-align: -0.25em;
  line-height: 1;
}
/* 核心代码 */
.loading > dot:before {
  display: block;
  /* 这行代码最重要 */
  content: '...\A..\A.';
  /* 值是Pre也是一样的效果 */
  white-space: pre-wrap;
  animation: dot 3s infinite step-start both;
}
@keyframes dot {
  33% {
    transform: translateY(-2em);
  }
  66% {
    transform: translateY(-1em);
  }
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/dot-loading.html"></iframe>

## 2. 对话框

创建一个顶部带有三角形的内容容器对话框，实现思路如下:

- 使用 :before 和 :after 伪元素创建两个三角形。
- 两个三角形的颜色应分别与容器的边框颜色和容器的背景颜色相同。
- 一个三角形的边框宽度 (:before) 应该比另一个三角形 (:after) 宽 1px，以便充当边框。
- 较小的三角形 (:after) 应位于较大三角形 (:before) 右侧 1px 处，以允许显示其左边框。

html 代码如下:

```html
<div class="container">Border with top triangle</div>
```

css 代码如下:

```css
.container {
  --borderColor--: #ddd;
  --bgColor--: #fff;
  position: relative;
  background-color: var(--bgColor--);
  padding: 15px;
  margin-top: 20px;
  border: 1px solid var(--borderColor--);
}
.container:before,
.container:after {
  content: '';
  position: absolute;
  bottom: 100%;
  left: 19px;
  border: 11px solid transparent;
  border-bottom-color: var(--borderColor--);
}
.container:after {
  left: 20px;
  border: 10px solid transparent;
  border-bottom-color: var(--bgColor--);
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/border-with-top-triangle.html"></iframe>

## 3. 弹跳加载效果

创建一个弹跳加载器动画,实现思路如下:

- 使用 @keyframes 定义弹跳动画，使用 opacity 和 transform 属性。 在 transform: translate3d() 上使用单轴平移来获得更好的动画性能。
- 为弹跳圆创建一个父容器 .bouncing-loader。 使用 display: flex 和 justify-content: center 将它们定位在中心。
- 给三个弹跳的圆形 `<div>` 元素设置相同的宽度和高度以及 border-radius: 50% 以使它们成为圆形。
- 将 bouncing-loader 动画应用于三个弹跳圆圈中的每一个。
- 为每个圆圈和动画方向使用不同的动画延迟：交替以创建适当的效果。

html 代码如下:

```html
<div class="bouncing-loader">
  <div class="bouncing-loader-item"></div>
  <div class="bouncing-loader-item"></div>
  <div class="bouncing-loader-item"></div>
</div>
```

css 代码如下:

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body,
html {
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;
}
.bouncing-loader {
  display: flex;
  justify-content: center;
  width: 150px;
}
.bouncing-loader-item {
  width: 16px;
  height: 16px;
  margin: 3rem 0.2rem;
  background-color: #0b16f1;
  border-radius: 50%;
  animation: bouncingLoader 0.6s infinite alternate;
}
.bouncing-loader-item:nth-child(2) {
  animation-delay: 0.2s;
}
.bouncing-loader-item:nth-child(3) {
  animation-delay: 0.4s;
}
@keyframes bouncingLoader {
  to {
    opacity: 0.1;
    transform: translate3d(0, -16px, 0);
  }
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/bouncing-loader.html"></iframe>

## 4. 动画边框按钮

在悬停时创建边框动画，实现思路如下:

- 使用 :before 和 :after 伪元素创建两个 24px 宽的盒子，在盒子的上方和下方彼此相对。
- 使用 :hover 伪类在悬停时将这些元素的宽度扩展到 100% 并使用过渡动画更改。

html 代码如下:

```html
<button class="animated-border-button">Submit</button>
```

css 代码如下:

```css
.animated-border-button {
  outline: none;
  background-color: #2396ef;
  padding: 12px 40px 10px;
  border: none;
  position: relative;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
}
.animated-border-button::before,
.animated-border-button::after {
  content: '';
  position: absolute;
  border: 0 solid transparent;
  height: 0;
  width: 24px;
  transition: all 0.3s cubic-bezier(0.075, 0.82, 0.165, 1);
}
.animated-border-button::before {
  border-top: 2px solid #2396ef;
  top: -4px;
  right: 0;
}
.animated-border-button::after {
  border-bottom: 2px solid #2396ef;
  bottom: -4px;
  left: 0;
}
.animated-border-button:hover::before,
.animated-border-button:hover::after {
  width: 100%;
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/button-border-animation.html"></iframe>

## 5. border 等高布局

使用 border 实现等高布局，实现思路如下:

- 给盒子元素设置一个左边框，边框宽度由子元素的宽度决定，这里是 150px。
- 给盒子元素的伪类设置清除浮动，这里不能使用 overflow:hidden 来清除。
- 给盒子元素的左边导航元素设置左浮动，并设置宽度和左负间距，间距值等于宽度值。
- 给盒子元素的右边内容元素设置 overflow:hidden。
- 导航子元素设置行高和右边子元素设置行高。

html 代码如下:

```html
<section class="box">
  <nav class="box-nav">
    <div class="box-nav-item">导航1</div>
  </nav>
  <section class="box-content">
    <div class="box-content-module">模块1</div>
  </section>
</section>
```

css 代码如下:

```css
.box {
  border-left: 150px solid #232425;
  background-color: #eeeded;
}

.box::after {
  content: '';
  clear: both;
  display: block;
}

.box-nav {
  width: 150px;
  margin-left: -150px;
  float: left;
}

.box-nav-item {
  line-height: 40px;
  color: #fff;
  text-align: center;
}

.box-content-module {
  line-height: 40px;
  text-align: center;
  color: #c40dd4;
}

.box-content {
  overflow: hidden;
}
```

javascript 代码如下所示:

```js
const navMore = document.getElementById('addNav'),
  moduleMore = document.getElementById('addContent');

if (navMore && moduleMore) {
  const nav = document.querySelector('.box-nav'),
    section = document.querySelector('.box-content');
  let navIndex = nav.children.length,
    sectionIndex = 1;
  let rand = () => 'f' + (Math.random() + '').slice(-1);
  navMore.onclick = function () {
    navIndex++;
    nav.insertAdjacentHTML(
      'beforeEnd',
      '<div class="box-nav-item">导航' + navIndex + '</div>'
    );
  };
  moduleMore.onclick = function () {
    sectionIndex++;
    section.insertAdjacentHTML(
      'beforeEnd',
      '<div class="box-content-module" style="background:#' +
        [rand(), rand(), rand()].join('') +
        '">模块' +
        sectionIndex +
        '</div>'
    );
  };
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/border-contour.html"></iframe>

## 6. 自定义复选框

创建带有状态更改动画的样式复选框,实现思路如下:

- 使用 `<svg>` 元素创建检查 `<symbol>` 并通过 `<use>` 元素将其插入以创建可重用的 SVG 图标。
- 创建一个 .ew-checkbox-group 并使用 flex box 为复选框创建适当的布局。
- 隐藏 `<input>` 元素并使用与其关联的标签来显示复选框和提供的文本。
- 使用 stroke-dashoffset 在状态更改时为检查符号设置动画。
- 通过 CSS 动画使用 transform: scale(0.9) 创建缩放动画效果。

html 代码如下:

```html
<div class="ew-checkbox-group">
  <label class="ew-checkbox">
    <svg class="ew-checkbox-symbol">
      <symbol id="ew-check" viewbox="0 0 12 10">
        <polyline
          points="1.5 6 4.5 9 10.5 1"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
        ></polyline>
      </symbol>
    </svg>
    <input type="checkbox" class="ew-checkbox-input" />
    <span class="ew-checkbox-item">
      <svg class="ew-checkbox-icon" width="12px" height="10px">
        <use xlink:href="#ew-check"></use>
      </svg>
    </span>
    <span class="ew-checkbox-item"> Apples </span>
  </label>
  <label class="ew-checkbox">
    <input type="checkbox" class="ew-checkbox-input" />
    <span class="ew-checkbox-item">
      <svg class="ew-checkbox-icon" width="12px" height="10px">
        <use xlink:href="#ew-check"></use>
      </svg>
    </span>
    <span class="ew-checkbox-item"> Oranges </span>
  </label>
</div>
```

css 代码如下:

```css
.ew-checkbox-group {
  background-color: #fff;
  color: rgba(0, 0, 0, 0.85);
  height: 64px;
  display: flex;
  flex-wrap: row wrap;
  justify-content: center;
  align-items: center;
}
.ew-checkbox-group .ew-checkbox-symbol {
  width: 0;
  height: 0;
  position: absolute;
  pointer-events: none;
  user-select: none;
}
.ew-checkbox-group * {
  box-sizing: border-box;
}
.ew-checkbox-input {
  position: absolute;
  visibility: hidden;
}
.ew-checkbox {
  user-select: none;
  cursor: pointer;
  padding: 6px 8px;
  border-radius: 6px;
  overflow: hidden;
  transition: all 0.3s ease-in-out;
  display: flex;
}
.ew-checkbox:not(:last-of-type) {
  margin-right: 6px;
}
.ew-checkbox:hover {
  background-color: rgba(0, 120, 255, 0.06);
}
.ew-checkbox .ew-checkbox-item {
  vertical-align: middle;
  transform: translate3d(0, 0, 0);
}
.ew-checkbox .ew-checkbox-item:first-of-type {
  position: relative;
  flex: 0 0 18px;
  width: 18px;
  height: 18px;
  border-radius: 4px;
  transform: scale(1);
  border: 1px solid #cdcdfd;
  transition: all 0.4s ease;
}
.ew-checkbox .ew-checkbox-icon {
  position: absolute;
  top: 3px;
  left: 2px;
  fill: none;
  stroke: #fff;
  stroke-dasharray: 16px;
  stroke-dashoffset: 16px;
  transition: all 0.4s ease;
  transform: translate3d(0, 0, 0);
}
.ew-checkbox .ew-checkbox-item:last-of-type {
  padding-left: 8px;
  line-height: 18px;
}
.ew-checkbox:hover .ew-checkbox-item:first-of-type {
  border-color: #2396ef;
}
.ew-checkbox-input:checked + .ew-checkbox-item:first-of-type {
  animation: zoom-in-out 0.3s ease;
  background-color: #2396ef;
  border-color: #2396ef;
}
.ew-checkbox-input:checked + .ew-checkbox-item .ew-checkbox-icon {
  stroke-dashoffset: 0;
}
@keyframes zoom-in-out {
  50% {
    transform: scale(0.9);
  }
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/custom-checkbox.html"></iframe>

## 7. 自定义单选框

创建一个带有状态更改动画的样式单选按钮,实现思路如下:

- 创建一个 .radio-container 并使用 flex box 为单选按钮创建适当的布局。
- 重置 `<input>` 上的样式并使用它来创建单选按钮的轮廓和背景。
- 使用 ::before 元素创建单选按钮的内圈。
- 使用 transform: scale(1) 和 CSS transition 在状态变化时创建动画效果。

html 代码如下:

```html
<div class="radio-container">
  <input type="radio" class="radio-input" id="male" name="sex" />
  <label for="male" class="radio">男</label>
  <input type="radio" class="radio-input" id="female" name="sex" />
  <label for="female" class="radio">女</label>
</div>
```

css 代码如下:

```css
.radio-container {
  box-sizing: border-box;
  background-color: #fff;
  color: #545355;
  height: 64px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: row wrap;
}
.radio-container * {
  box-sizing: border-box;
}
.radio-input {
  appearance: none;
  background-color: #fff;
  width: 16px;
  height: 16px;
  border: 1px solid #cccfdb;
  margin: 0;
  border-radius: 50%;
  display: grid;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease-in;
}
.radio-input::before {
  content: '';
  width: 6px;
  height: 6px;
  border-radius: 50%;
  transform: scale(0);
  transition: 0.3s transform ease-in-out;
  box-shadow: inset 6px 6px #fff;
}
.radio-input:checked {
  background-color: #2396ef;
  border-color: #2396ef;
}
.radio-input:checked::before {
  transform: scale(1);
}
.radio {
  cursor: pointer;
  padding: 6px 8px;
}
.radio:not(:last-child) {
  margin-right: 6px;
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/custom-radio.html"></iframe>

## 8. 打字效果

创建打字机效果动画,实现思路如下:

- 定义两个动画，键入动画字符和闪烁动画插入符号。
- 使用 ::after 伪元素将插入符号添加到容器元素。
- 使用 JavaScript 设置内部元素的文本并设置包含字符数的 --characters 变量。 此变量用于为文本设置动画。
- 使用 white-space: nowrap 和 overflow: hidden 使内容在必要时不可见。

html 代码如下:

```html
<div class="typewriter-effect">
  <div class="text" id="typewriter-text"></div>
</div>
```

css 代码如下:

```css
.typewriter-effect {
  display: flex;
  justify-content: center;
  font-family: monospace;
  font-size: 25px;
  color: #535455;
  font-weight: 500;
}
.text {
  max-width: 0;
  animation: typing 3s steps(var(--characters--)) infinite;
  white-space: nowrap;
  overflow: hidden;
}
.typewriter-effect::after {
  content: ' |';
  animation: blink 1s infinite;
  animation-timing-function: step-end;
}
@keyframes typing {
  75%,
  100% {
    max-width: calc(var(--characters--) * 1ch);
  }
}
@keyframes blink {
  0%,
  75%,
  100% {
    opacity: 1;
  }
  25% {
    opacity: 0;
  }
}
```

javascript 代码如下:

```js
const typeWriter = document.getElementById('typewriter-text');
const createWriter = (text = 'Lorem ipsum dolor sit amet.') => {
  typeWriter.innerHTML = text;
  typeWriter.style.setProperty('--characters--', text.length);
};
createWriter();
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/typewriter-effect.html"></iframe>

## 9. 高度过渡效果

当元素的高度未知时，将元素的高度从 0 转换为 auto,实现思路如下:

- 使用 transition 来指定对 max-height 的更改应该被过渡。
- 使用 overflow:hidden 来防止隐藏元素的内容溢出其容器。
- 使用 max-height 指定初始高度为 0。
- 使用 :hover 伪类将 max-height 更改为 JavaScript 设置的 --max-height 变量的值。
- 使用 Element.scrollHeight 和 CSSStyleDeclaration.setProperty() 将 --max-height 的值设置为元素的当前高度。
- 注意：在每个动画帧上导致重排，如果在高度过渡的元素下方有大量元素，则会出现延迟。

html 代码如下:

```html
<div class="trigger">
  Hover me to see a height transition.
  <div class="el">Additional content</div>
</div>
```

css 代码如下:

```css
.trigger {
  cursor: pointer;
  border-bottom: 1px solid #2396ef;
}
.el {
  transition: max-height 0.4s;
  overflow: hidden;
  max-height: 0;
}
.trigger:hover > .el {
  max-height: var(--max-height--);
}
```

javascript 代码如下:

```js
const el = document.querySelector('.el'),
  height = el.scrollHeight;
el.style.setProperty('--max-height--', height + 'px');
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/height-transition.html"></iframe>

## 10. 开关切换

仅使用 CSS 创建一个切换开关小组件,实现思路如下:

- 使用 for 属性将 `<label>` 与复选框 `<input>` 元素相关联。
- 使用 `<label>` 的 ::after 伪元素为开关创建一个圆形旋钮。
- 使用 :checked 伪类选择器更改旋钮的位置，使用 transform: translateX(20px) 和开关的背景颜色。
- 使用 position: absolute 和 left: -9999px 在视觉上隐藏 `<input>` 元素。

html 代码如下:

```html
<input type="checkbox" id="toggle" class="offscreen checkbox" />
<label for="toggle" class="switch"></label>
```

css 代码如下:

```css
.offscreen {
  position: absolute;
  left: -9999px;
}
.checkbox:checked + .switch::after {
  transform: translateX(20px);
}
.checkbox:checked + .switch {
  background-color: #7983ff;
}
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 20px;
  border-radius: 20px;
  background-color: rgba(0, 0, 0, 0.25);
  transition: all 0.3s;
  cursor: pointer;
}
.switch::after {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  border-radius: 18px;
  background-color: #fff;
  top: 1px;
  left: 1px;
  transition: all 0.3s;
}
```

效果如下所示:

<iframe src="https://eveningwater.github.io/code-segment/codes/css/html/toggle-switch.html"></iframe>
