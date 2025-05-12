# uni-app 的 pages.json 的配置项列表

pages.json 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生 tabbar 等。

| 属性          | 类型         | 必填 | 描述                                     | 平台兼容                 |
| ------------- | ------------ | ---- | ---------------------------------------- | ------------------------ |
| globalStyle   | Object       | 否   | 设置默认页面的窗口表现                   |                          |
| pages         | Object Array | 是   | 设置页面路径及窗口表现                   |                          |
| easycom       | Object       | 否   | 组件自动引入规则                         | 2.5.5+                   |
| tabBar        | Object       | 否   | 设置底部 tab 的表现                      |                          |
| condition     | Object       | 否   | 启动模式配置                             |                          |
| subPackages   | Object Array | 否   | 分包加载配置                             | H5 不支持                |
| preloadRule   | Object       | 否   | 分包预下载规则                           | 微信小程序               |
| workers       | String       | 否   | Worker 代码放置的目录                    | 微信小程序               |
| leftWindow    | Object       | 否   | 大屏左侧窗口                             | H5                       |
| topWindow     | Object       | 否   | 大屏顶部窗口                             | H5                       |
| rightWindow   | Object       | 否   | 大屏右侧窗口                             | H5                       |
| uniIdRouter   | Object       | 否   | 自动跳转相关配置，新增于 HBuilderX 3.5.0 |                          |
| entryPagePath | String       | 否   | 默认启动首页，新增于 HBuilderX 3.7.0     | 微信小程序、支付宝小程序 |
