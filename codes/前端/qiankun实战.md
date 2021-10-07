本文章主要讲述如何接入qiankun微前端。

父类：采用vue。

子类：采用vue以及react的方式接入。

其次：vue是通过 vue cli进行创建， react是通过create-react-app来进行项目创建。

在创建完父类项目以及子类项目之后，我们需要进行进行qiankun项目的安装。

```javascript
npm install qiankun --save
```

在安装完成之后，我们需要对其做相应的处理。分类三部分：

1. 在父类中创建微前端基座

   首先在src下创建一个子应用的配置文件，我们叫做micro_app.js

   ```javascript
   // src/micro_app.js
   
   const microApps = [
       {
         name: 'micro_vue',
         entry: '//localhost:8081/',
         activeRule: '/micro_vue',
         container: '#subapp-viewport', // 子应用挂载的div
         props: {
           routerBase: '/micro_vue' // 下发路由给子应用，子应用根据该值去定义qiankun环境下的路由
         }
       },
       {
         name: 'micro_react',
         entry: '//localhost:10100/',
         activeRule: '/micro_react',
         container: '#subapp-viewport', // 子应用挂载的div
         props: {
           routerBase: '/micro_react'
         }
       }
     ]
     
     export default microApps
   
   ```

   在设置完子应用的配置文件之后，我们需要改写App.vue

   ```javascript
   // 为了简单期间，只是改写了模板文件,
   <template>
     <div id="layout-wrapper">
       <div class="layout-header">头部导航</div>
       <div id="subapp-viewport"></div>
     </div>
   </template>
   ```

   接下来是更改main.js。在main.js中最主要做的事情就是将之前配置的子应用进行注册并且启动qiankun框架服务

   ```javascript
   import Vue from 'vue'
   import App from './App.vue'
   import store from './store'
   import router from './router'
   import { registerMicroApps, start } from 'qiankun';
   import appConfig from './micro_app';
   Vue.config.productionTip = false
   
   new Vue({
     router,
     store,
     render: h => h(App),
   }).$mount('#app');
   
   // 注册子应用
   registerMicroApps(appConfig, {
     beforeLoad: app => {
       console.log('before load app.name====>>>>>', app.name)
     },
     beforeMount: [
       app => {
         console.log('[LifeCycle] before mount %c%s', 'color: green;', app.name);
       },
     ],
     afterMount: [
       app => {
         console.log('[LifeCycle] after mount %c%s', 'color: green;', app.name);
       }
     ],
     afterUnmount: [
       app => {
         console.log('[LifeCycle] after unmount %c%s', 'color: green;', app.name);
       },
     ],
   });
   // 启动qiankun
   start();
   
   
   ```

   讲完父类的基座创建完成之后，接下来就是子应用的注册了。

   2. vue接入qiankun

      首先需要在项目根目录下简历vue.config.js用来改写webpack的相应配置。在src下创建public-path.js。public-path用来改写webpack打包好之后，通过那个路径可以访问到打包后的文件。

      ```javascript
      // vue.config.js
      const { name } = require('../package.json')
      
      module.exports = {
        configureWebpack: {
          output: {
            library: `${name}-[name]`,
            libraryTarget: 'umd',
            jsonpFunction: `webpackJsonp_${name}`,
          }
        },
        devServer: {
          port: 8081, // 在.env中VUE_APP_PORT=7788，与父应用的配置一致
          headers: {
            'Access-Control-Allow-Origin': '*' // 主应用获取子应用时跨域响应头
          }
        }
      }
      
      // public-path.js
      (function() {
        // 用来判断是否运行在乾坤的框架下
          if (window.__POWERED_BY_QIANKUN__) {
            // eslint-disable-next-line no-undef
            __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
          }
        })();
      ```

      修改router下的Index，我们只需要导出对应的路径定义即可，不需要导出对应的vue-router实例，如果直接导出vue-router实例的话，会与我们在父类micro_app.js中设置的routerBase匹配不上。导致渲染子应用失败。

      ```javascript
      import Home from '../views/Home.vue'
      const routes = [
        {
          path: '/',
          name: 'Home',
          component: Home
        },
        {
          path: '/about',
          name: 'About',
          component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
        }
      ]
      export default routes;
      
      ```

      

      接下来就是需要改写main.js。我们既然要用乾坤，那就要提供qiankun的相应的生命周期。并且注册vue-vouter实例。

      ```javascript
      import './public-path' // 注意需要引入public-path
      import Vue from 'vue'
      import App from './App.vue'
      import routes from './router'
      import store from './store'
      import VueRouter from 'vue-router'
      Vue.use(VueRouter)
      Vue.config.productionTip = false
      let instance = null
      window.projectName = 'micro_vue';
      function render (props = {}) {
        // 这个是我们在父类注册的时候定义的那些参数。
        const { container, routerBase } = props
        const router = new VueRouter({
          base: window.__POWERED_BY_QIANKUN__ ? routerBase : process.env.BASE_URL,
          mode: 'history',
          routes
        })
        instance = new Vue({
          router,
          store,
          render: (h) => h(App)
        }).$mount(container ? container.querySelector('#app') : '#app')
      }
      
      if (!window.__POWERED_BY_QIANKUN__) {
        render()
      }
      
      export async function bootstrap () {
        console.log('[vue] vue app bootstraped')
      }
      
      export async function mount (props) {
        console.log('[vue] props from main framework', props)
      
        render(props)
      }
      
      export async function unmount () {
        instance.$destroy()
        instance.$el.innerHTML = ''
        instance = null
      }
      
      ```

      最后需要在根目录下创建.env文件。

      ```javascript
      VUE_APP_PORT=8081
      ```

      以上步骤完成之后，我们只需要启动项目我们既可以通过对应的路由进行访问了`http://localhost:xxxx/micro_vue`

      ![image-20210925151551862](https://gitee.com/ByeL/blogimg/raw/master/img/20210925151551.png)

      3. react项目的接入

         在src下创建public-path.js文件

         ```javascript
         if (window.__POWERED_BY_QIANKUN__) {
            // eslint-disable-next-line no-undef
            __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
           }
         ```

         接下来修改`index.js`

         首先我们需要引入`react-router-dom`

         ```javascript
         // npm install react-router-dom --save
         import {BrowserRouter as Router} from 'react-router-dom'
         // 接下来我们需要对render进行改写
         function render(props) {
           const { container } = props;
           ReactDOM.render(
           <Router basename={window.__POWERED_BY_QIANKUN__ ? '/micro_react' : '/'}>
             <App />
           </Router>, container ? container.querySelector('#root') : document.querySelector('#root'));
         }
         // 然后加入生命周期即可
         export async function bootstrap() {
           console.log("ReactMicroApp bootstraped");
         }
         
         /**
          * 应用每次进入都会调用 mount 方法，通常我们在这里触发应用的渲染方法
          */
         export async function mount(props) {
           console.log("ReactMicroApp mount", props);
           render(props);
         }
         
         /**
          * 应用每次 切出/卸载 会调用的方法，通常在这里我们会卸载微应用的应用实例
          */
         export async function unmount() {
           console.log("ReactMicroApp unmount");
           ReactDOM.unmountComponentAtNode(document.getElementById("root"));
         }
         ```

         

         修改 `webpack` 配置

         方法1： 

         运行 `npm run eject`

         接下来 修改`config/webpackDevServer.config.js`

         ```javascript
         module.exports = function (proxy, allowedHost) {
           return {
             headers: {
               'Access-Control-Allow-Origin': '*', // 表示允许跨域
             },
             ...
         };
         ```

         接下来修改`config/webpack.config.js`

         ```javascript
         return {
             ...
             output: {
               // The build folder.
               ...
               // 微应用配置
               library: `${appPackageJson.name}-[name]`,
               libraryTarget: 'umd'
             },
             ...
         };
         ```

         方法二： 

         安装插件 `@rescripts/cli`，当然也可以选择其他的插件，例如 `react-app-rewired`。

         ```dash
         npm i -D @rescripts/cli
         ```

         根目录新增 `.rescriptsrc.js`：

         ```javascript
         
         
         const { name } = require('./package');
         
         module.exports = {
           webpack: (config) => {
             config.output.library = `${name}-[name]`;
             config.output.libraryTarget = 'umd';
             config.output.jsonpFunction = `webpackJsonp_${name}`;
             config.output.globalObject = 'window';
         
             return config;
           },
         
           devServer: (_) => {
             const config = _;
         
             config.headers = {
               'Access-Control-Allow-Origin': '*',
             };
             config.historyApiFallback = true;
             config.hot = false;
             config.watchContentBase = false;
             config.liveReload = false;
         
             return config;
           },
         };
         ```

         修改 `package.json`

         ```json
         -   "start": "react-scripts start",
         +   "start": "rescripts start",
         -   "build": "react-scripts build",
         +   "build": "rescripts build",
         -   "test": "react-scripts test",
         +   "test": "rescripts test",
         -   "eject": "react-scripts eject"
         ```

         

         最后，同理在根目录下创建一个.env文件，

         ```javascript
         PORT=10100
         BROWSER=none
         ```

         然后启动react 项目，之后就可以通过`http://localhost:xxx/micro_react`进行访问了

         ![image-20210925150249912](https://gitee.com/ByeL/blogimg/raw/master/img/20210925150250.png)

      

      ### 数据交互

      qiankun内部使用initGlobalState(state)定义全局状态，该方法执行后返回一个MicroAppStateActions实例，实例中包含三个方法，分别是onGlobalStateChange、setGlobalState、offGlobalStateChange。

      默认会通过props将通信方法传递给子应用。

      **onGlobalStateChange：** 当global有数据发生更改的时候，就会触发回调函数，回调函数传入的是更改后的globalState

      **setGlobalState:** 更改globalstate的方法

      **offGlobalStateChange：**移除监听，微应用执行 umount 时会默认调用。

      

      如何使用：

      首先我们在父类基座的src下建议一个文件为`action.js`文件。

      ```javascript
      import { initGlobalState } from "qiankun"; 
      import store from "./store";
      
      const initialState = {
        //这里写初始化数据
        userName: 'testInfo',
        userId: '1232',
      };
      
      // 初始化 state
      const actions = initGlobalState(initialState);
      actions.onGlobalStateChange((state, prev) => {//监听公共状态的变化
        console.log("主应用: 变更前");
        console.log(prev);
        console.log("主应用: 变更后");
        console.log(state);
      });
      export default actions;
      ```

      在父类如何获取初始的`GlobalState`:

      ```javascript
      // 当onGlobalStateChange的最后一个属性为true的时候，表示立即获取，不需要等待数据发生变更
      action.onGlobalStateChange((state) => { 
        console.log('全局状态发生改变');
        // this.mes = state
        this.userInfo.userName = state.userName;
        console.log(state);
      }, true);
      ```

      在父类中更改`GlobalState`:

      ```javascript
      action.setGlobalState({
        userName: '更改之后的名称',
        userId: 'new Id'
      });
      ```

      如何在子类中进行使用：

      首先在子类中src下创建一个action.js

      ```javascript
      function emptyAction() {  //设置一个actions实例
          // 提示当前使用的是空 Action
          console.warn("Current execute action is empty!");
      }
      
      class Actions {
          // 默认值为空 Action
          actions = {
              onGlobalStateChange: emptyAction,
              setGlobalState: emptyAction,
          };
      
          /**
           * 设置 actions
           */
          setActions(actions) {
              this.actions = actions;
          }
      
          /**
           * 映射
           */
          onGlobalStateChange(...args) {
              return this.actions.onGlobalStateChange(...args);
          }
      
          /**
           * 映射
           */
          setGlobalState(...args) {
              return this.actions.setGlobalState(...args);
          }
      }
      
      const actions = new Actions();
      export default actions;
      ```

      之后我们在子类的main.js中接受props上传递过来的方法

      ```javascript
      export async function mount (props) {
        console.log('[vue] props from main framework', props)
        actions.setActions(props);
        render(props)
      }
      ```

      之后的获取数据，以及更新与父类的相同。