# chrome/Edge 控制台出现很多与项目无关的日志

最近在开发一个 Vue3 项目，每次刷新页面，Chrome 控制台会打印了很多莫名其妙的日志，经过仔细分析，发现这些日志都不是 Vue3 项目产生的。在 Google、百度、ChatGPT 都搜了，也没找到原因。通过反复查看日志内容，加上 ChatGPT 提出的几个可能原因，终于揭开真相了，是 Chrome 安装的拓展导致的。

比如我的 Chrome 安装了“图片批量下载器 - Imageye”，会打印图片 src 被设置的相关信息，在控制台打印几个屏幕高度。还安装了“Spotify Ad Blocker - Blockify”，会打印“cssjs、enabled、go、a、cssjs、uBOL: Generic cosmetic filtering stopped because no more DOM changes”。啰嗦这么多，只是为了遇到同样问题的人，通过搜索引擎能找到这里。
