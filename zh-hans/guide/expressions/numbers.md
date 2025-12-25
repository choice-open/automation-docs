---
title: 数字方法
description: 数字数据转换方法参考
---

# 数字方法

数字方法用于处理和转换数字数据。

## 方法列表

### ceil()

将数字向上取整到下一个整数。

**返回值**: `Number`

**示例**:

```javascript
{{ (1.234).ceil() }}
// 2

{{ (3.2).ceil() }}
// 4

{{ (3.9).ceil() }}
// 4

{{ (-3.2).ceil() }}
// -3

{{ $('HTTP Request').body.price.ceil() }}
// 将价格向上取整
```

---

### floor()

将数字向下取整到最近的整数。

**返回值**: `Number`

**示例**:

```javascript
{{ (1.234).floor() }}
// 1

{{ (3.2).floor() }}
// 3

{{ (3.9).floor() }}
// 3

{{ (-3.2).floor() }}
// -4

{{ (-3.7).floor() }}
// -4

{{ $('HTTP Request').body.price.floor() }}
// 将价格向下取整
```

---

### isEven()

如果数字为偶数，返回 `true`，否则返回 `false`。如果数字不是整数，会抛出错误。

**返回值**: `Boolean`

**示例**:

```javascript
{{ (33).isEven() }}
// false

{{ (42).isEven() }}
// true

{{ (2).isEven() }}
// true

{{ (0).isEven() }}
// true

{{ (-4).isEven() }}
// true

{{ $('HTTP Request').body.count.isEven() }}
// 检查数量是否为偶数
```

---

### isOdd()

如果数字为奇数，返回 `true`，否则返回 `false`。如果数字不是整数，会抛出错误。

**返回值**: `Boolean`

**示例**:

```javascript
{{ (33).isOdd() }}
// true

{{ (42).isOdd() }}
// false

{{ (3).isOdd() }}
// true

{{ (1).isOdd() }}
// true

{{ (-3).isOdd() }}
// true

{{ $('HTTP Request').body.count.isOdd() }}
// 检查数量是否为奇数
```

---

### round(decimalPlaces?: Number)

将数字四舍五入到最近的整数（或指定的小数位数）。

**参数**:
- `decimalPlaces` (Number, 可选): 要四舍五入到的小数位数（默认：`0`）

**返回值**: `Number`

**示例**:

```javascript
// 四舍五入到整数
{{ (1.256).round() }}
// 1

{{ (3.4).round() }}
// 3

{{ (3.6).round() }}
// 4

// 保留指定小数位数
{{ (1.256).round(1) }}
// 1.3

{{ (1.256).round(2) }}
// 1.26

{{ (3.14159).round(2) }}
// 3.14

{{ $('HTTP Request').body.price.round(2) }}
// 将价格四舍五入到 2 位小数
```

---

### toBoolean()

对于 `0` 返回 `false`，对于任何其他数字（包括负数）返回 `true`。

**返回值**: `Boolean`

**示例**:

```javascript
{{ (12).toBoolean() }}
// true

{{ (0).toBoolean() }}
// false

{{ (-1.3).toBoolean() }}
// true

{{ (100).toBoolean() }}
// true

{{ $('HTTP Request').body.count.toBoolean() }}
// 将数字计数转换为布尔值
```

## 使用场景

### 1. 价格计算

```javascript
// 计算价格并四舍五入
{{ ($('HTTP Request').body.price * 1.1).round(2) }}
// 价格 + 10% 税，保留两位小数

// 向上取整到整数
{{ $('HTTP Request').body.price.ceil() }}
// 总是向上取整，不亏本
```

### 2. 奇偶判断

```javascript
// 根据奇偶数分配任务
{{ $('HTTP Request').body.taskId.isEven() ? 'Team A' : 'Team B' }}

// 检查是否可以平均分配
{{ $('HTTP Request').body.totalItems.isEven() }}
```

### 3. 数值转换

```javascript
// 将数字转换为布尔值用于条件判断
{{ $('HTTP Request').body.errorCount.toBoolean() }}
// 有错误返回 true，没有错误返回 false

// 向下取整获取整数部分
{{ $('HTTP Request').body.average.floor() }}
```

### 4. 数据格式化

```javascript
// 格式化统计数据
{{ $('HTTP Request').body.averageScore.round(1) }}
// 平均分保留 1 位小数

{{ $('HTTP Request').body.percentage.round(2) }}
// 百分比保留 2 位小数
```

## 链式调用

数字方法可以与其他方法链式调用：

```javascript
// 计算并格式化
{{ $('HTTP Request').body.prices
  .sum()
  .round(2) }}
// 求和后保留 2 位小数

// 四舍五入后检查是否为偶数
{{ $('HTTP Request').body.value
  .round()
  .isEven() }}
// 四舍五入到整数，然后检查是否为偶数
```

## 最佳实践

### 1. 处理货币

```javascript
// 总是使用 round 处理货币计算
{{ ($('HTTP Request').body.subtotal * 1.15).round(2) }}
// 避免浮点数精度问题
```

### 2. 整数检查

```javascript
// 在使用 isEven/isOdd 前确保是整数
{{ $('HTTP Request').body.value.floor().isEven() }}
```

### 3. 安全的布尔转换

```javascript
// 明确处理 0 值
{{ $('HTTP Request').body.count.toBoolean()
  ? '有数据'
  : '无数据' }}
```

### 4. 避免精度问题

```javascript
// 使用 round 避免浮点数精度问题
{{ (0.1 + 0.2).round(10) }}
// 0.3 而不是 0.30000000000000004
```

## 数学运算

除了这些方法，你还可以使用标准的数学运算符：

```javascript
// 基本运算
{{ 10 + 5 }}        // 15 (加法)
{{ 10 - 5 }}        // 5  (减法)
{{ 10 * 5 }}        // 50 (乘法)
{{ 10 / 5 }}        // 2  (除法)
{{ 10 % 3 }}        // 1  (取模)
{{ 2 ** 3 }}        // 8  (幂运算)

// 复杂计算
{{ ($('HTTP Request').body.price * 1.1).round(2) }}
// 价格加 10% 并四舍五入

{{ ($('HTTP Request').body.total / $('HTTP Request').body.count).round(2) }}
// 计算平均值
```

## 与数组方法结合

```javascript
// 计算数组平均值并格式化
{{ $('HTTP Request').body.scores
  .average()
  .round(1) }}

// 求和并向上取整
{{ $('HTTP Request').body.prices
  .sum()
  .ceil() }}

// 检查最大值是否为偶数
{{ $('HTTP Request').body.numbers
  .max()
  .isEven() }}
```

## 相关资源

- [表达式概述](/zh-hans/guide/expressions)
- [数组方法](/zh-hans/guide/expressions/arrays)
- [字符串方法](/zh-hans/guide/expressions/strings)
- [代码节点](/zh-hans/guide/workflow/nodes/action-nodes/code)
