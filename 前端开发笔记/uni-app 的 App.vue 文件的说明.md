# uni-app 的 App.vue 文件的说明

`App.vue/uvue` 是 uni-app 的主组件。uni-app js 引擎版是 `app.vue`。uni-app x 是 `app.uvue`。

所有页面都是在 `App.vue` 下进行切换的，是应用入口文件。但 `App.vue` 本身不是页面，这里不能编写视图元素，也就是没有`<template>`。

这个文件的作用包括：监听应用生命周期、配置全局样式、配置全局的存储 globalData。

应用生命周期仅可在 `App.vue` 中监听，在页面监听无效。

## 应用生命周期

| 函数名               | 说明                                                                                                    |
| -------------------- | ------------------------------------------------------------------------------------------------------- |
| onLaunch             | 当 uni-app 初始化完成时触发（全局只触发一次），参数为应用启动参数，同 uni.getLaunchOptionsSync 的返回值 |
| onShow               | 当 uni-app 启动，或从后台进入前台显示，参数为应用启动参数，同 uni.getLaunchOptionsSync 的返回值         |
| onHide               | 当 uni-app 从前台进入后台                                                                               |
| onError              | 当 uni-app 报错时触发                                                                                   |
| onUniNViewMessage    | 对 nvue 页面发送的数据进行监听，可参考 nvue 向 vue 通讯                                                 |
| onUnhandledRejection | 对未处理的 Promise 拒绝事件监听函数（2.8.1+ app-uvue 暂不支持）                                         |
| onPageNotFound       | 页面不存在监听函数                                                                                      |
| onThemeChange        | 监听系统主题变化                                                                                        |
| onLastPageBackPress  | 最后一个页面按下 Android back 键，常用于自定义退出                                                      |
| onExit               | 监听应用退出                                                                                            |
