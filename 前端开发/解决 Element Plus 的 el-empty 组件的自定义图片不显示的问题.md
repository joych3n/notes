# 解决 Element Plus 的 el-empty 组件的自定义图片不显示的问题

Element Plus 的 el-empty 组件有自定义图片属性，如果设置本地图片路径，可能会无法正常显示。可以使用 require 方式：

```html
<el-empty
  :image="require('/src/assets/images/no-result.png')"
  description="很抱歉，没有相关信息！"
>
</el-empty>
```

如果使用 require 方式报错：`Uncaught (in promise) ReferenceError: require is not defined`，建议使用插槽方式：

```html
<el-empty description="很抱歉，没有相关信息！">
  <template #image>
    <img src="/src/assets/images/no-result.png" alt="Custom Image" />
  </template>
</el-empty>
```
