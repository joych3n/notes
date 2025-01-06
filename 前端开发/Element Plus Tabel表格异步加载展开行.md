# Element Plus Tabel 表格异步加载展开行

## 1、基础用法

当行内容过多并且不想显示横向滚动条时，可以使用 Table 展开行功能。
通过设置 type="expand" 和 slot 可以开启展开行功能， el-table-column 的模板会被渲染成为展开行的内容，展开行可访问的属性与使用自定义列模板时的 slot 相同。

### 代码示例 1

```js
<template>
  <el-table :data="tableData" @expand-change="handleExpandChange">
    <el-table-column type="expand">
      <template #default="props">
          <el-table :data="props.row.family" :border="childBorder">
            <el-table-column label="Name" prop="name" />
            <el-table-column label="State" prop="state" />
            <el-table-column label="Address" prop="address" />
          </el-table>
      </template>
    </el-table-column>
    <el-table-column label="Date" prop="date" />
  </el-table>
</template>
```

## 2、expand-change 事件

当用户对某一行展开或者关闭的时候会触发该事件（第一个参数为当前操作的行，第二个参数为所有已展开的行；）

> ### 官方文档的说明
>
> expand-change 当用户对某一行展开或者关闭的时候会触发该事件（展开行时，回调的第二个参数为 expandedRows；树形表格时第二参数为 expanded）

## 3、异步加载展开行

在 expand-change 事件中，第一次展开时，请求当前行下展开行的数据，并标记当前行为“已展开过”

### 代码示例 2

```js
function handleExpandChange(row: any, expandedRows: []) {
  // expand-change事件获取不到当前事件到底是展开还是关闭，
  // 所以在展开时标记当前行“已展开过”，避免每次展开都请求数据。
  if (!row.isExpanded) {
    row.isExpanded = true;
    getExpandData(row.id).then((res) => {
      row.family = res.data;
    });
  }
}
```
