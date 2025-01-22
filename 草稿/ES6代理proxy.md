要理解 JavaScript 的 `Proxy`，你可以把它想象成一个“中介”或“代理”，它拦截并控制对另一个对象的操作。我们通过 `Proxy` 来**修改对象的默认行为**，比如拦截属性的读取、修改，甚至拦截方法调用等。

下面我们从**核心概念**、**例子**和**常见应用**几个角度来帮你理清 `Proxy` 的机制。

### 1. 核心概念

- **`Proxy` 对象**：`Proxy` 是一个**包装器**，用来代理另一个对象。它通过“**捕获器（handler）**”拦截对原始对象的操作，并允许我们自定义这些操作的行为。
- **原始对象（target）**：`Proxy` 代理的对象，可以是任何 JavaScript 对象（比如普通对象、数组、函数等）。
- **捕获器（handler）**：捕获器是一个对象，它定义了拦截行为。通过捕获器的特定方法，你可以修改对代理对象的各种操作（如属性读取、设置、删除等）。

### 2. Proxy 的基本结构

一个 `Proxy` 由三个部分组成：

1. **目标对象**（`target`）：被代理的原始对象。
2. **捕获器对象**（`handler`）：定义拦截操作的对象。
3. **代理对象**（`proxy`）：代理后的对象，用于执行被拦截和修改的操作。

```javascript
const target = { name: "Alice" };

const handler = {
  get: function (target, prop) {
    console.log(`Getting property ${prop}`);
    return target[prop];
  },
  set: function (target, prop, value) {
    console.log(`Setting property ${prop} to ${value}`);
    target[prop] = value;
  },
};

const proxy = new Proxy(target, handler);
```

### 3. 如何使用 `Proxy`

上面的 `handler` 对象定义了两个捕获器：`get` 和 `set`，分别拦截属性读取和设置的操作。下面看看如何使用它：

```javascript
// 读取属性
console.log(proxy.name); // 输出: Getting property name，Alice

// 设置属性
proxy.name = "Bob"; // 输出: Setting property name to Bob

console.log(proxy.name); // 输出: Getting property name，Bob
```

#### 解释：

- 当你通过 `proxy.name` 访问 `name` 属性时，`get` 捕获器被调用，`console.log` 输出 `Getting property name`，然后返回 `target.name`。
- 当你给 `proxy.name` 赋值时，`set` 捕获器被触发，并输出 `Setting property name to Bob`，然后将 `name` 的值更新为 `'Bob'`。

### 4. Proxy 的常见应用场景

#### 1. **验证输入**

可以使用 `set` 捕获器来限制给对象属性赋予不合理的值。

```javascript
const handler = {
  set: function (target, prop, value) {
    if (typeof value !== "number") {
      throw new Error("Value must be a number");
    }
    target[prop] = value;
  },
};

const proxy = new Proxy({}, handler);
proxy.age = 30; // 正常工作
proxy.age = "thirty"; // 抛出错误：Value must be a number
```

#### 2. **自动计算属性**

可以通过 `get` 捕获器创建动态属性。

```javascript
const target = { num1: 10, num2: 20 };

const handler = {
  get: function (target, prop) {
    if (prop === "sum") {
      return target.num1 + target.num2;
    }
    return target[prop];
  },
};

const proxy = new Proxy(target, handler);
console.log(proxy.sum); // 输出: 30
```

#### 3. **观察者模式**

可以使用 `Proxy` 来监听对象的变化，方便在数据改变时触发一些逻辑。

```javascript
const handler = {
  set: function (target, prop, value) {
    console.log(`Property ${prop} is being set to ${value}`);
    target[prop] = value;
  },
};

const proxy = new Proxy({}, handler);
proxy.name = "Alice"; // 输出: Property name is being set to Alice
```

#### 4. **保护对象**

通过 `Proxy` 可以防止属性被修改或删除，保护对象的某些属性。

```javascript
const handler = {
  deleteProperty: function (target, prop) {
    if (prop in target) {
      console.log(`Property ${prop} cannot be deleted`);
      return false; // 阻止删除
    }
  },
};

const target = { name: "Alice" };
const proxy = new Proxy(target, handler);

delete proxy.name; // 输出: Property name cannot be deleted
```

### 5. Proxy 与原始对象的关系

尽管 `proxy` 代理了 `raw`，它们是两个**不同的对象**。你可以通过 `proxy` 访问和操作 `raw`，但它们本质上是分离的。这就是为什么 `proxy !== raw`，但通过 `proxy` 对属性的任何修改会直接影响 `raw`。

```javascript
const raw = { age: 25 };
const proxy = new Proxy(raw, {});

proxy.age = 30;
console.log(raw.age); // 输出: 30

console.log(proxy === raw); // 输出: false
```

`proxy` 和 `raw` 是不同的对象，但它们的状态是同步的，修改 `proxy` 也会影响 `raw`，但直接比较时 `proxy !== raw`，因为它们不是同一个引用。

### 总结

- **`Proxy` 是 JavaScript 的一种工具，允许你拦截和修改对对象的操作**。
- 它通过 `handler` 对象中的各种捕获器拦截操作（如读取、设置、删除属性等），你可以在这些捕获器中定义自定义行为。
- **`proxy` 是代理对象，`raw` 是被代理的原始对象，它们是不同的，但状态会互相影响**。
