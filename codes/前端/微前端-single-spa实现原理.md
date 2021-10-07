我们在之前的文档中了解到，目前的微前端采用的是`组合式应用路由分发`。通过父类对路由的检测，动态的对子应用进行卸载或挂载。

在咱们理解原理之前，先看一下如何使用single-spa。我们以vue为例。

首先我们定义两个子项目，以及一个父类基座。

1. 在子类项目中引入`single-spa-vue`

   ```javascript
   npm install single-spa-vue --save
   ```

   

2. 在子类的`main.js`中加入`single-spa-vue`相应的生命周期

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import singleSpaVue from 'single-spa-vue'

Vue.config.productionTip = false

const appOptions = {
  el: '#microApp',
  router,
  render: h => h(App)
}

// 支持应用独立运行、部署，不依赖于基座应用
if (!process.env.isMicro) {
  delete appOptions.el
  new Vue(appOptions).$mount('#app')
}
// 基于基座应用，导出生命周期函数
const vueLifecycle = singleSpaVue({
  Vue,
  appOptions
})
// 启动生命周期
export function bootstrap (props) {
  console.log('app2 bootstrap')
  return vueLifecycle.bootstrap(() => {})
}
// 挂载生命周期
export function mount (props) {
  console.log('app2 mount')
  return vueLifecycle.mount(() => {})
}
// 卸载生命周期
export function unmount (props) {
  console.log('app2 unmount')
  return vueLifecycle.unmount(() => {})
}

```

3. 新建vue.config.js文件，并设置导出格式为`umd`

   在这里需要补充说明的就是webpack配置中的`path`与`publicPath`。path它指的是文件到最后要输出到某个目录下。必须是绝对路径。例如：`path: './dist'`那就是指定输出到dist目录下。`publicPath`指的是默认公共前缀。例如上面我们将项目输出到了dist目录，然后我们在项目中引用了图片，例如：`background-image:url(’./people.png’);`那么因为我们线上打包之后，可能都将图片放到了`dist/image`下，如果还是使用上面那个url的话，肯定是访问不到的，因此publicPath就是相当于给这些请求加前缀。例如我们设置`publicPath`为`dist/image`那么此时就可以正常访问图片了。

   ```javascript
   const package = require('./package.json')
   module.exports = {
     // 告诉子应用在这个地址加载静态资源，否则会去基座应用的域名下加载
     publicPath: '//localhost:8082',
     // 开发服务器
     devServer: {
       port: 8082
     },
     configureWebpack: {
       // 导出umd格式的包，在全局对象上挂载属性package.name，基座应用需要通过这个全局对象获取一些信息，比如子应用导出的生命周期函数
       output: {
         // library的值在所有子应用中需要唯一
         library: package.name,
         libraryTarget: 'umd'
       }
     }
   ```

   经过以上几步操作，一个微前端的子项目基本就创建完成了。

   接下来我们继续来创建父类基座。

   1. 父类基座引入`single-spa`

      ```javascript
      npm install single-spa --save
      ```

   2. 改造父类`main.js`

      ```javascript
      import Vue from 'vue'
      import App from './App.vue'
      import router from './router'
      import { registerApplication, start } from 'single-spa'
      
      Vue.config.productionTip = false
      
      // 远程加载子应用
      function createScript(url) {
        return new Promise((resolve, reject) => {
          const script = document.createElement('script')
          script.src = url
          script.onload = resolve
          script.onerror = reject
          const firstScript = document.getElementsByTagName('script')[0]
          firstScript.parentNode.insertBefore(script, firstScript)
        })
      }
      
      // 记载函数，返回一个 promise
      function loadApp(url, globalVar) {
        // 支持远程加载子应用
        return async () => {
          // 
          await createScript(url + '/js/chunk-vendors.js')
          await createScript(url + '/js/app.js')
          // 这里的return很重要，需要从这个全局对象中拿到子应用暴露出来的生命周期函数
          return window[globalVar]
        }
      }
      
      // 子应用列表
      const apps = [
        {
          // 子应用名称
          name: 'app1',
          // 子应用加载函数，是一个promise
          app: loadApp('http://localhost:8083', 'app1'),
          // 当路由满足条件时（返回true），激活（挂载）子应用
          activeWhen: location => location.pathname.startsWith('/app1'),
          // 传递给子应用的对象
          customProps: {}
        },
        {
          name: 'app2',
          app: loadApp('http://localhost:8082', 'app2'),
          activeWhen: location => location.pathname.startsWith('/app2'),
          customProps: {}
        }
      ]
      
      // 注册子应用
      for (let i = apps.length - 1; i >= 0; i--) {
        registerApplication(apps[i])
      }
      
      new Vue({
        router,
        mounted() {
          // 启动
          start()
        },
        render: h => h(App)
      }).$mount('#app')
      ```

      经过上面改造完成之后呢，一个父类基座应用也就创建完毕了。

      那么我们对比一下`main.js`与改造后的区别

      ![image-20210920143550690](https://gitee.com/ByeL/blogimg/raw/master/img/20210920143550.png)

   我们通过比较可以看出，我们在父类基座中总共新加了这么几个方法或属性。分别是：

   `loadApp, createScript, apps`。从具体的实现中，我们可看出:

   1. loadApp主要是根据资源地址去请求资源
   2. createScript主要是将请求回来的script添加到页面中。
   3. apps就是一些子类应用的描述。
   4. registerApplication就是将这些子应用注册到`single-spa`中。

   对比完父类基座的不同，我们再来对比子类的不同吧。

   ![image-20210920144047313](https://gitee.com/ByeL/blogimg/raw/master/img/20210920144047.png)

   我们可以看出，子类最大的不同就是添加了一些`single-spa-vue`的生命周期。因为微前端的方案，是`single-spa`的，想用的话，就必须导入这些个生命周期。

   那么看了对比之后，我们在看一张图，就能对微前端的原理豁然开朗。

   ![2155778-ec1fb1f2f34e459f](https://gitee.com/ByeL/blogimg/raw/master/img/20210920144336.png)

从图上，我们可以看出浏览器首次打开父类应用的时候，第一步就是先使用调用`registerApplication`注册app，接下来判断当前的路由是属于哪一个子应用的，他的判断依据就是apps中的`activeWhen`，接下来就会将当前的子应用划分状态，`appToLoad,appToUnmounted, appToMounted`。接下来根据子应用的状态，先去执行需要卸载的子应用，卸载完成之后，就会去执行状态为`appToLoad, appToMounted`的子应用，那么在最后在执行相应的回调函数，也就是我们在子应用中注册的那些生命周期。

那么我们有`single-spa`这种微前端解决方案，为什么还需要`qiankun`呢？

相比于`single-spa`，`qiankun`他解决了JS沙盒环境，不需要我们自己去进行处理。在`single-spa`的开发过程中，我们需要自己手动的去写调用子应用JS的方法（如上面的 createScript方法），而`qiankun`不需要，乾坤只需要你传入响应的apps的配置即可，会帮助我们去加载。还有一个就是他们本质的区别

那么在说这个本质的区别之前呢，我们需要了解一下`组合式应用路由分发`他也分为两种解决方案，一种是`JS entry`，另外一种是`html entry`

JS Entry 的方式通常是子应用将资源打成一个 entry script，但这个方案的限制也颇多，如要求子应用的所有资源打包到一个 js bundle 里，包括 css、图片等资源。除了打出来的包可能体积庞大之外的问题之外，资源的并行加载等特性也无法利用上。

HTML Entry 则更加灵活，直接将子应用打出来 HTML 作为入口，主框架可以通过 fetch html 的方式获取子应用的静态资源，同时将 HTML document 作为子节点塞到主框架的容器中。这样不仅可以极大的减少主应用的接入成本，子应用的开发方式及打包方式基本上也不需要调整，而且可以天然的解决子应用之间样式隔离的问题(后面提到)。

以上就是他们之间的区别。简要概括：qiankun基于single-spa，在single-spa上做了改造，使得接入更加方便。

