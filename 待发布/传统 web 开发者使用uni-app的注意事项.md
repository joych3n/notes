# 传统 web 开发者使用 uni-app 的注意事项

你之前可能习惯自由的 web 开发，但目前各家小程序都有很多限制。 当然限制是为了在框架层更好的优化用户体验，所以小程序的体验要优于 web。 并且这些限制只是写法的限制，并不影响功能。 如果你做过微信小程序开发，对这些限制应该已经很了解了。如果没有做过小程序，请仔细阅读本节。

1. JS 注意

- 非 H5 端，不能使用浏览器自带对象，比如 document、window、localstorage、cookie 等，更不能使用 jquery 等依赖这些浏览器对象的框架。因为各家小程序快应用都不支持这些对象。
- 没有这些浏览器自带对象并不影响业务开发，uni 提供的 api 足够完成业务。
- uni 的 api 在编译到 web 平台运行时，其实也会转为浏览器的 js api。
- App 端若要使用操作 window、document 的库，需要通过 renderjs 来实现。
- uni 的 api 是多端可用的。在条件编译区，每个平台的专有 api 也可以使用，比如 wx.、plus.等 api 可以分别在微信下和 app 下使用。
- 出于降低小程序向 uni-app 迁移成本的考虑，wx 的 api 在 app 里也可以直接运行，比如写 wx.request 和 uni.request 是一样的，但仍然建议仅在微信的条件编译区使用 wx 的 api。

2. Tag 注意

- uni-app 的 tag 同小程序的 tag，和 HTML 的 tag 不一样，比如 div 要改成 view，span 要改成 text、a 要改成 navigator。
- 出于降低 h5 应用向 uni-app 迁移成本的考虑，写成 div、span 也可以运行在 app 和小程序上，因为 uni-app 编译器会把这些 HTML 标签编译为小程序标签。但仍然建议养成新习惯。

3. Css 注意

- 虽然大部分 css 样式在微信小程序和 app 中都可以支持，但推荐使用 flex 布局模型，这种布局更灵活高效且支持更多平台(比如 nvue、快应用只支持 flex 布局)
- 单位方面，uni-app 默认为 rpx。这是一种可跨端的通用单位。

4. 工程目录注意

- 页面文件：放到 pages 目录下；推荐方案：新建一个页面目录，然后创建一个目录同名的.vue 文件，如/pages/list/list.vue，接着在 pages.json 里完成注册。这与小程序的策略相同。
- 自定义组件：放到 component 目录
- 静态资源：如图片，固定放到 static 目录下。这是 webpack 的规则

5. 数据绑定方式的注意

- uni-app 基于 Vue 2.0 实现，开发者需注意 Vue 1.0 -> 2.0 的使用差异。

6. 每个页面支持使用原生 title，首页支持使用原生底部 tab，这些是要在 pages.json 里配置，这些并不是 vue 页面的一部分。当然 vue 里的 js api 也可以动态修改原生 title
7. 虽然使用 vue，但在 app 和小程序里，不是 spa 而是 mpa
8. 位置坐标系统一为国测局坐标系 gcj02，这种坐标系可以被多端支持。老版 5+的百度定位和百度地图使用的是百度私有坐标系 bd09ll，这种坐标系需要转换。新版 uni-app 里的百度地图已经默认改为 gcj02。高德地图不受影响，一直是 gcj02
