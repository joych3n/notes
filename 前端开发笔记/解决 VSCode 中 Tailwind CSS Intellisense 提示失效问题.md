# 解决 VSCode 中 Tailwind CSS Intellisense 提示失效问题

使用 VSCode 开发 Tailwind CSS 项目时，若发现`Tailwind CSS IntelliSense`扩展无法提供智能提示，通常可通过以下步骤快速修复：

1. **打开设置**
   通过快捷键 `Ctrl/Cmd + ,` 进入 VSCode 设置界面，或点击左下角齿轮图标选择“Settings”。

2. **调整`editor.quickSuggestions`配置**
   在搜索栏输入`editor.quickSuggestions`，展开设置选项后找到下方的`strings`子配置项，将其值从默认的`off`改为`on`。此配置允许在字符串（如 HTML 类名）中启用代码提示。修改后关闭并重新打开当前文件（或重启 VSCode）

**原理说明**
Tailwind 的类名提示依赖于对字符串内容的分析。若`strings`选项为`off`，VSCode 默认不在字符串内提供自动补全，导致扩展无法正常触发。
