---
title: 数组方法
description: 数组数据转换方法参考
---

# 数组方法

数组方法用于处理和转换数组数据。所有方法都可以链式调用。

## 方法列表

### average()

返回数组中数字的平均值。如果数组中有任何非数字，会抛出错误。如果数组为空，返回 `0`。

**返回值**: `Number`

**示例**:

```javascript
{{ [12, 1, 5].average() }}
// 6

{{ [1, 2, 3, 4, 5].average() }}
// 3

{{ [].average() }}
// 0

{{ $('HTTP Request').body.prices.average() }}
// 计算价格平均值
```

---

### chunk(size: Number)

将数组拆分为指定长度的子数组数组。如果 `size` 不是数字或为零，会抛出错误。

**参数**:
- `size` (Number): 每个块中的元素数量（必须是非零数字）

**返回值**: `Array`

**示例**:

```javascript
{{ [1, 2, 3, 4, 5, 6].chunk(2) }}
// [[1, 2], [3, 4], [5, 6]]

{{ [1, 2, 3, 4, 5].chunk(2) }}
// [[1, 2], [3, 4], [5]]

{{ ['a', 'b', 'c', 'd', 'e', 'f'].chunk(3) }}
// [['a', 'b', 'c'], ['d', 'e', 'f']]

{{ $('HTTP Request').body.items.chunk(10) }}
// 每次处理 10 个项目
```

---

### compact()

从数组中移除 `null` 和 `undefined` 值。递归处理嵌套数组和对象。

**返回值**: `Array`

**示例**:

```javascript
{{ [1, null, 2, undefined, 3].compact() }}
// [1, 2, 3]

{{ [0, false, '', null, undefined, 'hello'].compact() }}
// [0, false, '', 'hello']
// 注意：0、false、'' 不会被移除

{{ [2, null, 1, undefined, '', 'nil', []].compact() }}
// [2, 1, '', 'nil', []]

{{ $('HTTP Request').body.users.compact() }}
// 移除空用户
```

---

### difference(arr: Array)

比较两个数组。返回基础数组中不在 `arr` 中的所有元素。结果中的重复项会被移除。

**参数**:
- `arr` (Array): 要与基础数组进行比较的数组

**返回值**: `Array`

**示例**:

```javascript
{{ [1, 2, 3].difference([2, 3]) }}
// [1]

{{ [1, 2, 3, 4, 5].difference([3, 4, 5, 6, 7]) }}
// [1, 2]

{{ ['a', 'b', 'c'].difference(['b', 'c', 'd']) }}
// ['a']

{{ $('Current Users').users.difference($('Previous Users').users) }}
// 找出新用户
```

---

### first()

返回数组的第一个元素。数组为空时返回 `undefined`。

**返回值**: `Array item`

**示例**:

```javascript
{{ [1, 2, 3].first() }}
// 1

{{ ['apple', 'banana', 'cherry'].first() }}
// 'apple'

{{ [].first() }}
// undefined

{{ $('HTTP Request').body.items.first() }}
// 第一个项目
```

---

### intersection(arr: Array)

比较两个数组，返回同时存在于两个数组中的所有元素。结果中的重复项会被移除。

**参数**:
- `arr` (Array): 要比较的数组

**返回值**: `Array`

**示例**:

```javascript
{{ [1, 2, 3].intersection([2, 3]) }}
// [2, 3]

{{ [1, 2, 3, 4, 5].intersection([3, 4, 5, 6, 7]) }}
// [3, 4, 5]

{{ ['a', 'b', 'c'].intersection(['b', 'c', 'd']) }}
// ['b', 'c']

{{ $('List A').items.intersection($('List B').items) }}
// 找出共同项
```

---

### isEmpty()

如果数组没有元素或是 `null`，返回 `true`。

**返回值**: `Boolean`

**示例**:

```javascript
{{ [].isEmpty() }}
// true

{{ ['quick', 'brown', 'fox'].isEmpty() }}
// false

{{ $('HTTP Request').body.items.isEmpty() }}
// 检查是否有项目
```

---

### isNotEmpty()

检查数组是否包含元素。

**返回值**: `Boolean`

**示例**:

```javascript
{{ [1, 2, 3].isNotEmpty() }}
// true

{{ [].isNotEmpty() }}
// false

{{ $('HTTP Request').body.users.isNotEmpty() }}
// 检查是否有用户
```

---

### last()

返回数组的最后一个元素。数组为空时返回 `undefined`。

**返回值**: `Array item`

**示例**:

```javascript
{{ [1, 2, 3].last() }}
// 3

{{ ['apple', 'banana', 'cherry'].last() }}
// 'cherry'

{{ [].last() }}
// undefined

{{ $('HTTP Request').body.items.last() }}
// 最后一个项目
```

---

### max()

返回数组中的最大数字。如果数组中有任何非数字，会抛出错误。字符串数字使用 `parseFloat` 解析。

**返回值**: `Number`

**示例**:

```javascript
{{ [1, 12, 5].max() }}
// 12

{{ [1, 5, 3, 9, 2].max() }}
// 9

{{ [-10, -5, -20].max() }}
// -5

{{ $('HTTP Request').body.prices.max() }}
// 最高价格
```

---

### merge(arr?: Array)

将两个对象数组合并为一个对象，通过合并对应位置的键值对。如果不提供参数，则将数组内的所有对象合并为一个对象。

**参数**:
- `arr` (Array, 可选): 要与基础数组合并的对象数组

**返回值**: `Object`

**示例**:

```javascript
// 合并两个对象数组
{{ [{ name: 'Nathan' }, { age: 42 }].merge([{ city: 'Berlin' }, { country: 'Germany' }]) }}
// { name: 'Nathan', age: 42, city: 'Berlin', country: 'Germany' }

// 合并数组内所有对象（无参数）
{{ [{ name: 'Alice' }, { age: 25 }, { city: 'New York' }].merge() }}
// { name: 'Alice', age: 25, city: 'New York' }

{{ $('List A').items.merge($('List B').items) }}
// 合并两个对象数组
```

---

### min()

返回数组中的最小数字。如果数组中有任何非数字，会抛出错误。字符串数字使用 `parseFloat` 解析。

**返回值**: `Number`

**示例**:

```javascript
{{ [12, 1, 5].min() }}
// 1

{{ [1, 5, 3, 9, 2].min() }}
// 1

{{ [-10, -5, -20].min() }}
// -20

{{ $('HTTP Request').body.prices.min() }}
// 最低价格
```

---

### pluck(...fieldNames: String)

返回一个数组，包含数组中每个对象的给定字段的值。忽略任何不是对象或不具有匹配字段名键的数组元素。如果提供多个字段名，返回数组的数组。如果不提供字段名，返回原数组。

**参数**:
- `fieldNames` (String, 可变参数): 要检索值的键

**返回值**: `Array`

**示例**:

```javascript
// 提取单个字段
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Jan', city: 'Berlin' }
].pluck('name') }}
// ['Nathan', 'Jan']

// 提取不存在于所有对象中的字段
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Jan', city: 'Berlin' }
].pluck('age') }}
// [42]

// 提取多个字段
{{ [
  { name: 'Alice', age: 25, city: 'NYC' },
  { name: 'Bob', age: 30, city: 'LA' }
].pluck('name', 'age') }}
// [['Alice', 25], ['Bob', 30]]

// 无参数 - 返回原数组
{{ [1, 2, 3].pluck() }}
// [1, 2, 3]

{{ $('HTTP Request').body.users.pluck('email') }}
// 提取所有用户的邮箱
```

---

### randomItem()

从数组中返回一个随机选择的元素。如果数组为空或是 `undefined`，返回 `undefined`。

**返回值**: `Array item` 或 `undefined`

**示例**:

```javascript
{{ ['quick', 'brown', 'fox'].randomItem() }}
// 随机返回：'quick'、'brown' 或 'fox' 之一

{{ [1, 2, 3, 4, 5].randomItem() }}
// 随机返回一个数字，例如：3

{{ [].randomItem() }}
// undefined

{{ $('HTTP Request').body.quotes.randomItem() }}
// 随机选择一条引用
```

---

### removeDuplicates(...fieldNames: String)

从数组中移除重复元素。`unique()` 的别名。对于对象数组，可以指定一个或多个字段名来检查相等性。

**参数**:
- `fieldNames` (String, 可变参数, 可选): 对于对象数组，指定用于判断重复的字段名

**返回值**: `Array`

**示例**:

```javascript
// 简单数组去重
{{ [1, 2, 2, 3, 3, 3, 4].removeDuplicates() }}
// [1, 2, 3, 4]

// 对象数组，根据指定字段去重
{{ [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 1, name: "Alice" }
].removeDuplicates("id") }}
// [
//   { "id": 1, "name": "Alice" },
//   { "id": 2, "name": "Bob" }
// ]

// 对象数组，使用多个字段名
{{ [
  { name: 'Alice', age: 25, city: 'NYC' },
  { name: 'Alice', age: 25, city: 'LA' },
  { name: 'Bob', age: 30, city: 'NYC' }
].removeDuplicates('name', 'age') }}
// [
//   { "name": "Alice", "age": 25, "city": "NYC" },
//   { "name": "Bob", "age": 30, "city": "NYC" }
// ]

{{ $('HTTP Request').body.users.removeDuplicates('email') }}
// 根据邮箱去重
```

---

### renameKeys(from1: String, to1: String, from2?: String, to2?: String, ...)

更改数组中任何对象的匹配键（字段名）。通过添加额外的参数对来重命名多个键：`from1, to1, from2, to2, ...`。必须提供偶数个参数。

**参数**:
- `from1` (String): 要重命名的键
- `to1` (String): 新的键名
- `from2, to2, ...` (String, 可选): 要重命名的其他键对

**返回值**: `Array`

**示例**:

```javascript
// 重命名单个键
{{ [
  { name: 'bob' },
  { name: 'meg' }
].renameKeys('name', 'x') }}
// [
//   { "x": "bob" },
//   { "x": "meg" }
// ]

// 重命名多个键
{{ [
  { firstName: 'Alice', lastName: 'Smith', age: 25 }
].renameKeys('firstName', 'first', 'lastName', 'last') }}
// [
//   { "first": "Alice", "last": "Smith", "age": 25 }
// ]

{{ $('HTTP Request').body.users.renameKeys('user_id', 'id', 'user_name', 'name') }}
// 将 API 字段名转换为内部字段名
```

---

### sum()

返回数组中所有数字的总和。如果数组中有任何非数字，会抛出错误。字符串数字使用 `parseFloat` 解析。

**返回值**: `Number`

**示例**:

```javascript
{{ [12, 1, 5].sum() }}
// 18

{{ [1, 2, 3, 4, 5].sum() }}
// 15

{{ $('HTTP Request').body.prices.sum() }}
// 总价格
```

---

### toJsonString()

将数组转换为 JSON 字符串。相当于 `JSON.stringify`。

**返回值**: `String`

**示例**:

```javascript
{{ [1, 2, 3].toJsonString() }}
// "[1,2,3]"

{{ [{ name: 'Alice' }, { name: 'Bob' }].toJsonString() }}
// '[{"name":"Alice"},{"name":"Bob"}]'

{{ $('HTTP Request').body.items.toJsonString() }}
// 将数组转换为 JSON 字符串用于存储或传输
```

---

### union(arr: Array)

将两个数组合并后移除重复项。

**参数**:
- `arr` (Array): 要合并的数组

**返回值**: `Array`

**示例**:

```javascript
{{ [1, 2, 3].union([3, 4, 5]) }}
// [1, 2, 3, 4, 5]

{{ ['a', 'b'].union(['b', 'c']) }}
// ['a', 'b', 'c']

{{ $('List A').items.union($('List B').items) }}
// 合并两个列表并去重
```

---

### unique(...fieldNames: String)

从数组中移除任何重复元素。对于对象数组，可以指定一个或多个字段名来检查相等性。功能与 `removeDuplicates()` 相同。

**参数**:
- `fieldNames` (String, 可变参数, 可选): 对于对象数组，指定用于判断重复的字段名

**返回值**: `Array`

**示例**:

```javascript
// 简单数组去重
{{ ['quick', 'brown', 'quick'].unique() }}
// ['quick', 'brown']

{{ [1, 1, 2, 2, 3].unique() }}
// [1, 2, 3]

// 对象数组，不使用字段名（比较整个对象）
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Nathan', age: 22 }
].unique() }}
// [
//   { "name": "Nathan", "age": 42 },
//   { "name": "Nathan", "age": 22 }
// ]
// 注意：对象不同，所以都保留

// 对象数组，使用单个字段名
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Nathan', age: 22 }
].unique('name') }}
// [
//   { "name": "Nathan", "age": 42 }
// ]

// 对象数组，使用多个字段名
{{ [
  { name: 'Alice', age: 25, city: 'NYC' },
  { name: 'Alice', age: 25, city: 'LA' },
  { name: 'Bob', age: 30, city: 'NYC' }
].unique('name', 'age') }}
// [
//   { "name": "Alice", "age": 25, "city": "NYC" },
//   { "name": "Bob", "age": 30, "city": "NYC" }
// ]
```

## 链式调用

数组方法可以链式调用：

```javascript
{{ $('HTTP Request').body.items
  .filter(item => item.active)
  .pluck('price')
  .compact()
  .average() }}
// 计算所有活跃项目的平均价格
```

## 使用场景

### 1. 数据提取

```javascript
// 提取所有对象的单个字段
{{ $('Get Users').body.users.pluck('email') }}

// 提取所有对象的多个字段
{{ $('Get Users').body.users.pluck('name', 'email') }}

// 提取第一个和最后一个项目
{{ $('HTTP Request').body.items.first() }}
{{ $('HTTP Request').body.items.last() }}
```

### 2. 数据聚合

```javascript
// 计算总价
{{ $('HTTP Request').body.prices.sum() }}

// 计算平均分
{{ $('HTTP Request').body.scores.average() }}

// 找出最高价和最低价
{{ $('HTTP Request').body.prices.max() }}
{{ $('HTTP Request').body.prices.min() }}
```

### 3. 数据清理

```javascript
// 移除空值（递归处理嵌套数组和对象）
{{ $('HTTP Request').body.items.compact() }}

// 根据单个字段去重
{{ $('HTTP Request').body.users.unique('id') }}

// 根据多个字段去重
{{ $('HTTP Request').body.users.unique('email', 'name') }}

// 重命名单个字段
{{ $('HTTP Request').body.users.renameKeys('user_id', 'id') }}

// 重命名多个字段
{{ $('HTTP Request').body.users.renameKeys('user_id', 'id', 'user_name', 'name') }}
```

### 4. 数据合并

```javascript
// 将对象数组合并为单个对象
{{ $('List A').items.merge($('List B').items) }}

// 合并数组内所有对象
{{ $('HTTP Request').body.items.merge() }}

// 合并并去重
{{ $('List A').items.union($('List B').items) }}

// 找出共同项
{{ $('List A').items.intersection($('List B').items) }}

// 找出差异
{{ $('List A').items.difference($('List B').items) }}
```

### 5. 数据分组

```javascript
// 将数组分块处理
{{ $('HTTP Request').body.items.chunk(10) }}
// 每次处理 10 个项目
```

## 最佳实践

### 1. 检查数组是否为空

```javascript
// 在处理前检查
{{ $('HTTP Request').body.items.isNotEmpty() }}
```

### 2. 处理可能不存在的字段

```javascript
// 使用 compact 移除空值
{{ $('HTTP Request').body.users.pluck('email').compact() }}
```

### 3. 组合多个方法

```javascript
// 链式调用多个方法
{{ $('HTTP Request').body.items
  .compact()
  .unique('id')
  .pluck('name') }}
```

### 4. 性能考虑

```javascript
// 先过滤再处理，减少数据量
{{ $('HTTP Request').body.items
  .filter(item => item.active)
  .pluck('price')
  .sum() }}
```

## 相关资源

- [表达式概述](/zh-hans/guide/expressions)
- [对象方法](/zh-hans/guide/expressions/objects)
- [字符串方法](/zh-hans/guide/expressions/strings)
- [代码节点](/zh-hans/guide/workflow/nodes/action-nodes/code)
