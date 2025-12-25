---
title: 日期方法
description: 日期时间数据转换方法参考
---

# 日期方法

日期方法用于处理和转换日期时间数据。{{PRODUCT_NAME}} 使用 Luxon 库进行日期时间处理。

## 方法列表

### beginningOf(unit?: DurationUnit)

将日期转换为给定时间段的开始。默认单位是 `week`。返回值根据输入类型，可为 JavaScript Date 或 Luxon DateTime。

**参数**:
- `unit` (DurationUnit, 可选): 指定时间单位的有效字符串。枚举值：`second`、`minute`、`hour`、`day`、`week`、`month`、`year`。默认值为 `week`。

**返回值**: `Date | DateTime`

**示例**:

```javascript
// 假设当前时间为 2025-09-18T15:30:45.123-04:00

{{ $now().beginningOf('day') }}
// 2025-09-18T00:00:00.000-04:00

{{ $now().beginningOf('week') }}
// 2025-09-15T00:00:00.000-04:00 (本周一)

{{ $now().beginningOf('month') }}
// 2025-09-01T00:00:00.000-04:00

{{ $now().beginningOf('year') }}
// 2025-01-01T00:00:00.000-04:00

{{ $('HTTP Request').body.createdAt.beginningOf('day') }}
// 获取创建日期的当天开始时间
```

---

### endOfMonth()

将日期转换为该月份内最后一个可能的时刻。返回值根据输入类型，可为 JavaScript Date 或 Luxon DateTime。

**返回值**: `Date | DateTime`

**示例**:

```javascript
// 假设当前时间为 2025-09-18T15:30:45.123-04:00

{{ $now().endOfMonth() }}
// 2025-09-30T23:59:59.999-04:00

{{ $('HTTP Request').body.startDate.endOfMonth() }}
// 获取月底时间
```

---

### extract(datePart?: DatePart)

提取日期或时间的部分，例如月份，作为数字。要提取文本名称，请参见 `format()`。默认单位是 `week`。

**参数**:
- `datePart` (DatePart, 可选): 要返回的日期或时间部分。枚举值：`year`、`month`、`week`、`day`、`hour`、`minute`、`second`、`millisecond`、`weekNumber`、`yearDayNumber`、`weekday`。默认值为 `week`。

**返回值**: `Number`

**示例**:

```javascript
// 假设日期为 2024-03-30T18:49:00.000Z

{{ $now().extract('year') }}
// 2024

{{ $now().extract('month') }}
// 3

{{ $now().extract('day') }}
// 30

{{ $now().extract('hour') }}
// 18

{{ $now().extract('minute') }}
// 49

{{ $now().extract('second') }}
// 0

{{ $now().extract('millisecond') }}
// 0

{{ $now().extract('weekNumber') }}
// 年份中的周数

{{ $now().extract('yearDayNumber') }}
// 年份中的天数（1-365/366）

{{ $now().extract('weekday') }}
// 星期几（1-7，星期一=1）

{{ $('HTTP Request').body.createdAt.extract('year') }}
// 提取创建年份
```

---

### format(fmt: TimeFormat, localeOpts?: LocaleOptions)

使用指定的格式将 DateTime 转换为字符串。参见 [Luxon 格式化标记](https://moment.github.io/luxon/#/formatting?id=table-of-tokens)。对于常见格式，`toLocaleString()` 可能更容易。

**参数**:
- `fmt` (TimeFormat): 要返回的字符串格式。默认：`'yyyy-MM-dd'`。参见 [Luxon 格式化标记](https://moment.github.io/luxon/#/formatting?id=table-of-tokens)。
- `localeOpts` (LocaleOptions, 可选): 格式化的区域设置选项。

**返回值**: `String`

**常用格式标记**:
- `yyyy` - 4位年份 (2025)
- `yy` - 2位年份 (25)
- `MM` - 2位月份 (01-12)
- `M` - 月份 (1-12)
- `dd` - 2位日期 (01-31)
- `d` - 日期 (1-31)
- `HH` - 24小时制小时 (00-23)
- `hh` - 12小时制小时 (01-12)
- `mm` - 分钟 (00-59)
- `ss` - 秒 (00-59)
- `a` - 上午/下午 (am/pm)

**示例**:

```javascript
{{ $now().format('yyyy-MM-dd') }}
// "2025-09-18"

{{ $now().format('yyyy-MM-dd HH:mm:ss') }}
// "2025-09-18 15:30:45"

{{ $now().format('dd/MM/yyyy') }}
// "18/09/2025"

{{ $now().format('MMM dd, yyyy') }}
// "Sep 18, 2025"

{{ $now().format('yyyy年MM月dd日') }}
// "2025年09月18日"

{{ $now().format('HH:mm') }}
// "15:30"

{{ $now().format('hh:mm a') }}
// "03:30 pm"

{{ $now().format('dd/LL/yyyy') }}
// "30/04/2024"

{{ $now().format('dd LLL yy') }}
// "30 Apr 24"

{{ $now().setLocale('fr').format('dd LLL yyyy') }}
// "30 avr. 2024"

{{ $now().format("HH 'hours and' mm 'minutes'") }}
// "18 hours and 49 minutes"

{{ $('HTTP Request').body.createdAt.format('yyyy-MM-dd') }}
// 格式化创建日期
```

---

### isBetween(date1: Date | DateTime | String, date2: Date | DateTime | String)

如果 DateTime 位于指定的两个时刻之间，返回 `true`。需要恰好两个参数。可以是 ISO 日期字符串或 Luxon DateTime。

**参数**:
- `date1` (Date | DateTime | String): 基础 DateTime 必须在其之后的时刻。可以是 ISO 日期字符串或 Luxon DateTime。
- `date2` (Date | DateTime | String): 基础 DateTime 必须在其之前的时刻。可以是 ISO 日期字符串或 Luxon DateTime。

**返回值**: `Boolean`

**示例**:

```javascript
// 使用 ISO 日期字符串
{{ $now().isBetween('2020-01-01', '2025-12-31') }}
// true

{{ $now().isBetween('2020', '2025') }}
// true

// 使用 DateTime 对象
{{ $('HTTP Request').body.eventDate.isBetween(
  $('HTTP Request').body.startDate,
  $('HTTP Request').body.endDate
) }}
// 检查事件日期是否在指定范围内
```

---

### isInLast(n?: Number, unit?: DurationUnit)

检查日期是否在给定的时间段内。默认单位是 `minutes`。

**参数**:
- `n` (Number, 可选): 单位数。例如，要检查日期是否在过去 9 周内，请输入 9。默认值：0
- `unit` (DurationUnit, 可选): 指定时间单位的有效字符串。枚举值：`milliseconds`、`seconds`、`minutes`、`hours`、`days`、`weeks`、`months`、`years`。默认为 `minutes`。

**返回值**: `Boolean`

**示例**:

```javascript
// 检查是否在过去 7 天内
{{ $('HTTP Request').body.createdAt.isInLast(7, 'day') }}

// 检查是否在过去 1 小时内
{{ $('HTTP Request').body.lastLogin.isInLast(1, 'hour') }}

// 检查是否在过去 30 分钟内
{{ $('HTTP Request').body.timestamp.isInLast(30, 'minute') }}

// 检查是否在过去 3 个月内
{{ $('HTTP Request').body.orderDate.isInLast(3, 'month') }}
```

---

### isWeekend()

检查日期是否为星期六或星期日。

**返回值**: `Boolean`

**示例**:

```javascript
{{ $now().isWeekend() }}
// 检查今天是否为周末

{{ $('HTTP Request').body.eventDate.isWeekend() }}
// 检查事件日期是否在周末

// 用于条件判断
{{ $('HTTP Request').body.deliveryDate.isWeekend()
  ? '周末配送加收费用'
  : '工作日配送' }}
```

---

### minus(n: Number | DurationLike, unit?: DurationUnit)

从 DateTime 中减去给定的时间段。要减去的单位数，或使用 Luxon [Duration](https://moment.github.io/luxon/api-docs/index.html#duration) 对象一次减去多个单位。

**参数**:
- `n` (Number | DurationLike): 要减去的单位数，或 Luxon Duration 对象。
- `unit` (DurationUnit, 可选): 数字的单位。枚举值：`years`、`months`、`weeks`、`days`、`hours`、`minutes`、`seconds`、`milliseconds`。默认值：`milliseconds`。

**返回值**: `Date | DateTime`

**示例**:

```javascript
// 减去天数
{{ $now().minus(7, 'day') }}
// 7天前

{{ $now().minus(1, 'week') }}
// 1周前

{{ $now().minus(3, 'month') }}
// 3个月前

{{ $now().minus(1, 'year') }}
// 1年前

// 减去小时/分钟
{{ $now().minus(2, 'hour') }}
// 2小时前

{{ $now().minus(30, 'minute') }}
// 30分钟前

{{ $('HTTP Request').body.expiryDate.minus(7, 'day') }}
// 在到期日期前 7 天
```

---

### plus(n: Number | DurationLike, unit?: DurationUnit)

向 DateTime 添加给定的时间段。要添加的单位数，或使用 Luxon [Duration](https://moment.github.io/luxon/api-docs/index.html#duration) 对象一次添加多个单位。

**参数**:
- `n` (Number | DurationLike): 要添加的单位数，或 Luxon Duration 对象。
- `unit` (DurationUnit, 可选): 数字的单位。枚举值：`years`、`months`、`weeks`、`days`、`hours`、`minutes`、`seconds`、`milliseconds`。默认值：`milliseconds`。

**返回值**: `Date | DateTime`

**示例**:

```javascript
// 添加天数
{{ $now().plus(7, 'day') }}
// 7天后

{{ $now().plus(2, 'week') }}
// 2周后

{{ $now().plus(3, 'month') }}
// 3个月后

{{ $now().plus(1, 'year') }}
// 1年后

// 添加小时/分钟
{{ $now().plus(2, 'hour') }}
// 2小时后

{{ $now().plus(30, 'minute') }}
// 30分钟后

{{ $('HTTP Request').body.startDate.plus(14, 'day') }}
// 在开始日期后 14 天
```

## 内置日期函数

### $now()

返回当前日期时间。

**示例**:

```javascript
{{ $now() }}
// 当前时间

{{ $now().format('yyyy-MM-dd') }}
// 今天日期

{{ $now().plus(1, 'day').format('yyyy-MM-dd') }}
// 明天日期
```

### $today()

返回今天的日期（时间设为 00:00:00）。

**示例**:

```javascript
{{ $today() }}
// 今天 00:00:00

{{ $today().format('yyyy-MM-dd') }}
// 今天日期
```

## 使用场景

### 1. 日期范围查询

```javascript
// 查询过去 7 天的数据
{{ {
  startDate: $now().minus(7, 'day').format('yyyy-MM-dd'),
  endDate: $now().format('yyyy-MM-dd')
} }}

// 查询本月数据
{{ {
  startDate: $now().beginningOf('month').format('yyyy-MM-dd'),
  endDate: $now().endOfMonth().format('yyyy-MM-dd')
} }}
```

### 2. 日期格式化

```javascript
// API 通常需要 ISO 格式
{{ $('HTTP Request').body.createdAt.format('yyyy-MM-dd\'T\'HH:mm:ss') }}

// 用户友好的显示格式
{{ $('HTTP Request').body.createdAt.format('yyyy年MM月dd日 HH:mm') }}
```

### 3. 日期计算

```javascript
// 计算到期日（购买后 30 天）
{{ $('HTTP Request').body.purchaseDate.plus(30, 'day') }}

// 计算试用期结束日（注册后 14 天）
{{ $('HTTP Request').body.registeredAt.plus(14, 'day') }}

// 计算提醒时间（截止日期前 3 天）
{{ $('HTTP Request').body.deadline.minus(3, 'day') }}
```

### 4. 日期验证

```javascript
// 检查是否在有效期内
{{ $('HTTP Request').body.currentDate.isBetween(
  $('HTTP Request').body.validFrom,
  $('HTTP Request').body.validUntil
) }}

// 检查是否是最近创建的
{{ $('HTTP Request').body.createdAt.isInLast(24, 'hour') }}

// 检查是否为周末
{{ $('HTTP Request').body.deliveryDate.isWeekend() }}
```

### 5. 提取日期部分

```javascript
// 提取年月日用于统计
{{ {
  year: $('HTTP Request').body.orderDate.extract('year'),
  month: $('HTTP Request').body.orderDate.extract('month'),
  day: $('HTTP Request').body.orderDate.extract('day')
} }}

// 按小时统计
{{ $('HTTP Request').body.timestamp.extract('hour') }}
```

## 链式调用

日期方法可以链式调用：

```javascript
// 获取下个月第一天
{{ $now()
  .plus(1, 'month')
  .beginningOf('month')
  .format('yyyy-MM-dd') }}

// 获取上周一
{{ $now()
  .minus(1, 'week')
  .beginningOf('week')
  .format('yyyy-MM-dd') }}

// 获取去年同期日期
{{ $('HTTP Request').body.date
  .minus(1, 'year')
  .format('yyyy-MM-dd') }}
```

## 时区处理

{{PRODUCT_NAME}} 自动处理时区转换。日期时间始终包含时区信息。

```javascript
// 日期会自动转换为用户时区
{{ $('HTTP Request').body.utcDate.format('yyyy-MM-dd HH:mm:ss') }}

// 提取的时间部分会考虑时区
{{ $('HTTP Request').body.utcDate.extract('hour') }}
```

## 最佳实践

### 1. 始终格式化用于 API 的日期

```javascript
// 好的做法 - 明确格式
{{ $now().format('yyyy-MM-dd') }}

// 不好的做法 - 依赖默认格式
{{ $now() }}
```

### 2. 使用有意义的时间单位

```javascript
// 好的做法 - 清晰的意图
{{ $now().minus(7, 'day') }}

// 不好的做法 - 难以理解
{{ $now().minus(168, 'hour') }}
```

### 3. 处理月底日期

```javascript
// 使用 endOfMonth 而不是硬编码日期
{{ $now().endOfMonth() }}

// 而不是
{{ $now().plus(30, 'day') }} // 不准确
```

### 4. 组合方法获得精确时间

```javascript
// 获取本月第一天的开始时间
{{ $now().beginningOf('month') }}

// 获取今天的结束时间
{{ $now().beginningOf('day').plus(1, 'day').minus(1, 'millisecond') }}
```

## 常见日期格式

### ISO 8601 标准格式

```javascript
{{ $now().format('yyyy-MM-dd\'T\'HH:mm:ss') }}
// "2025-09-18T15:30:45"
```

### 中国常用格式

```javascript
{{ $now().format('yyyy年MM月dd日') }}
// "2025年09月18日"

{{ $now().format('yyyy-MM-dd HH:mm:ss') }}
// "2025-09-18 15:30:45"
```

### 美国常用格式

```javascript
{{ $now().format('MM/dd/yyyy') }}
// "09/18/2025"

{{ $now().format('MMM dd, yyyy') }}
// "Sep 18, 2025"
```

### 欧洲常用格式

```javascript
{{ $now().format('dd/MM/yyyy') }}
// "18/09/2025"

{{ $now().format('dd.MM.yyyy') }}
// "18.09.2025"
```

## 相关资源

- [表达式概述](/zh-hans/guide/expressions)
- [数组方法](/zh-hans/guide/expressions/arrays)
- [字符串方法](/zh-hans/guide/expressions/strings)
- [Luxon 文档](https://moment.github.io/luxon/)
- [代码节点](/zh-hans/guide/workflow/nodes/action-nodes/code)
