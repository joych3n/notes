# Vue3 项目警告[@vue/compiler-sfc] ::v-deep usage as a combinator has been deprecated. Use :deep(＜ inner-selector ＞)

这个警告的意思是::v-deep 作为组合符已被弃用，使用:v-deep()

将项目中`::v-deep`改为`:v-deep()`写法，例如：

```css
::v-deep .el-button {
  background-color: red;
}
```

改为

```css
:deep(.el-button) {
  background-color: red;
}
```
