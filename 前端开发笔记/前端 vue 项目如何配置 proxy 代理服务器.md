# 前端 vue 项目如何配置 proxy 代理服务器

## 一、vue proxy 代理服务器是什么？

本质是一个位于开发环境（development）中的轻量级 HTTP 请求中介服务。

## 二、vue proxy 代理服务器的作用？

主要作用是开发时绕过浏览器的同源策略限制。因为请求是发给同源的开发服务器，当请求路径与代理配置匹配时，会转发到后端服务器。

## 三、如何配置 vue proxy 代理服务器？

```js
// vue.config.js (Vue CLI) 或 vite.config.ts (Vite)
module.exports = {
  devServer: {
    // Vite 中是 server: {}
    proxy: {
      // 代理规则1: 简单字符串格式
      "/api": "http://your-api-domain.com",

      // 代理规则2: 详细对象格式
      "/service": {
        target: "https://target-server.com",
        changeOrigin: true, // 修改请求Host头
        secure: false, // 忽略HTTPS证书验证
        pathRewrite: {
          // 路径重写规则 (Vite使用rewrite函数)
          "^/service": "/v2", // Vue CLI: 用正则替换路径
        },
        // Vite专用路径重写：
        // rewrite: path => path.replace(/^\/service/, '')
      },
    },
  },
};
```
