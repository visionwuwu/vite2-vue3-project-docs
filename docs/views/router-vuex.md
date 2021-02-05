# 整合vue-router4和vuex4
尤大也对`vue-router`和`vuex`进行了重构，升级到了4.0。让我们来看看`vue-router4`和`vuex4`在`vite`项目中如何使用，和升级之后带来的变化。

## 安装vue-router4
你可以使用`npm`或者`yarn`等包管理工具进行安装我们的`vue-router`，由于现在非正式版你必须在vue-router后面指定`@next`将来在正式版你可以不加`@next`来进行安装。注意这是一个运行时依赖的包安装时加上`-S`。
``` sh
yarn add vue-router@next -S
or
npm install vue-router@next -S
```

## 创建路由实例
一般我们会在存放项目源代码的`src`目录下新建一个叫`router`的目录存放我们页面的所有路由配置，并以`index.js`作为我们路由的入口文件。接下来我们就可以初始化配置路由了：
``` js
import { createRouter, createWebHashHistory } from 'vue-router'

// 使用工厂函数创建路由实例
const router = createRouter({
  // 路由模式为hash
  history: createWebHashHistory(),
  router: [
    {
      path: '/',
      name: 'Home',
      component: () => import('views/home.vue')
    }
  ]
})

export default router
```
为了方便我们创建路由，并初始配置项我在`vscode`的用户自定义代码段创建了一个叫`v3router`的代码片段，专门用来创建vue3版本的路由。你就可以在你的`vscode`中输入`v3router`、回车后就有代码模板。
``` json
{
	// Example:
	"Vue3 Router": {
		"prefix": "v3router",
		"body": [
			"import { createRouter, createWebHistory } from 'vue-router'",
      "",
      "const router = createRouter({",
      "\thistory: createWebHistory(),",
      "\troutes: [",
      "\t\t{ path: '${1:/}', component: ${2:component} }",
      "\t]",
			"})",
			"",
			"export default router",
			""
		],
		"description": "Base for Vue3 Router"
	}
}
```
对比以前的vue-router，以前是以`new VueRouter({...})`的方式来创建路由实例的，现在使用`createRouter()`这个工厂函数创建路由实例。路由模式也发生了改变，以前配置`mode`属性给定`hash`、`history`来指定路由模式为hash模式，或history模式路由，而现在用`createWebHashHistory`或`createWebHistory`指定路由的`hash`或`history`模式。

## 使用路由
路由使用非常简单，其实就是插件的使用在`main.js`文件下，导入我们的`router`实例，在用`app.use(router)`挂载路由。
``` js
import { createApp } from 'vue'
import App from './App.vue'
// 注意这里from 'router'是在vite.config.js中配置了别名
import router from 'router'

const app = createApp(App)

app.use(router)
```
### 测试路由
在`src`目录下新建`views`目录，在其目录下新建`home.vue`文件作为测试页面。打开`chrome`浏览器输入`http://localhost:3000/#/home`访问，就会看到对应页面。注意必须在`App.vue`文件的`template`模板加上`<router-view></router-view>`组件哦~
``` vue
<template>
  <div class="home-page">
    home-page
  </div>
</template>

<script setup>

</script>

<style scoped>

</style>

```

## 安装vuex4
你可以使用`npm`或者`yarn`等包管理工具进行安装我们的`vuex`，由于现在非正式版你必须在vue-router后面指定`@next`将来在正式版你可以不加`@next`来进行安装。注意这是一个运行时依赖的包安装时加上`-S`。
``` sh
yarn add vuex@next -S
or
npm install vuex@next -S
```

## 创建全局store实例
在项目源代码的`src`目录下新建一个叫`store`的目录存放整个应用的全局状态，并以`index.js`作为我们`store`的入口文件。接下来我们就可以创建全局store：
``` js
import { createStore } from 'vuex'

const store = createStore({
  state: {
    counter: 0
  },
  getters: {
    value: state => {
      return state.value;
    }
  },
  mutations: {
    updateValue(state, payload) {
      state.value = payload;
    }
  },
  actions: {
    updateValue({commit}, payload) {
      commit('updateValue', payload);
    }
  }
})

export default store
```
为了方便我们创建路由，并初始配置项我在`vscode`的用户自定义代码段创建了一个叫`v3router`的代码片段，专门用来创建vue3版本的路由。你就可以在你的`vscode`中输入`v3router`、回车后就有代码模板。
``` json
{
	// Example:
	"Print to console": {
		"prefix": "v3store",
		"body": [
			"import { createStore } from 'vuex'",
			"",
			"const store = createStore({",
			"\tstate: {",
			"\t\t${1:counter}: ${2:0}",
			"\t},",
			"\tgetters: {",
      "\t\tvalue: state => {",
      "\t\t\treturn state.value;",
      "\t\t}",
			"\t},",
			"\tmutations: {",
      "\t\tupdateValue(state, payload) {",
      "\t\t\tstate.value = payload;",
      "\t\t}",
			"\t},",
			"\tactions: {",
      "\t\tupdateValue({commit}, payload) {",
      "\t\t\tcommit('updateValue', payload);",
      "\t\t}",
			"\t}",
			"",
			"export default store"
		],
		"description": "Base for Vue3 Store"
	}
}
```
除了用`createStore()`工厂函数创建store，其他的没什么变化。

## 使用store
首先在`main.js`文件中导入`store`，再用`app.use(store)`使用该插件。
``` js
import { createApp } from 'vue'
import App from 'App.vue'
import store from 'store'

const app = createApp(App)

app.use(store)
```
在`home.vue`页面上使用`$store.state.counter`获取我们全局`store`中定义的`counter`变量，在使用`$store.commit('updateValue')`提交一个`mutation`来修改`store`中的`counter`状态。
``` vue
<template>
  <h1> {{ $store.state.counter }} </h1>

  <button @click="$store.commit('updateValue', 2)">Update</button>
</template>
```