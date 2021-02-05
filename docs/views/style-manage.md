# 样式管理

## 安装scss
``` sh
npm install sass -D
or
yarn add sass -D
```

## style目录保存各种样式
```
|———— styles // 样式目录
|  |———— element-ui.scss // element-ui样式覆盖
|  |———— index.scss // 样式入口文件
|  |———— mixin.scss // 可复用的mixin
|  |———— sidebar.scss // 布局组件sidebar样式
|  |———— transition.scss // 全局过渡动画
|  |———— variables.scss // 全局scss变量
```

## 样式出口index.scss
index.scss作为出口组织这些样式，同时编写一些全局样式
``` scss
@import './variables';
@import './mixin';
@import './transition';
@import './element-ui';
@import './sidebar';
```

## 最后在main.js中导入
``` js
// main.js
import 'styles/index.scss'
```