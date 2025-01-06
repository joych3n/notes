# 解决 Element Plus 的 Table 组件暴露的方法 toggleRowExpansion 不生效

我们公司的管理后台使用了 Element Plus 的 Tabel 组件，表格有拓展功能，需要在搜索后折叠已拓展的行，于是使用 Tabel 组件暴露的方法 toggleRowExpansion，官方文档的说明如下：

```js
//	用于可扩展的表格或树表格，如果某行被扩展，则切换。
//  使用第二个参数，您可以直接设置该行应该被扩展或折叠。
toggleRowExpansion(row: any, expanded?: boolean) => void
```

奇怪的是，这个方法可以拓展，但是不能折叠，也就是说，第二个参数`expanded`为`true`时，表格会拓展，但是为`false`时，已拓展的行不会折叠。

解决的方法是使用`nextTick()`:

```javascript
nextTick(() => {
  tableData.value.forEach((item: any) => {
    myTableRef.value.toggleRowExpansion(item, false);
  });
});
```
