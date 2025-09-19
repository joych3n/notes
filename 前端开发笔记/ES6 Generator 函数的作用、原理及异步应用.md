# ES6 Generator 函数的作用、原理及异步应用

ES6 Generator 函数是 JavaScript 中用于**控制函数执行流程**的强大特性，它允许函数暂停和恢复执行，有助于解决异步编程问题，并能创建高效的数据迭代器。下面我们深入看看它的核心用法、原理以及一些实用场景。

## 🔍 一、Generator 函数基本概念

Generator 函数是 ES6 引入的一种**异步编程解决方案**，语法行为与传统函数完全不同。形式上，Generator 函数是一个普通函数，但有兩個特征：

- `function` 关键字与函数名之间有一个星号（\*）。
- 函数体内部使用 `yield` 表达式，定义不同的内部状态（yield 在英语里的意思就是“产出”）。

调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的**指针对象**（即遍历器对象 Iterator Object）。必须调用遍历器对象的 `next` 方法，使得指针移向下一个状态。

```javascript
function* helloWorldGenerator() {
  yield "hello";
  yield "world";
  return "ending";
}

var hw = helloWorldGenerator();

hw.next(); // { value: 'hello', done: false }
hw.next(); // { value: 'world', done: false }
hw.next(); // { value: 'ending', done: true }
hw.next(); // { value: undefined, done: true }
```

## ⚙️ 二、核心特性与工作原理

Generator 函数的核心在于其能够**暂停执行**和**恢复执行**。

1. **yield 表达式**：

   - `yield` 是暂停执行的标记。当执行到 `yield` 时，函数会暂停，并将紧跟在 `yield` 后面的表达式的值作为返回对象的 `value` 属性值。
   - `yield` 表达式后面的表达式不会立即求值，只会在 `next` 方法将指针移到这一句时才会求值（**惰性求值**）。
   - `yield` 表达式只能用在 Generator 函数内部，在其他地方使用会报错。

2. **next 方法**：

   - `next` 方法用于将指针移向下一个状态。
   - 每次调用 `next` 方法，会返回一个包含 `value` 和 `done` 两个属性的对象。
   - `value` 属性表示当前 `yield` 表达式或 `return` 语句的值。
   - `done` 属性是一个布尔值，表示遍历是否结束。
   - `next` 方法可以接受一个参数，**此参数会被当作上一个 `yield` 表达式的返回值**（注意：第一次调用 `next` 方法时传递参数是无效的，V8 引擎会直接忽略）。这使得我们可以从外部向 Generator 函数内部注入不同的值，调整函数行为。

```javascript
function* foo(x) {
  var y = 2 * (yield x + 1);
  var z = yield y / 3;
  return x + y + z;
}

var a = foo(5);
a.next(); // Object {value: 6, done: false}
a.next(); // Object {value: NaN, done: false} (因为 y = 2 * undefined 得到 NaN)
a.next(); // Object {value: NaN, done: true}

var b = foo(5);
b.next(); // { value:6, done:false }
b.next(12); // { value:8, done:false } (此时 y = 2 * 12 = 24)
b.next(13); // { value:42, done:true } (此时 z = 13, 5 + 24 + 13 = 42)
```

## 🛠️ 三、常用方法与语法

1. **for...of 循环**：`for...of` 循环可以自动遍历 Generator 函数运行时生成的 Iterator 对象，且此时不再需要调用 `next` 方法。需要注意的是，一旦 `next` 方法的返回对象的 `done` 属性为 `true`，`for...of` 循环就会中止，且不包含该返回对象，因此 `return` 语句返回的值不会包括在循环中。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

2. **yield\* 表达式**：用于在一个 Generator 函数内部调用另一个 Generator 函数或可迭代对象。`yield*` 表达式会迭代其后跟随的可迭代对象，并 yield 出其中每一个值。

```javascript
function* generator1() {
  yield "a";
  yield "b";
}

function* generator2() {
  yield 1;
  yield* generator1(); // 委托给另一个 Generator 函数
  yield 2;
  yield* [3, 4]; // 委托给一个数组
}

for (let value of generator2()) {
  console.log(value);
}
// 1
// 'a'
// 'b'
// 2
// 3
// 4
```

3. **Generator.prototype.throw()**：方法可以在 Generator 函数体外抛出错误，然后在 Generator 函数体内捕获。

```javascript
function* myGenerator() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (error) {
    console.error("内部捕获:", error);
  }
}

const gen = myGenerator();
gen.next(); // { value: 1, done: false }
gen.throw(new Error("出错了！")); // "内部捕获: Error: 出错了！"
// 接下来 gen.next() 会返回 { value: undefined, done: true }
```

4. **Generator.prototype.return()**：方法可以强制 Generator 函数结束执行并返回一个指定的值。

```javascript
function* myGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = myGenerator();
gen.next(); // { value: 1, done: false }
gen.return("foo"); // { value: "foo", done: true }，Generator 遍历终止
gen.next(); // { value: undefined, done: true }
```

## 💡 四、Generator 的常见应用场景

Generator 函数的强大之处在于它能提供一种**暂停和恢复执行**的能力，这使得它在许多场景下非常有用。

1. **异步流程控制**：在 `async/await` 普及之前，Generator 函数（通常与 Promise 结合）是解决“回调地狱”、管理异步操作流程的一种重要方式。它允许我们以接近同步代码的写法来处理异步操作。

```javascript
function* asyncFlow() {
  try {
    const result1 = yield mockFetch("/api/data1");
    console.log(result1);
    const result2 = yield mockFetch("/api/data2");
    console.log(result2);
  } catch (error) {
    console.error("Error:", error);
  }
}

// 需要一个执行器来运行这个 Generator
function runGenerator(generator) {
  const gen = generator();

  function handle(result) {
    if (result.done) return result.value;
    return Promise.resolve(result.value).then(
      (res) => handle(gen.next(res)),
      (err) => handle(gen.throw(err))
    );
  }

  return handle(gen.next());
}

runGenerator(asyncFlow);
//这种模式后来直接催生了 `async/await` 语法糖的出现。
```

2. **实现迭代器与无限序列**：Generator 为生成迭代器提供了极大的便利，它是可迭代对象（Iterable）和迭代器（Iterator）的完美实现方式。对于无限数据流或大规模数据集合，Generator 可以按需计算和产生值，避免一次性计算所有值带来的性能问题。

```javascript
// 一个简单的无限计数器
function* infiniteSequence() {
  let i = 0;
  while (true) {
    yield i++;
  }
}

const counter = infiniteSequence();
console.log(counter.next().value); // 0
console.log(counter.next().value); // 1
console.log(counter.next().value); // 2
// ...可以一直继续下去
```

```javascript
// 斐波那契数列生成器
function* fibonacci() {
  let [prev, curr] = [0, 1];
  while (true) {
    yield curr;
    [prev, curr] = [curr, prev + curr];
  }
}

const fibGen = fibonacci();
for (let i = 0; i < 10; i++) {
  console.log(fibGen.next().value);
}
// 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
```

3. **状态机**：Generator 函数非常适合用于表示状态机，因为其本身允许维护多个状态，并且可以通过 `yield` 来暂停。

## ⚖️ 五、Generator 的优缺点

| 优点                                       | 缺点                                                |
| :----------------------------------------- | :-------------------------------------------------- |
| 提供更优雅的异步编程解决方案，避免回调地狱 | 语法和理解相对复杂，学习曲线较陡                    |
| 函数执行过程可控，可暂停、恢复             | 需要额外的执行器或库（如 co）来自动执行             |
| 能按需生成数据，节省内存（惰性求值）       | 在简单异步场景下，可能不如 `async/await` 直观和简洁 |

## 🔮 六、Generator 与 async/await

ES7 引入的 `async` 函数和 `await` 关键字，实际上是 Generator 和 Promise 的**语法糖**，它在语言层面提供了更简洁、更直观的异步编程方式。`async` 函数内置了执行器，无需像 Generator 那样需要额外编写执行代码。

大致可以这样类比：

- `async function` 相当于 `function*`
- `await` 相当于 `yield`
- `async` 函数自动执行，而 Generator 需要手动调用 `next` 或使用执行器

虽然 `async/await` 现在更为主流，但理解 Generator 有助于你更深入地掌握 JavaScript 的异步编程机制。

## 💎 总结

ES6 Generator 函数通过引入 `yield` 和 `next()`，赋予了 JavaScript 函数**暂停和恢复执行**的能力，为处理异步操作、创建迭代器和实现状态机提供了强大的工具。虽然在实际项目中，`async/await` 因其简洁性常常成为首选，但 Generator 的概念和原理对于理解 JavaScript 的异步编程模型至关重要。
