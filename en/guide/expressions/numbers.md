---
title: Number Methods
description: Number data transformation methods reference
---

# Number Methods

Number methods are used to process and transform numeric data.

## Method Reference

### ceil()

Rounds the number up to the next whole number.

**Returns**: `Number`

**Examples**:

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
// Round price up to nearest integer
```

---

### floor()

Rounds the number down to the nearest whole number.

**Returns**: `Number`

**Examples**:

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
// Round price down to nearest integer
```

---

### isEven()

Returns `true` if the number is even or `false` if not. Throws an error if the number isn't a whole number.

**Returns**: `Boolean`

**Examples**:

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
// Check if count is even
```

---

### isOdd()

Returns `true` if the number is odd or `false` if not. Throws an error if the number isn't a whole number.

**Returns**: `Boolean`

**Examples**:

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
// Check if count is odd
```

---

### round(decimalPlaces?: Number)

Rounds the number to the nearest integer (or decimal place).

**Parameters**:
- `decimalPlaces` (Number, optional): The number of decimal places to round to (default: `0`)

**Returns**: `Number`

**Examples**:

```javascript
// Round to integer
{{ (1.256).round() }}
// 1

{{ (3.4).round() }}
// 3

{{ (3.6).round() }}
// 4

// Preserve specified decimal places
{{ (1.256).round(1) }}
// 1.3

{{ (1.256).round(2) }}
// 1.26

{{ (3.14159).round(2) }}
// 3.14

{{ $('HTTP Request').body.price.round(2) }}
// Round price to 2 decimal places
```

---

### toBoolean()

Returns `false` for `0` and `true` for any other number (including negative numbers).

**Returns**: `Boolean`

**Examples**:

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
// Convert number count to boolean
```

## Use Cases

### 1. Price Calculations

```javascript
// Calculate price and round
{{ ($('HTTP Request').body.price * 1.1).round(2) }}
// Price + 10% tax, preserve two decimal places

// Round up to integer
{{ $('HTTP Request').body.price.ceil() }}
// Always round up, never lose money
```

### 2. Even/Odd Check

```javascript
// Assign tasks based on even/odd
{{ $('HTTP Request').body.taskId.isEven() ? 'Team A' : 'Team B' }}

// Check if can be evenly distributed
{{ $('HTTP Request').body.totalItems.isEven() }}
```

### 3. Value Conversion

```javascript
// Convert number to boolean for conditional logic
{{ $('HTTP Request').body.errorCount.toBoolean() }}
// Returns true if there are errors, false if no errors

// Floor to get integer part
{{ $('HTTP Request').body.average.floor() }}
```

### 4. Data Formatting

```javascript
// Format statistical data
{{ $('HTTP Request').body.averageScore.round(1) }}
// Average score with 1 decimal place

{{ $('HTTP Request').body.percentage.round(2) }}
// Percentage with 2 decimal places
```

## Method Chaining

Number methods can be chained with other methods:

```javascript
// Calculate and format
{{ $('HTTP Request').body.prices
  .sum()
  .round(2) }}
// Sum then preserve 2 decimal places

// Round then check if even
{{ $('HTTP Request').body.value
  .round()
  .isEven() }}
// Round to integer, then check if even
```

## Best Practices

### 1. Handle Currency

```javascript
// Always use round for currency calculations
{{ ($('HTTP Request').body.subtotal * 1.15).round(2) }}
// Avoid floating-point precision issues
```

### 2. Integer Check

```javascript
// Ensure integer before using isEven/isOdd
{{ $('HTTP Request').body.value.floor().isEven() }}
```

### 3. Safe Boolean Conversion

```javascript
// Explicitly handle 0 value
{{ $('HTTP Request').body.count.toBoolean()
  ? 'Has data'
  : 'No data' }}
```

### 4. Avoid Precision Issues

```javascript
// Use round to avoid floating-point precision issues
{{ (0.1 + 0.2).round(10) }}
// 0.3 instead of 0.30000000000000004
```

## Math Operations

In addition to these methods, you can use standard math operators:

```javascript
// Basic operations
{{ 10 + 5 }}        // 15 (addition)
{{ 10 - 5 }}        // 5  (subtraction)
{{ 10 * 5 }}        // 50 (multiplication)
{{ 10 / 5 }}        // 2  (division)
{{ 10 % 3 }}        // 1  (modulo)
{{ 2 ** 3 }}        // 8  (exponentiation)

// Complex calculations
{{ ($('HTTP Request').body.price * 1.1).round(2) }}
// Add 10% to price and round

{{ ($('HTTP Request').body.total / $('HTTP Request').body.count).round(2) }}
// Calculate average
```

## Combine with Array Methods

```javascript
// Calculate array average and format
{{ $('HTTP Request').body.scores
  .average()
  .round(1) }}

// Sum and round up
{{ $('HTTP Request').body.prices
  .sum()
  .ceil() }}

// Check if max value is even
{{ $('HTTP Request').body.numbers
  .max()
  .isEven() }}
```

## Related Resources

- [Expressions Overview](/en/guide/expressions)
- [Array Methods](/en/guide/expressions/arrays)
- [String Methods](/en/guide/expressions/strings)
- [Code Node](/en/guide/workflow/nodes/action-nodes/code)
