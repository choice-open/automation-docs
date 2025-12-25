---
title: Object Methods
description: Object data transformation methods reference
---

# Object Methods

Object methods are used to process and transform object data.

## Method Reference

### compact()

Removes all fields that have `null` or `undefined` values. Recursively processes nested objects.

**Returns**: `Object`

**Examples**:

```javascript
{{ ({ x: null, y: 2, z: '', w: 'nil', a: {} }).compact() }}
// { y: 2, z: '', w: 'nil', a: {} }

{{ { a: 1, b: null, c: undefined, d: "hello" }.compact() }}
// { "a": 1, "d": "hello" }

{{ { a: 0, b: "", c: [], d: {} }.compact() }}
// { "a": 0, "b": "", "c": [], "d": {} }
// Note: 0, empty string, empty array, empty object are not removed

{{ $('HTTP Request').body.user.compact() }}
// Remove null and undefined values from user object (recursively)
```

---

### hasField(fieldName: String)

Returns `true` if there is a field called `name`. Only checks top-level keys. Comparison is case-sensitive.

**Parameters**:
- `fieldName` (String): The name of the key to search for

**Returns**: `Boolean`

**Examples**:

```javascript
{{ ({ name: 'Nathan', age: 42 }).hasField('name') }}
// true

{{ ({ name: 'Nathan', age: 42 }).hasField('Name') }}
// false

{{ ({ name: 'Nathan', age: 42 }).hasField('inventedField') }}
// false

{{ $('HTTP Request').body.user.hasField('email') }}
// Check if user object contains email field
```

---

### isEmpty()

Returns `true` if the Object has no keys (fields) set or is `null`.

**Returns**: `Boolean`

**Examples**:

```javascript
{{ ({ name: 'Nathan' }).isEmpty() }}
// false

{{ ({}).isEmpty() }}
// true

{{ { name: 'Alice' }.isEmpty() }}
// false

{{ $('HTTP Request').body.config.isEmpty() }}
// Check if config object is empty
```

---

### isNotEmpty()

Returns `true` if the Object has at least one key (field) set.

**Returns**: `Boolean`

**Examples**:

```javascript
{{ ({ name: 'Nathan' }).isNotEmpty() }}
// true

{{ ({}).isNotEmpty() }}
// false

{{ $('HTTP Request').body.config.isNotEmpty() }}
// Check if config object has fields
```

---

### keepFieldsContaining(value: String)

Removes any fields whose values don't at least partly match the given `value`. Comparison is case-sensitive. Fields that aren't strings will always be removed. Throws an error if the argument is not a string.

**Parameters**:
- `value` (String): The text that a value must contain in order to be kept

**Returns**: `Object`

**Examples**:

```javascript
{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).keepFieldsContaining('Nathan') }}
// { name: 'Mr Nathan' }

{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).keepFieldsContaining('nathan') }}
// {}
// Case-sensitive, so 'Nathan' doesn't match 'nathan'

{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).keepFieldsContaining('han') }}
// { name: 'Mr Nathan', city: 'hanoi' }

{{ $('HTTP Request').body.data.keepFieldsContaining("prod") }}
// Keep only production-related fields
```

---

### merge(object: Object)

Merges two objects into a new object using the current object as the base. If both objects share a key, the base object's value is kept. Throws an error if the argument is not an object or is an array.

**Parameters**:
- `object` (Object): The object to merge into the base object

**Returns**: `Object`

**Examples**:

```javascript
// Base object values take priority
{{ ({ a: 1, b: 2 }).merge({ b: 3, c: 4 }) }}
// { a: 1, b: 2, c: 4 }

{{ { name: 'Alice', age: 25 }.merge({ age: 30, city: 'Beijing' }) }}
// { "name": "Alice", "age": 25, "city": "Beijing" }

{{ $('HTTP Request').body.user.merge($('Default Config').defaults) }}
// Merge default config into user data
```

---

### removeField(key: String)

Removes a field from the Object. The same as JavaScript's `delete`. Returns the original object if the field doesn't exist.

**Parameters**:
- `key` (String): The name of the field to remove

**Returns**: `Object`

**Examples**:

```javascript
{{ ({ name: 'Nathan', city: 'hanoi' }).removeField('name') }}
// { city: 'hanoi' }

{{ { name: 'Alice', age: 25, password: '123456' }.removeField('password') }}
// { "name": "Alice", "age": 25 }

{{ $('HTTP Request').body.user.removeField('internalId') }}
// Remove internal ID field
```

---

### removeFieldsContaining(value: String)

Removes keys (fields) whose values at least partly match the given `value`. Comparison is case-sensitive. Fields that aren't strings are always kept. Throws an error if the argument is not a string.

**Parameters**:
- `value` (String): The text that a value must contain in order to be removed

**Returns**: `Object`

**Examples**:

```javascript
{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).removeFieldsContaining('Nathan') }}
// { city: 'hanoi', age: 42 }

{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).removeFieldsContaining('Han') }}
// { age: 42 }

{{ ({ name: 'Mr Nathan', city: 'hanoi', age: 42 }).removeFieldsContaining('nathan') }}
// { name: 'Mr Nathan', city: 'hanoi', age: 42 }
// Case-sensitive, so 'Nathan' doesn't match 'nathan'

{{ $('HTTP Request').body.data.removeFieldsContaining("temp") }}
// Remove all temporary fields containing "temp"
```

---

### toJsonString()

Converts the Object to a JSON string. Similar to JavaScript's `JSON.stringify()`.

**Returns**: `String`

**Examples**:

```javascript
{{ ({ name: 'Mr Nathan', age: 42 }).toJsonString() }}
// '{"name":"Nathan","age":42}'

{{ { name: 'Alice', age: 25 }.toJsonString() }}
// '{"name":"Alice","age":25}'

{{ $('HTTP Request').body.user.toJsonString() }}
// Convert user object to JSON string
```

---

### urlEncode()

Generates a URL parameter string from the Object's keys and values. Only top-level keys are supported.

**Returns**: `String`

**Examples**:

```javascript
{{ ({ name: 'Mr Nathan', city: 'hanoi' }).urlEncode() }}
// 'name=Mr+Nathan&city=hanoi'

{{ { name: 'Alice', age: 25 }.urlEncode() }}
// "name=Alice&age=25"

{{ { q: 'hello world', page: 1 }.urlEncode() }}
// "q=hello+world&page=1"

{{ $('HTTP Request').body.params.urlEncode() }}
// Convert params object to URL query string
```

## Use Cases

### 1. Object Merging

```javascript
// Merge user input with default config
{{ $('User Input').config.merge({
  timeout: 30000,
  retries: 3,
  debug: false
}) }}

// Merge multiple data sources
{{ $('Source A').data.merge($('Source B').data) }}
```

### 2. Field Filtering

```javascript
// Remove sensitive fields
{{ $('HTTP Request').body.user
  .removeField('password')
  .removeField('token') }}

// Keep only specific fields
{{ $('HTTP Request').body.data.keepFieldsContaining("public") }}

// Remove test fields
{{ $('HTTP Request').body.data.removeFieldsContaining("test") }}
```

### 3. Data Cleaning

```javascript
// Remove null values
{{ $('HTTP Request').body.formData.compact() }}

// Remove unwanted fields
{{ $('HTTP Request').body.user
  .removeField('_id')
  .removeField('__v')
  .compact() }}
```

### 4. Data Conversion

```javascript
// Convert to JSON string for storage
{{ $('HTTP Request').body.config.toJsonString() }}

// Convert to URL parameters
{{ $('HTTP Request').body.filters.urlEncode() }}
```

### 5. Field Validation

```javascript
// Check required fields
{{ $('HTTP Request').body.user.hasField('email')
  && $('HTTP Request').body.user.hasField('name') }}

// Check if object is empty
{{ $('HTTP Request').body.result.isEmpty() }}

// Check if object has fields
{{ $('HTTP Request').body.result.isNotEmpty() }}
```

## Method Chaining

Object methods can be chained:

```javascript
// Clean and merge objects
{{ $('HTTP Request').body.user
  .compact()
  .removeField('password')
  .merge({ updatedAt: $now() }) }}

// Filter and convert
{{ $('HTTP Request').body.config
  .keepFieldsContaining("prod")
  .compact()
  .toJsonString() }}
```

## Nested Object Handling

Object methods primarily handle top-level keys. For nested objects, use Code nodes:

```javascript
// Handle nested objects in Code node
const user = $('HTTP Request').body.user;

// Process nested fields
if (user.profile && user.profile.settings) {
  delete user.profile.settings.internalFlag;
}

return user;
```

## Best Practices

### 1. Pay Attention to Order When Merging

```javascript
// Base object values take priority
{{ $('User Data').config.merge($('Default Config').defaults) }}
// User Data values override Default Config values
```

### 2. Use compact to Remove Null Values

```javascript
// Clean data before sending API request
{{ $('HTTP Request').body.params.compact() }}
```

### 3. Remove Sensitive Information

```javascript
// Remove sensitive fields before logging
{{ $('HTTP Request').body.user
  .removeField('password')
  .removeField('creditCard')
  .removeField('ssn') }}
```

### 4. Check Field Existence

```javascript
// Check if field exists before accessing
{{ $('HTTP Request').body.user.hasField('email')
  ? $('HTTP Request').body.user.email
  : 'no-email@example.com' }}
```

### 5. Use urlEncode to Build Query Strings

```javascript
// Build API query parameters
{{ `https://api.example.com/search?${$('HTTP Request').body.filters.urlEncode()}` }}
```

## Combine with Other Methods

```javascript
// Get array of object keys
{{ Object.keys($('HTTP Request').body.user) }}

// Get array of object values
{{ Object.values($('HTTP Request').body.prices) }}

// Use object methods in Code node
const data = $('HTTP Request').body.data;
const cleaned = data.compact().removeField('_internal');
return cleaned;
```

## Related Resources

- [Expressions Overview](/en/guide/expressions)
- [Array Methods](/en/guide/expressions/arrays)
- [String Methods](/en/guide/expressions/strings)
- [Code Node](/en/guide/workflow/nodes/action-nodes/code)
