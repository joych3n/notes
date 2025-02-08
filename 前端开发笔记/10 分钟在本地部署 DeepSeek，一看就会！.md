10 分钟在本地部署 DeepSeek，一看就会！

最近国产 AI 大模型 DeepSeek 爆火出圈，登顶中美 App Store 下载榜，还在性能、成本上碾压了 ChatGPT 和 Google Gemini 等硅谷巨头，直接杀入科技圈 C 位，成为现象级应用！

DeepSeek 是一个开源模型，今天就跟大家分享一下，如何将 DeepSeek 部署在你的电脑上。

## 第 1 步：Ollama 环境搭建

1、进入 Ollama 官网（https://ollama.com），下载适合自己系统的版本（Windows/mac/Linux）。

![下载ollama](/img/deepseek1.png)

2、下载后点击安装，等待安装完成即可。最好安装到 C 盘，安装在其它盘，否则安装后可能需要重新配置环境变量。

## 第 2 步：安装模型

1、接着返回 Ollama 官网，点击右上角的【Models】，再选择【deepseek-r1】（直达链接：https://ollama.com/library/deepseek-r1:1.5b）。

2、下拉可以看到多个参数版本，数字越大，模型参数越多，性能越强，对显存要求越高。选中适合你电脑配置的版本（这里以 1.5b 为例），复制页面中显示的命令。

![选择模型](/img/deepseek2.png)

## 第 3 步：安装模型

1、在 Windows 系统中，按下【Win+R】，输入【cmd】，点击【确定】，打开命令行。

2、在命令行中粘贴刚才复制的命令，按下回车键后，系统会自动开始下载模型，等待下载完成即可。

## 第 4 步：开始对话

1、下载完成后，你的电脑就成功拥有了一个离线 DeepSeek，就算断网也能使用。此时，你可以直接在命令行里输入问题，与模型开始互

![deepseek3](/img/deepseek3.png)

2、关闭 cmd 后，下次想与 DeepSeek 聊天，重复第 3 步即可。如果不习惯命令行窗口里面对话，可以安装 Open-WebUI 或 Chatbox，此处不再赘叙。
