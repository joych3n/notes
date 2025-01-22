# ES6 对象的扩展

## 1、属性的简洁表示法

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```js
let birth = "2000/01/01";

const Person = {
  name: "张三",

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() {
    console.log("我的名字是", this.name);
  },
};
```

## 2、属性名表达式

JavaScript 定义对象的属性，有两种方法。

```js
// 方法一
obj.foo = true;

// 方法二
obj["a" + "bc"] = 123;
```

如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。ES6 可以用方法二（表达式）作为对象的属性名和方法名，即把表达式放在方括号内。下面是一个例子。

```js
// 表达式写法 - 属性名
let lastWord = "last word";

const a = {
  "first word": "hello",
  [lastWord]: "world",
};

a["first word"]; // "hello"
a[lastWord]; // "world"
a["last word"]; // "world"

// 表达式写法 - 方法名
let obj = {
  ["h" + "ello"]() {
    return "hi";
  },
};

obj.hello(); // hi
```

## 3、 方法的 name 属性

如果对象的方法使用了取值函数（getter）和存值函数（setter），则 name 属性不是在该方法上面，而是该方法的属性的描述对象的 get 和 set 属性上面，返回值是方法名前加上 get 和 set。

```js
const obj = {
  get foo() {},
  set foo(x) {},
};

obj.foo.name;
// TypeError: Cannot read property 'name' of undefined

const descriptor = Object.getOwnPropertyDescriptor(obj, "foo");

descriptor.get.name; // "get foo"
descriptor.set.name; // "set foo"
```

## 4、属性的可枚举性和遍历

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。Object.getOwnPropertyDescriptor 方法可以获取该属性的描述对象。

```js
let obj = { foo: 123 };
Object.getOwnPropertyDescriptor(obj, "foo");
//  {
//    value: 123,
//    writable: true,
//    enumerable: true, // 可枚举性,影响遍历对象属性的返回值
//    configurable: true
//  }
```

## 5、super 关键字

我们知道，this 关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字 super，指向当前对象的原型对象。

<!-- JS代码例子 -->

```js
const proto = {
  foo: "hello",
};

const obj = {
  foo: "world",
  find() {
    return super.foo;
  },
};

Object.setPrototypeOf(obj, proto);
obj.find(); // "hello"
```

## 6、对象的扩展运算符

### 解构赋值

将目标对象尚未被读取的属性，分配到指定的对象上面。

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x; // 1
y; // 2
z; // { a: 3, b: 4 }
```

### 扩展运算符

用于取出参数对象的所有可遍历属性，拷贝到当前对象之中。

```js
let z = { a: 3, b: 4 };
let n = { ...z };
n; // { a: 3, b: 4 }
```
