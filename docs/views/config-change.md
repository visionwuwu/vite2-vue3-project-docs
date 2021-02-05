# Vite2配置变化
对于我们之前项目影响较大的标记如下：
- 配置选项的变化：vue特有选项、创建选项、css选项、jsx选项等
- 别名行为变化：不在要求/开头或结尾
- Vue支持：通过@vitejs/plugin-vue插件支持
- React支持
- HMR API变化
- 清单格式变化
- 插件API重新设计

## Vue支持
Vue的整合也通过插件实现，和其他框架一视同仁
``` js
import vue from '@vitejs/plugin-vue'
/**
 * @type {import('vite').UserConfig}
 */
export default defineConfig({
  plugins: [vue()]
})
```

## 配置选项的变化
全局选项分割开来，有用的选项是alias、css选项esbuild

## alias别名配置
``` js
import { defineConfig } from 'vite'
/**
 * @type {import('vite').UserConfig}
 */
export config defineConfig({
  alias: {
    '@': path.resolve(__dirname, './src'),
    'apis': path.resolve(__dirname, './src/apis'),
    'assets': path.resolve(__dirname, './src/assets'),
    'comps': path.resolve(__dirname, './src/components'),
    'views': path.resolve(__dirname, './src/views'),
    'utils': path.resolve(__dirname, './src/utils'),
    'styles': path.resolve(__dirname, './src/styles'),
    'router': path.resolve(__dirname, './src/router'),
  }
})
```

## 条件配置
``` js
export default ({ command, mode }) => {
  if (command === 'serve') {
    return {
      // serve specific config
    }
  } else {
    return {
      // build specific config
    }
  }
}
```

