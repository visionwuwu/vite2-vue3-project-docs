# 业务处理

## 结构化数据展示
使用`el-table`展示结构化数据，配合`el-pagination`做数据分页。

![表格](/images/table.png)

文件组织结构如下：`list.vue`展示列表，`edit.vue`和`create.vue`编辑或创建，内部复用`detail.vue`处理，`model`中负责数据业务处理。

![文件组织结构](/images/user-structor.jpg)

`list.vue`中的数据展示
``` vue
<template>
  <el-table v-loading="loading" :data="list">
    <el-table-column label="ID" prop="id"></el-table-column>
    <el-table-column label="账户名" prop="name"></el-table-column>
    <el-table-column label="年龄" prop="age"></el-table-column>
  </el-table>
</template>
```
`list`和`loading`数据的获取逻辑，可以使用`compsition-api`提取到`userModel.js`
``` js
export function useList() {
  // 列表数据
  const state = reactive({
    loading: true, // 加载状态
    list: [], // 列表数据
  });

  // 获取列表
  function getList() {
    state.loading = true;
    return request({
      url: "/getUsers",
      method: "get",
    }).then(({ data, total }) => {
      // 设置列表数据
      state.list = data;
    }).finally(() => {
      state.loading = false;
    });
  }
  
  // 首次获取数据
  getList();

  return { state, getList };
}

```

`list.vue`中使用
``` js
import { useList } fr

om "./model/userModel";

```
``` js
const { state, getList } = useList();
```
分页处理，`list.vue`
``` vue
<template>
  <pagination
      :total="total"
      v-model:page="listQuery.page"
      v-model:limit="listQuery.limit"
      @pagination="getList"
  ></pagination>
</template>
```
数据也在`userModel`中处理
``` js
const state = reactive({
  total: 0,   // 总条数
  listQuery: {// 分页查询参数
    page: 1,  // 当前页码
    limit: 5, // 每页条数
  },
});

```
``` js
request({
  url: "/getUsers",
  method: "get",
  params: state.listQuery, // 在查询中加入分页参数
})

```

## 表单处理
用户数据新增、编辑使用`el-form`处理

可用一个组件`detail.vue`来处理，区别仅在于初始化时是否获取信息回填到表单。

``` vue
<template>
  <el-form ref="form" :model="model" :rules="rules">
    <el-form-item prop="name" label="用户名">
      <el-input v-model="model.name"></el-input>
    </el-form-item>
    <el-form-item prop="age" label="用户年龄">
      <el-input v-model.number="model.age"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button @click="submitForm" type="primary">提交</el-button>
    </el-form-item>
  </el-form>
</template>
```

数据处理同样可以提取到`userModel`中处理。

``` js
const defaultData = {
  name: '',
  age: undefined
}
export function useItem(isEdit, id) {
  const model = ref(Object.assign({}, defaultData))

  // 初始化时，根据isEdit判定是否需要获取详情
  onMounted(() => {
    if (isEdit && id) {
      request({
        url: '/getUser',
        method: 'get',
        params: { id }
      }).then(({data}) => {
        model.value = data
      })
    }
  })

  return {
    model
  }
}
```