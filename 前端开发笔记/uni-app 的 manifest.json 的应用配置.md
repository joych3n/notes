# uni-app 的 manifest.json 应用配置项列表

| 属性                   | 类型                   | 默认值                        | 描述                                 |
| ---------------------- | ---------------------- | ----------------------------- | ------------------------------------ |
| name                   | String                 |                               | 应用名称                             |
| appid                  | String                 | 新建项目时，DCloud 云端分配。 | 应用标识                             |
| description            | String                 |                               | 应用描述                             |
| locale                 | String                 | auto                          | 设置当前默认语言，具体参考 locale    |
| versionName            | String                 |                               | 版本名称，例如：1.0.0。              |
| versionCode            | Number                 |                               | 版本号，例如：36                     |
| transformPx            | Boolean                | true                          | 此选项已废弃                         |
| networkTimeout         | Object                 |                               | 网络超时时间，详见                   |
| debug                  | Boolean                | false                         | 是否开启 debug 模式                  |
| uniStatistics          | Object                 |                               | 是否开启 uni 统计，全局配置          |
| sassImplementationName | dart-sass 或 node-sass |                               | 使用的 scss 预编译库，仅限 vue2 项目 |
| app-plus               | Object                 |                               | App 特有配置                         |
| h5                     | Object                 |                               | H5 特有配置                          |
| quickapp               | Object                 |                               | 快应用特有配置，即将支持             |
| mp-weixin              | Object                 |                               | 微信小程序特有配置                   |
| mp-alipay              | Object                 |                               | 支付宝小程序特有配置                 |
| mp-baidu               | Object                 |                               | 百度小程序特有配置                   |
| mp-toutiao             | Object                 |                               | 抖音小程序特有配置                   |
| mp-lark                | Object                 |                               | 飞书小程序特有配置                   |
| mp-qq                  | Object                 |                               | qq 小程序特有配置                    |
| mp-kuaishou            | Object                 |                               | 快手小程序特有配置                   |
