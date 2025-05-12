# uni-app 组成和跨端原理

## 基本语言和开发规范

为了实现多端兼容，综合考虑编译速度、运行性能等因素，uni-app 约定了如下开发规范：

- 页面文件遵循 Vue 单文件组件 (SFC) 规范，即每个页面是一个.vue 文件
- 组件标签靠近小程序规范，详见 uni-app 组件规范
- 接口能力（JS API）靠近小程序规范，但需将前缀 wx、my 等替换为 uni，详见 uni-app 接口规范
- 数据绑定及事件处理同 Vue.js 规范，同时补充了应用生命周期及页面的生命周期
- 如需兼容 app-nvue 平台，建议使用 flex 布局进行开发
- uni-app 分编译器和运行时（runtime）。uni-app 能实现一套代码、多端运行，是通过这 2 部分配合完成的。

编译器将开发者的代码进行编译，编译的输出物由各个终端的 runtime 进行解析，每个平台（Web、Android App、iOS App、各家小程序）都有各自的 runtime。

## 编译器

- 编译器运行在电脑开发环境。一般是内置在 HBuilderX 工具中，也可以使用独立的 cli 版。
- 开发者按 uni-app 规范编写代码，由编译器将开发者的代码编译生成每个平台支持的特有代码
- 在 web 平台，将.vue 文件编译为 js 代码。与普通的 vue cli 项目类似
- 在微信小程序平台，编译器将.vue 文件拆分生成 wxml、wxss、js 等代码
- 在 app 平台，将.vue 文件编译为 js 代码。
- 编译器分 vue2 版和 vue3 版
- 编译器支持条件编译，即可以指定某部分代码只编译到特定的终端平台。从而将公用和个性化融合在一个工程中。

## 运行时（runtime）

runtime 不是运行在电脑开发环境，而是运行在真正的终端上，包括 3 部分：基础框架、组件、API。

uni-app 在每个平台（Web、Android App、iOS App、各家小程序）都有各自的 runtime。这是一个比较庞大的工程。

- 在小程序端，uni-app 的 runtime，主要是一个小程序版的 vue runtime，页面路由、组件、api 等方面基本都是转义。
- 在 web 端，uni-app 的 runtime 相比普通的 vue 项目，多了一套 ui 库、页面路由框架、和 uni 对象（即常见 API 封装）
- 在 App 端，uni-app 的 runtime 更复杂，可以先简单理解为 DCloud 也有一套小程序引擎，打包 app 时将开发者的代码和 DCloud 的小程序打包成了 apk 或 ipa。当然，事实上 DCloud 确实有小程序引擎产品，供原生应用实现小程序化，详见

## 逻辑层和渲染层分离

在 web 平台，逻辑层（js）和渲染层（html、css），都运行在统一的 webview 里。

但在小程序和 app 端，逻辑层和渲染层被分离了。

分离的核心原因是性能。过去很多开发者吐槽基于 webview 的 app 性能不佳，很大原因是 js 运算和界面渲染抢资源导致的卡顿。

不管小程序还是 app，逻辑层都独立为了单独的 js 引擎，渲染层仍然是 webview（app 上也支持纯原生渲染）。

所以注意小程序和 app 的逻辑层都不支持浏览器专用的 window、dom 等 API。app 只能在渲染层操作 window、dom，即 renderjs。
