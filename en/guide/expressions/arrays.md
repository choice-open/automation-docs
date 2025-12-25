---
title: Array Methods
description: Array data transformation methods reference
---

# Array Methods

Array methods are used to process and transform array data. All methods can be chained.

## Method Reference

### average()

Returns the average of the numbers in the array. Throws an error if there are any non-numbers. Returns `0` if the array is empty.

**Returns**: `Number`

**Examples**:

```javascript
{{ [12, 1, 5].average() }}
// 6

{{ [1, 2, 3, 4, 5].average() }}
// 3

{{ [].average() }}
// 0

{{ $('HTTP Request').body.prices.average() }}
// Calculate average price
```

---

### chunk(size: Number)

Splits the array into an array of sub-arrays, each with the given length. Throws an error if `size` is not a number or is zero.

**Parameters**:
- `size` (Number): The number of elements in each chunk (must be a non-zero number)

**Returns**: `Array`

**Examples**:

```javascript
{{ [1, 2, 3, 4, 5, 6].chunk(2) }}
// [[1, 2], [3, 4], [5, 6]]

{{ [1, 2, 3, 4, 5].chunk(2) }}
// [[1, 2], [3, 4], [5]]

{{ ['a', 'b', 'c', 'd', 'e', 'f'].chunk(3) }}
// [['a', 'b', 'c'], ['d', 'e', 'f']]

{{ $('HTTP Request').body.items.chunk(10) }}
// Process 10 items at a time
```

---

### compact()

Removes `null` and `undefined` values from an array. Recursively processes nested arrays and objects.

**Returns**: `Array`

**Examples**:

```javascript
{{ [1, null, 2, undefined, 3].compact() }}
// [1, 2, 3]

{{ [0, false, '', null, undefined, 'hello'].compact() }}
// [0, false, '', 'hello']
// Note: 0, false, '' are not removed

{{ [2, null, 1, undefined, '', 'nil', []].compact() }}
// [2, 1, '', 'nil', []]

{{ $('HTTP Request').body.users.compact() }}
// Remove null users
```

---

### difference(arr: Array)

Compares two arrays. Returns all elements in the base array that aren't present in `arr`. Duplicates are removed from the result.

**Parameters**:
- `arr` (Array): The array to compare to the base array

**Returns**: `Array`

**Examples**:

```javascript
{{ [1, 2, 3].difference([2, 3]) }}
// [1]

{{ [1, 2, 3, 4, 5].difference([3, 4, 5, 6, 7]) }}
// [1, 2]

{{ ['a', 'b', 'c'].difference(['b', 'c', 'd']) }}
// ['a']

{{ $('Current Users').users.difference($('Previous Users').users) }}
// Find new users
```

---

### first()

Returns the first element of an array. Returns `undefined` if array is empty.

**Returns**: `Array item`

**Examples**:

```javascript
{{ [1, 2, 3].first() }}
// 1

{{ ['apple', 'banana', 'cherry'].first() }}
// 'apple'

{{ [].first() }}
// undefined

{{ $('HTTP Request').body.items.first() }}
// First item
```

---

### intersection(arr: Array)

Compares two arrays and returns all elements that exist in both arrays. Duplicates are removed from the result.

**Parameters**:
- `arr` (Array): Array to compare against

**Returns**: `Array`

**Examples**:

```javascript
{{ [1, 2, 3].intersection([2, 3]) }}
// [2, 3]

{{ [1, 2, 3, 4, 5].intersection([3, 4, 5, 6, 7]) }}
// [3, 4, 5]

{{ ['a', 'b', 'c'].intersection(['b', 'c', 'd']) }}
// ['b', 'c']

{{ $('List A').items.intersection($('List B').items) }}
// Find common items
```

---

### isEmpty()

Returns `true` if the array has no elements or is `null`.

**Returns**: `Boolean`

**Examples**:

```javascript
{{ [].isEmpty() }}
// true

{{ ['quick', 'brown', 'fox'].isEmpty() }}
// false

{{ $('HTTP Request').body.items.isEmpty() }}
// Check if there are any items
```

---

### isNotEmpty()

Checks if an array contains elements.

**Returns**: `Boolean`

**Examples**:

```javascript
{{ [1, 2, 3].isNotEmpty() }}
// true

{{ [].isNotEmpty() }}
// false

{{ $('HTTP Request').body.users.isNotEmpty() }}
// Check if there are any users
```

---

### last()

Returns the last element of an array. Returns `undefined` if array is empty.

**Returns**: `Array item`

**Examples**:

```javascript
{{ [1, 2, 3].last() }}
// 3

{{ ['apple', 'banana', 'cherry'].last() }}
// 'cherry'

{{ [].last() }}
// undefined

{{ $('HTTP Request').body.items.last() }}
// Last item
```

---

### max()

Returns the largest number in the array. Throws an error if there are any non-numbers. String numbers are parsed using `parseFloat`.

**Returns**: `Number`

**Examples**:

```javascript
{{ [1, 12, 5].max() }}
// 12

{{ [1, 5, 3, 9, 2].max() }}
// 9

{{ [-10, -5, -20].max() }}
// -5

{{ $('HTTP Request').body.prices.max() }}
// Highest price
```

---

### merge(arr?: Array)

Merges two object arrays into one object by merging key-value pairs of corresponding elements. If no argument is provided, merges all objects within the array into a single object.

**Parameters**:
- `arr` (Array, optional): Array of objects to merge with the base array

**Returns**: `Object`

**Examples**:

```javascript
// Merge two object arrays
{{ [{ name: 'Nathan' }, { age: 42 }].merge([{ city: 'Berlin' }, { country: 'Germany' }]) }}
// { name: 'Nathan', age: 42, city: 'Berlin', country: 'Germany' }

// Merge all objects in array (no argument)
{{ [{ name: 'Alice' }, { age: 25 }, { city: 'New York' }].merge() }}
// { name: 'Alice', age: 25, city: 'New York' }

{{ $('List A').items.merge($('List B').items) }}
// Merge two object arrays
```

---

### min()

Returns the smallest number in the array. Throws an error if there are any non-numbers. String numbers are parsed using `parseFloat`.

**Returns**: `Number`

**Examples**:

```javascript
{{ [12, 1, 5].min() }}
// 1

{{ [1, 5, 3, 9, 2].min() }}
// 1

{{ [-10, -5, -20].min() }}
// -20

{{ $('HTTP Request').body.prices.min() }}
// Lowest price
```

---

### pluck(...fieldNames: String)

Returns an array containing the values of the given field(s) in each object of the array. Ignores any array elements that aren't objects or don't have keys matching the field name(s) provided. If multiple field names are provided, returns an array of arrays. If no field names are provided, returns the original array.

**Parameters**:
- `fieldNames` (String, variadic): The keys to retrieve the value of

**Returns**: `Array`

**Examples**:

```javascript
// Extract single field
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Jan', city: 'Berlin' }
].pluck('name') }}
// ['Nathan', 'Jan']

// Extract field that doesn't exist in all objects
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Jan', city: 'Berlin' }
].pluck('age') }}
// [42]

// Extract multiple fields
{{ [
  { name: 'Alice', age: 25, city: 'NYC' },
  { name: 'Bob', age: 30, city: 'LA' }
].pluck('name', 'age') }}
// [['Alice', 25], ['Bob', 30]]

// No arguments - returns original array
{{ [1, 2, 3].pluck() }}
// [1, 2, 3]

{{ $('HTTP Request').body.users.pluck('email') }}
// Extract all user emails
```

---

### randomItem()

Returns a randomly-chosen element from the array. Returns `undefined` if the array is empty or `undefined`.

**Returns**: `Array item` or `undefined`

**Examples**:

```javascript
{{ ['quick', 'brown', 'fox'].randomItem() }}
// Randomly returns one of: 'quick', 'brown', or 'fox'

{{ [1, 2, 3, 4, 5].randomItem() }}
// Randomly returns a number, e.g.: 3

{{ [].randomItem() }}
// undefined

{{ $('HTTP Request').body.quotes.randomItem() }}
// Randomly select a quote
```

---

### removeDuplicates(...fieldNames: String)

Removes duplicate elements from an array. Alias for `unique()`. For object arrays, you can specify one or more field names to check for equality.

**Parameters**:
- `fieldNames` (String, variadic, optional): For object arrays, specifies the field name(s) to use for duplicate detection

**Returns**: `Array`

**Examples**:

```javascript
// Deduplicate simple array
{{ [1, 2, 2, 3, 3, 3, 4].removeDuplicates() }}
// [1, 2, 3, 4]

// Object array, deduplicate by specified field
{{ [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 1, name: "Alice" }
].removeDuplicates("id") }}
// [
//   { "id": 1, "name": "Alice" },
//   { "id": 2, "name": "Bob" }
// ]

// Object array with multiple field names
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
// Deduplicate by email
```

---

### renameKeys(from1: String, to1: String, from2?: String, to2?: String, ...)

Changes all matching keys (field names) of any objects in the array. Rename more than one key by adding extra argument pairs: `from1, to1, from2, to2, ...`. Must provide an even number of arguments.

**Parameters**:
- `from1` (String): The key to rename
- `to1` (String): The new key name
- `from2, to2, ...` (String, optional): Additional key pairs to rename

**Returns**: `Array`

**Examples**:

```javascript
// Rename single key
{{ [
  { name: 'bob' },
  { name: 'meg' }
].renameKeys('name', 'x') }}
// [
//   { "x": "bob" },
//   { "x": "meg" }
// ]

// Rename multiple keys
{{ [
  { firstName: 'Alice', lastName: 'Smith', age: 25 }
].renameKeys('firstName', 'first', 'lastName', 'last') }}
// [
//   { "first": "Alice", "last": "Smith", "age": 25 }
// ]

{{ $('HTTP Request').body.users.renameKeys('user_id', 'id', 'user_name', 'name') }}
// Convert API field names to internal field names
```

---

### sum()

Returns the total of all the numbers in the array. Throws an error if there are any non-numbers. String numbers are parsed using `parseFloat`.

**Returns**: `Number`

**Examples**:

```javascript
{{ [12, 1, 5].sum() }}
// 18

{{ [1, 2, 3, 4, 5].sum() }}
// 15

{{ $('HTTP Request').body.prices.sum() }}
// Total price
```

---

### toJsonString()

Converts an array to a JSON string. Equivalent to `JSON.stringify`.

**Returns**: `String`

**Examples**:

```javascript
{{ [1, 2, 3].toJsonString() }}
// "[1,2,3]"

{{ [{ name: 'Alice' }, { name: 'Bob' }].toJsonString() }}
// '[{"name":"Alice"},{"name":"Bob"}]'

{{ $('HTTP Request').body.items.toJsonString() }}
// Convert array to JSON string for storage or transmission
```

---

### union(arr: Array)

Merges two arrays and removes duplicates.

**Parameters**:
- `arr` (Array): Array to merge

**Returns**: `Array`

**Examples**:

```javascript
{{ [1, 2, 3].union([3, 4, 5]) }}
// [1, 2, 3, 4, 5]

{{ ['a', 'b'].union(['b', 'c']) }}
// ['a', 'b', 'c']

{{ $('List A').items.union($('List B').items) }}
// Merge two lists and deduplicate
```

---

### unique(...fieldNames: String)

Removes any duplicate elements from the array. For object arrays, you can specify one or more field names to check for equality. Same functionality as `removeDuplicates()`.

**Parameters**:
- `fieldNames` (String, variadic, optional): For object arrays, specifies the field name(s) to use for duplicate detection

**Returns**: `Array`

**Examples**:

```javascript
// Simple array deduplication
{{ ['quick', 'brown', 'quick'].unique() }}
// ['quick', 'brown']

{{ [1, 1, 2, 2, 3].unique() }}
// [1, 2, 3]

// Object array without field names (compares entire objects)
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Nathan', age: 22 }
].unique() }}
// [
//   { "name": "Nathan", "age": 42 },
//   { "name": "Nathan", "age": 22 }
// ]
// Note: Objects are different, so both are kept

// Object array with single field name
{{ [
  { name: 'Nathan', age: 42 },
  { name: 'Nathan', age: 22 }
].unique('name') }}
// [
//   { "name": "Nathan", "age": 42 }
// ]

// Object array with multiple field names
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

## Method Chaining

Array methods can be chained:

```javascript
{{ $('HTTP Request').body.items
  .filter(item => item.active)
  .pluck('price')
  .compact()
  .average() }}
// Calculate average price of all active items
```

## Use Cases

### 1. Data Extraction

```javascript
// Extract single field from all objects
{{ $('Get Users').body.users.pluck('email') }}

// Extract multiple fields from all objects
{{ $('Get Users').body.users.pluck('name', 'email') }}

// Extract first and last items
{{ $('HTTP Request').body.items.first() }}
{{ $('HTTP Request').body.items.last() }}
```

### 2. Data Aggregation

```javascript
// Calculate total price
{{ $('HTTP Request').body.prices.sum() }}

// Calculate average score
{{ $('HTTP Request').body.scores.average() }}

// Find highest and lowest prices
{{ $('HTTP Request').body.prices.max() }}
{{ $('HTTP Request').body.prices.min() }}
```

### 3. Data Cleaning

```javascript
// Remove null values (recursively processes nested arrays and objects)
{{ $('HTTP Request').body.items.compact() }}

// Deduplicate by single field
{{ $('HTTP Request').body.users.unique('id') }}

// Deduplicate by multiple fields
{{ $('HTTP Request').body.users.unique('email', 'name') }}

// Rename single field
{{ $('HTTP Request').body.users.renameKeys('user_id', 'id') }}

// Rename multiple fields
{{ $('HTTP Request').body.users.renameKeys('user_id', 'id', 'user_name', 'name') }}
```

### 4. Data Merging

```javascript
// Merge object arrays into single object
{{ $('List A').items.merge($('List B').items) }}

// Merge all objects in array
{{ $('HTTP Request').body.items.merge() }}

// Merge and deduplicate
{{ $('List A').items.union($('List B').items) }}

// Find common items
{{ $('List A').items.intersection($('List B').items) }}

// Find differences
{{ $('List A').items.difference($('List B').items) }}
```

### 5. Data Grouping

```javascript
// Split array into chunks for processing
{{ $('HTTP Request').body.items.chunk(10) }}
// Process 10 items at a time
```

## Best Practices

### 1. Check if Array is Empty

```javascript
// Check before processing
{{ $('HTTP Request').body.items.isNotEmpty() }}
```

### 2. Handle Potentially Missing Fields

```javascript
// Use compact to remove null values
{{ $('HTTP Request').body.users.pluck('email').compact() }}
```

### 3. Combine Multiple Methods

```javascript
// Chain multiple methods
{{ $('HTTP Request').body.items
  .compact()
  .unique('id')
  .pluck('name') }}
```

### 4. Performance Considerations

```javascript
// Filter first to reduce data volume before processing
{{ $('HTTP Request').body.items
  .filter(item => item.active)
  .pluck('price')
  .sum() }}
```

## Related Resources

- [Expressions Overview](/en/guide/expressions)
- [Object Methods](/en/guide/expressions/objects)
- [String Methods](/en/guide/expressions/strings)
- [Code Node](/en/guide/workflow/nodes/action-nodes/code)
