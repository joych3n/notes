# ES6 Proxy 技术解析

这是一篇简洁明了的 ES6 Proxy 技术解析，涵盖核心概念、常用场景及注意事项，助你快速掌握这一强大特性。

---

## **一、Proxy 是什么？**

Proxy（代理）是 ES6 引入的**对象包装器**，用于拦截并自定义对目标对象的基本操作（如属性读写、函数调用等）。其核心原理是为目标对象架设一个“中间层”，所有操作需先经代理处理。  
**语法**：

```javascript
const proxy = new Proxy(target, handler);
```

- `target`：被代理的目标对象（可以是对象、数组、函数等）。
- `handler`：包含**拦截方法**（陷阱函数）的对象，如 `get`、`set` 等。

---

## **二、核心拦截操作（陷阱函数）**

### 1. **`get(target, prop, receiver)`**

拦截属性读取：

```javascript
const handler = {
  get(target, prop) {
    return prop in target ? target[prop] : "默认值";
  },
};
const proxy = new Proxy({ name: "Alice" }, handler);
console.log(proxy.name); // "Alice"
console.log(proxy.age); // "默认值"
```

**应用**：动态生成属性、访问控制。

### 2. **`set(target, prop, value, receiver)`**

拦截属性赋值：

```javascript
const validator = {
  set(target, prop, value) {
    if (prop === "age" && (value < 0 || value > 120)) {
      throw new Error("年龄无效");
    }
    target[prop] = value;
    return true; // 必须返回布尔值
  },
};
const user = new Proxy({}, validator);
user.age = 25; // 成功
user.age = 150; // 报错
```

**应用**：数据验证、自动更新视图（响应式系统）。

### 3. **`has(target, prop)`**

拦截 `in` 操作符：

```javascript
const handler = {
  has(target, prop) {
    return prop.startsWith("_") ? false : prop in target;
  },
};
const obj = new Proxy({ _secret: "x", public: "y" }, handler);
console.log("_secret" in obj); // false
```

**应用**：隐藏私有属性（如 `_` 前缀）。

### 4. **`apply(target, thisArg, args)`**

拦截函数调用：

```javascript
const handler = {
  apply(target, thisArg, args) {
    console.log(`函数被调用，参数：${args}`);
    return target.apply(thisArg, args) * 2; // 修改返回值
  },
};
const doubleProxy = new Proxy((x) => x + 1, handler);
console.log(doubleProxy(2)); // 输出日志后返回 6
```

**应用**：函数缓存、日志记录。

---

## **三、实用场景**

1. **数据响应式（如 Vue 3）**：通过 `get` 收集依赖、`set` 触发更新：

```javascript
function reactive(obj) {
  return new Proxy(obj, {
    set(target, prop, value) {
      console.log(`更新 ${prop} 触发视图渲染`);
      return Reflect.set(target, prop, value);
    },
  });
}
const data = reactive({ count: 0 });
data.count++; // 输出日志并更新
```

2. **表单验证**：解耦校验逻辑与业务代码：

```javascript
const schema = { age: (v) => Number.isInteger(v) };
const form = new Proxy(
  {},
  {
    set(target, prop, value) {
      if (schemavalue) {
        throw new Error(`验证失败：${prop}`);
      }
      target[prop] = value;
      return true;
    },
  }
);
form.age = 30; // 成功
```

3. **API 请求拦截**：统一处理请求参数：

```javascript
const api = new Proxy(
  {},
  {
    get(target, method) {
      return (url, data) => {
        console.log(`[${method}] ${url}`, data);
        return fetch(url, { method, body: JSON.stringify(data) });
      };
    },
  }
);
api.get("/user", { id: 1 }); // 输出日志并发送请求
```

4. **权限控制**：动态限制属性访问：

```javascript
const secure = new Proxy(
  { password: "123" },
  {
    get(target, prop) {
      if (prop === "password" && !isAdmin()) throw new Error("无权访问");
      return target[prop];
    },
  }
);
```

---

## **四、注意事项**

1. **性能开销**：代理操作比直接访问稍慢，避免在高频循环中使用。
2. **`this` 绑定问题**：目标对象方法内的 `this` 默认指向代理对象，必要时使用 `Reflect` 或绑定修正。
3. **内置对象限制**：代理 `Date`、`Map` 等内置对象时，部分内部方法无法拦截（需通过继承解决）。
4. **兼容性**：不支持 IE 浏览器，可通过 Babel 或 polyfill 降级。

---

## **五、最佳实践**

- **结合 `Reflect` API**：用 `Reflect.get/set` 代替直接操作目标对象，确保行为一致性。
- **避免过度拦截**：仅拦截必要操作，保持代码可维护性。
- **可撤销代理**：通过 `Proxy.revocable()` 创建临时代理，控制访问时效。

---

## **总结**

Proxy 是 JavaScript **元编程**的核心工具，通过拦截器实现数据绑定、校验、缓存等高级功能。合理使用可提升代码灵活性与健壮性，但需权衡性能与复杂度。掌握其核心机制，能显著优化开发模式！
