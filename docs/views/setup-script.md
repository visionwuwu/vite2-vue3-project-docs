# Vue3 Setup Script详解

1. **直接导入组件**
``` js
import Comp from 'comps/Comp.vue'
```

2. **属性定义**
``` js
const props = defineProps({
  msg: String
})
console.log(props)
```

3. **获取上下文**
``` js
const ctx = useContext()
console.log(ctx)
// 对外暴露属性
ctx.expose({
  someMethod() {
    console.log('some message')
  }
})
```

4. **定义事件**
``` js
const emit = defineEmit(['myClick'])
```