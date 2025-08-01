# 前端面试题：vue 是单向数据流还是双向数据流？v-model 数据双向绑定是否矛盾？

## Vue 严格遵循单向数据流原则

父组件通过 props 向子组件传递数据（自上而下），子组件通过 $emit 向父组件发送事件（自下而上）。子组件不能直接修改父组件传递的 props，否则会导致状态混乱和难以追踪的 Bug。

## 为什么 v-model 能实现"双向绑定"？

v-model 本质上是 ​ 单向数据流+语法糖，通过两个步骤实现双向绑定的效果：

```html
<!-- 父组件 -->
<template>
  <Child v-model="parentData" />
  <!-- 等价于 -->
  <Child
    :modelValue="parentData"
    @update:modelValue="newVal => parentData = newVal"
  />
</template>

<!-- 子组件 -->
<script setup>
  const props = defineProps(["modelValue"]);
  const emit = defineEmits(["update:modelValue"]);

  const updateValue = (e) => {
    // 通过事件通知父组件更新
    emit("update:modelValue", e.target.value);
  };
</script>
```

## 为什么这种设计是合理的？

1. ​ 数据可追溯性 ​：所有数据变更都发生在父组件，便于调试
2. ​ 明确责任边界 ​：子组件只负责触发事件，不直接修改数据
3. ​ 兼容性优势 ​：在 Vue 3.3+ 中可以通过 defineModel() 宏进一步简化（底层仍然是单向数据流）

```html
<!-- 子组件简化版 -->
<script setup>
  const modelValue = defineModel(); // 自动处理 props 和 emit
</script>
```
