# JavaScript

## Topic

1. [Variable](#variable)
2. [Data Type](#data-type)
3. [Equality Operator](#equality-operators)

## Variable

- Always use `let` and `const` for declare function
- `let` will only affect the own block  
  [Youtube: var, let and const explain](https://youtu.be/pobWEaHNChY?si=vhBjoMcmPvshTRzf)

### var

- function scope
- `var` variable can be overwritten by redeclare `var`

```javascript
var fruit = 'Apple';
console.log(fruit); // Output: Apple

fruit = 'Orange';
console.log(fruit); // Output: Orange
```

And `var` will still work outside of the `if-statement`

`var` does not respect block scoping, so it's accessible here

```javascript
let wizards = ['Merlin', 'Gandalf', 'Ursula', 'Radagst'];
('Urchins');
let items = ['Spellbook', 'Staff', 'Urchins', 'Pinecones'];
for (var item of wizards) {
  console.log(1, item);
  for (var item of items) {
    // Stuff happens...
  }
  console.log(2, item);
  // it will be changed by the second loop redeclare. So, second item is referencing 'items' array
}
```

---

### let

- can't be redeclared and reassigned
- block-scope

```javascript
let wizards = ['Merlin', 'Gandalf', 'Ursula', 'Radagst'];
('Urchins');
let items = ['Spellbook', 'Staff', 'Urchins', 'Pinecones'];
for (let item of wizards) {
  console.log(1, item);
  for (let item of items) {
    // Stuff happens...
  }
  console.log(2, item);
  // it will not be changed by the second loop redeclare. So, item is still referencing 'wizards' array
}
```

### const

- constant variable and can't be changed
- can't be redeclared and reassigned

## Data Type

- Dynamic data type

### Primitive Data Type

1. **String**
2. **Number**
3. **Boolean**
4. **BigInt**
5. **null**
6. **undefined**
7. **Symbol**

### Non-Primitive Data Type

1. **Object**

---

- `null` vs `undefine`

```javascript
// null
const food = null;
```

```javascript
// undefine
const food;
```

- declare `object`

```javascript
const student = { name: 'howard', age: 20, id: 's32312' };
```

- how to read `object`

```javascript
console.log(student.name); // output: howard

console.log(student['name']); // output: howard
```

```javascript
const property = 'name';

console.log(student[property]); // output: howard
```

## Equality Operators

- `=`: Assignment
- `==`: Loose Equality
- `===`: Strict Equality

### 1. `=`: Assignment

Used to assign values to variables.

```javascript
let x = 5; // Assigns 5 to x
let y = 'hello'; // Assigns "hello" to y
```

### 2. `==`: loose equality

check value only and not checking data type

```javascript
5 == '5'; // true, because "5" is coerced to a number
true == 1; // true, because true is coerced to 1
null == undefined; // true, because they are treated as equal
```

### 3. `===`: strict equality

check value and data type

```javascript
5 == '5'; // false
true == 1; // false
null == undefined; // false
```

## HTML methods
