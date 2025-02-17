# ES6 Map 数据结构的概念、用法以及特点

在 JavaScript 中，`Map` 是新的数据结构，在 ES6（ECMAScript 2015）中被引入，具有很多独特的特点和用途。让我们来学习这种数据结构的概念、用法以及特点。

## Map

### 概念

- `Map` 是一种键值对（key-value pairs）的集合，其中每个键都是唯一的（即同一个 Map 中不能有两个相同的键），值则可以是任何类型的数据（基础类型或对象）。

### 用法

1. **创建 Map**

   ```javascript
   const myMap = new Map();
   ```

2. **添加元素**

   ```javascript
   myMap.set("name", "Alice");
   myMap.set("age", 25);
   ```

3. **访问元素**

   ```javascript
   console.log(myMap.get("name")); // 输出: Alice
   console.log(myMap.get("age")); // 输出: 25
   ```

4. **删除元素**

   ```javascript
   myMap.delete("age"); // 删除键为 'age' 的元素
   ```

5. **检查键是否存在**

   ```javascript
   console.log(myMap.has("name")); // 输出: true
   ```

6. **遍历 Map**

   ```javascript
   myMap.forEach((value, key) => {
     console.log(`${key}: ${value}`);
   });
   ```

### 特点

- **插入顺序**：Map 保持元素的插入顺序，可以使用 `forEach` 或 `for...of` 来按照插入顺序遍历元素。
- **任意类型的键**：Map 的键可以是任意类型，包括对象，而普通对象的键只能是字符串或符号。
