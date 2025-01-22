# JS 实现一个指定范围的随机数

在 JavaScript 中，可以使用 `Math.random()` 函数来生成指定范围的随机数，再对这个返回值进行一定的转换。

### 代码示例

```javascript
function getRandom(min, max) {
  return Math.random() * (max - min) + min;
}
```

### 原理解释

1. **Math.random()**：返回一个 [0, 1) 区间的随机小数。
2. **`Math.random() * (max - min)`**：将随机值缩放到 [0, `max - min`) 的范围内。
3. **`+ min`**：通过加上 `min`，将结果偏移到 [min, max) 区间。

### 示例

生成一个范围在 5 到 10 之间的随机数：

```javascript
const randomValue = getRandom(5, 10);
console.log(randomValue); // 可能输出：7.245, 8.034 等
```

### 生成整数

可以使用 `Math.floor()` 或 `Math.round()` 来对结果进行取整。例如，生成一个在 1 到 100 之间的随机整数：

```javascript
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

const randomInt = getRandomInt(1, 100);
console.log(randomInt); // 可能输出：34, 72 等
```

- **`Math.random() * (max - min + 1)`**：这里的 `+1` 是为了确保包括 `max` 的值。
- **`Math.floor()`**：对生成的浮动随机数取整，返回最接近的整数。

这种方法广泛用于生成随机数，尤其是在游戏开发、数据模拟、测试用例生成等场景中。
