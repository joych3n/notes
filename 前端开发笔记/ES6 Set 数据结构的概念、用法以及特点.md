# ES6 Set 数据结构的概念、用法以及特点

在 JavaScript 中，`Set` 是新的数据结构，在 ES6（ECMAScript 2015）中被引入，具有很多独特的特点和用途。让我们来学习这种数据结构的概念、用法以及特点。

### Set

#### 概念

- `Set` 是一种值的集合，其中每个值都是唯一的（即同一个 Set 中不能有两个相同的值）。Set 主要用于去重和存储唯一值。

#### 用法

1. **创建 Set**

   ```javascript
   const mySet = new Set();
   ```

2. **添加元素**

   ```javascript
   mySet.add(1);
   mySet.add(2);
   mySet.add(2); // 不会重复添加
   ```

3. **访问元素**

   - Set 不提供直接的索引访问，通常我们通过遍历来访问元素。

   ```javascript
   mySet.forEach((value) => {
     console.log(value);
   });
   ```

4. **删除元素**

   ```javascript
   mySet.delete(2); // 删除值为 2 的元素
   ```

5. **检查值是否存在**

   ```javascript
   console.log(mySet.has(1)); // 输出: true
   ```

6. **获取大小**

   ```javascript
   console.log(mySet.size); // 输出: 1 (因为只有一个元素)
   ```

#### 特点

- **唯一性**：Set 会自动去重，尝试添加相同的值不会有任何效果。
- **插入顺序**：跟 Map 一样，Set 也保持插入顺序。
