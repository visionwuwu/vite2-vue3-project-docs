# 基础布局
我们应用需要一个基本布局页，类似下图，将来每个页面以布局页为父页面即可：

![布局](/images/layout.png)

布局页面，`layout/index.vue`
``` vue
<template>
  <div class="app-wrapper">
    <!-- 侧边栏 -->
    <div class="sidebar-container"></div>
    <!-- 内容容器 -->
    <div class="main-container">
      <!-- 顶部导航栏 -->
      <navbar />
      <!-- 内容区 -->
      <app-main />
    </div>
  </div>
</template>

<script setup>
import AppMain from "./components/AppMain.vue";
import Navbar from "./components/Navbar.vue";
</script>

<style lang="scss" scoped>
@import "../styles/mixin.scss";

.app-wrapper {
  @include clearfix;
  position: relative;
  height: 100%;
  width: 100%;
}
</style>

```

::: tip
别忘了创建`AppMain.vue`和`Navbar.vue`
:::

路由配置，`router/index.js`
``` js
{
  path: "/",
	component: Layout,
  children: [
    {
      path: "",
      component: () => import('views/home.vue'),
      name: "Home",
      meta: { title: "首页", icon: "el-icon-s-home" },
    },
  ],
},

```