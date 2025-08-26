# JS 中什么是闭包？闭包有什么作用？

## 什么是闭包？

闭包（Closure）是函数与其定义时的词法作用域的组合，即使外部函数已执行完毕，内部函数仍能访问外部变量。简单说：​ 闭包 = 函数 + 创建时的环境。

原理是内部函数会持有外部作用域的引用（[[Environment]]属性），使外部变量不被销毁。

经典的 JavaScript 闭包的例子：

## 例子 1 - 封装私有变量：

隐藏数据，仅暴露指定接口

```js
function outer() {
  const message = "Hello"; // 外部变量
  return function inner() {
    console.log(message); // 内部函数访问外部变量
  };
}
const fn = outer();
fn(); // 输出 "Hello"（即使outer已执行结束）
```

## 例子 2 - 函数工厂：

动态生成定制函数

```js
function outerFun(outerVar) {
  return function innerFun(innerVar) {
    console.log("Outer Var:", outerVar);
    console.log("Inner Var:", innerVar);
  };
}

const newFun = outerFun("Hello"); // outerFun 返回了 innerFun，但 outerVar 被保存下来
newFun("World"); // 调用 innerFun，并传递 innerVar

// 输出：
// Outer Var: Hello
// Inner Var: World

//  解释：
// 1. `outerFun` 是一个**外部**函数，它接受一个参数 `outerVar`。
// 2. `innerFun` 是一个**内部**函数，它可以访问外部函数的变量 `outerVar`，并且也接受自己的参数 `innerVar`。
// 3. 当你调用 `outerFun('Hello')` 时，返回的是 `innerFun`，但 `outerFun` 中的 `outerVar` 依然存在，即使外部函数已经执行完毕。
// 4. 调用 `newFun('World')` 时，`innerFun` 能够访问并打印 `outerVar` 和 `innerVar`，体现了闭包的特性。
```

这 2 个例子展示了闭包如何“记住”外部函数中的变量，即使外部函数已经结束执行。
