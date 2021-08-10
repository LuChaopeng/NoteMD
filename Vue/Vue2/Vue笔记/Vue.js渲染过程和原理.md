1. **JS入口文件** `./build/webpack.base.conf.js`是webpack打包的主要配置文件，其中的代码

   ```javascript
   module.exports = {
   entry:{
       app:`./src/main.js`			//这里定义了Vue.js的入口文件
   	}
   }
   ```

2. **静态的HTML页面**

   打开的页面http://localhost/#/，实际上打开的是 http://localhost/#/index.html。

   其中的`<div id="app"></div>`就是将来会动态变化的内容（同时要知道这之外的内容不会动态变化，需要刷新）。这个index.html就是最外层的模板。

3. **main.js中的Vue定义**

   在入口文件 src/main.js 中，

   ```javascript
   new Vue({
       el:'#app',					//代表着index.html中的<div id='app'>
       router,
       template:'<App/>',
       components:{App}			//main.js将加载 App.vue ，就是./src/App.vue
   })
   ```

   App.vue中有代码

   ```html
   <template>
     <div id="app">
       <img src="./assets/logo.png">
       <router-view/>
     </div>
   </template>
   ```

   这个`<template>`就是第二层模板，页面的内容就是在这个位置渲染出来的。

   **注意：**所有`<router-view>`中的内容会被自动替换。***[这里仍然存在疑问]***

   

