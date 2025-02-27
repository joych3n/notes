# 解决 Vue3 报错：类型“ImportMeta”上不存在属性“env”

在基于 Vue3 开发的项目中，我们经常需要使用 env 环境变量：

```javascript
let baseUrl = import.meta.env.VITE_BASE_URL;
```

如果遇到报错：`类型“ImportMeta”上不存在属性“env”`,可以在`src/env.d.ts`文件添加一下代码：

```javascript
// src/env.d.ts
interface ImportMetaEnv {
  readonly VITE_API_URL: string    // 你的自定义环境变量
  readonly VITE_MODE: string      // 更多变量...
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```
