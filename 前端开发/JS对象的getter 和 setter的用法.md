### 示例：

```javascript
const obj = {
  _foo: 0,
  get foo() {
    console.log("Getting foo");
    return this._foo;
  },
  set foo(x) {
    console.log("Setting foo to", x);
    this._foo = x;
  },
};

obj.foo = 42; // 设置 foo 值时，触发 setter 方法
console.log(obj.foo); // 获取 foo 值时，触发 getter 方法
```

### 解释：

这段代码定义了一个包含 getter 和 setter 的对象 `obj`。具体来说：

1. **getter** (`get foo()`):

   - `get` 是用来定义一个属性的 **getter** 方法，它会在访问 `obj.foo` 时自动调用。
   - `foo` 这个属性是计算得到的，可以用于返回某个值，或者执行一些操作。这个方法不接受参数，但可以返回一个值。

2. **setter** (`set foo(x)`):
   - `set` 是用来定义一个属性的 **setter** 方法，它会在给 `obj.foo` 赋值时自动调用。
   - `x` 是传入的参数，代表你给 `foo` 赋的值。setter 方法可以对这个值进行处理或验证。

这种方式允许你控制对象属性的获取和赋值过程，通常用于封装和计算属性，是 Vue2 的响应式原理。
