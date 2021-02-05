# mock数据
在vite项目中想要使用mockjs，来mock数据我们可以安装一个插件[vite-plugin-mock](https://github.com/vbenjs/vite-plugin-mock/blob/HEAD/README.zh_CN.md)

## 安装mockjs
``` sh
npm i mockjs -D
or
yarn add mockjs -D
```

## 安装vite-plugin-mock
``` sh
npm i vite-plugin-mock cross-env -D
or
yarn add vite-plugin-mock cross-env -D
```

## 在vite.config.js导入，初始化插件
``` js
// vite.config.js
import { viteMockServer } from 'vite-plugin-mock'
/**
 * @type {import('vite').UserConfig}
 */
export default defineConfig({
  plugins: [
    viteMockServer({
      supportTs: false // 无需支持ts，否则将无法监听js文件
    })
  ]
})
```

## 配置cross-env
``` json
// package.json
{
  "script": "cross-env NODE_ENV=development vite"
}
```

## 使用mock数据
1. 在项目根目录下新建mock目录，新建一个user.js，我们可以参照官方github下的examples，将其拷贝到我们的项目中修改下
``` js
// user.js
export default [
  {
    url: '/api/getUsers',
    method: 'get',
    response: () => {
      return {
        code: 0,
        message: 'ok',
        data: ['yan', 'wu'],
      };
    },
  },
];
```

2. 使用fech发送请求获取mock数据
``` js
// 请求mock api
fetch('/api/getUsers')
  .then(res => res.json())
  .then(data => {
    console.log(data)
  })
```

3. 这样就能在vite项目中mock数据了，不用等后端同学，或自己用nodejs返回数据了，nice。
