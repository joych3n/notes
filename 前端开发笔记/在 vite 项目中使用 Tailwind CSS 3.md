# 在 vite 项目中使用 Tailwind CSS 3

Tailwind CSS 是一个功能强大的工具类 CSS 框架，它与传统的 CSS 框架不同，Tailwind 更强调原子化的类名，让你通过直接应用类来控制样式，而不需要编写自定义 CSS。以下是一个快速学习和使用 Tailwind CSS 的指南：

> 在 vite 项目中使用 Tailwind CSS 3 的[官方文档](<[https://](https://v3.tailwindcss.com/docs/guides/vite)>)

1. 安装 Tailwind CSS

```
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
```

2. 配置您的模板路径

```javascript
// 在项目根目录创建 tailwind.config.js 文件
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

3. 将 Tailwind 指令添加到您的 CSS 中

```css
/* 将@tailwind 指令添加到您的./src/index.css 文件中。 */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

4. 在项目中使用

```html
<template>
  <h1 className="text-3xl font-bold underline">Hello world!</h1>
</template>
```
