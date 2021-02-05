# Vue JSX支持

## 安装支持jsx插件
``` sh
npm i @vitejs/plugin-vue-jsx
```

## 配置插件
在vite.config.js中导入插件并初始化插件
``` js
import vueJsx from '@vitejs/plugin-vue-jsx'
/**
 * @type {import('vite').UserConfig}
 */
export default defineConfig({
  plugins: [
    vueJsx()
  ]
})
```

## Vue2中render函数编写jsx
``` vue
<script lang="jsx">
// Comp.vue
export default {
  data() {
    return {
      count: 0
    }
  },
  render () {
    return <>
      <h1>Test</h1>
      <p onClick={this.onClick}>{this.count}</p>
    </>
  },
  methods: {
    onClick() {
      this.count++
    }
  }
}
</script>
```

## Vue3中setup函数编写jsx
``` vue
<script lang="jsx">
// Comp.vue
import { ref, defineComponent } from "vue";
export default defineComponent({
  setup () {
    const count = ref(0)
    return () => (
      <>
        <h1>Count: {count.value}</h1>
        <button onClick={() => count.value++}>+1</button>
      </>
    )
  }
})
</script>
```
