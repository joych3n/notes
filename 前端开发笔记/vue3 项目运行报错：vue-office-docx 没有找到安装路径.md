# vue3 项目运行报错：vue-office/docx 没有找到安装路径

最近遇到一个问题，使用 `pnpm` 安装项目依赖没有报错，但是在打开页面时报错了，提示`vue-office/docx`这个包没有找到安装路径，解决方法有 2 个：

## 方法 1

删除`node_modules`，改用`npm`安装。

## 方法 2

运行`pnpm approve-builds`后再执行`pnpm i`

> 联合创作：陈浩 & 龙俊彪
