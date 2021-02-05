# 整合element3

## 安装element3
``` sh
npm i element3 -S
or
yarn add element3 -S
```

## 全局引入element3

> 在main.js中全局引入element3

1. **在main.js中全局引入element3，和全局样式**
``` js
// main.js
import element3 from 'element3'
import 'element3/lib/theme-chalk/index.css'
```

2. **组件库以插件的形式整合到vue中**
``` js
// main.js
create(App).use(element3)
```
> 以插件的形式全局引入element3

1. **在src目录下新建一个plugins/element3.js文件**

2. **全局引入element3的方式剪切过来，并导出一个函数**
``` js
// element3.js
import element3 from 'element3'
import 'element3/lib/theme-chalk/index.css'

export default function (app) {
  // 完整
  app.use(element3)
}
```

3. **修改vite.config.js文件的alias选项**
``` js
// vite.config.js
// 新增下面一个别名选项
export default defineConfig({
  alias: {
    plugins: path.resolve(__dirname, 'src/plugins')
  }
})
```

4. **在main.js中导入这个插件**
``` js
import element3 from 'plugins/element3'
```

::: tip 
- 全局引入带来的问题，有些组件在项目中不会使用，浪费打包的体积。
- 如果在mian.js中按需引入会使当前文件变得冗长，难以维护。
:::

## 按需引入element3

1. **element3.js文件下将全局引入方式删除或注释**

2. **按需加载**
``` js
// element3.js
import { ElButton, ElInput } from 'element3'
import 'element3/lib/theme-chalk/button.css'
import 'element3/lib/theme-chalk/input.css'

export default function (app) {
  app.use(ElButton)
  app.use(ElInput)
}
```

3. **接下来就可以愉快使用组件啦😄**

::: tip
具体引入各个组件参考element3官方文档 [element3-ui](https://element3-ui.com/#/component/quickstart)
:::
