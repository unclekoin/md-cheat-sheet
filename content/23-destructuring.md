[&#8678; get back to the start...](../README.md)
# Destructuring
## Arrays
```js
const [a, b, ...rest] = [10, 20, 30, 40, 50];
a // 10
b // 20
rest // [30, 40, 50]
```
### Ignoring values
```js
const [a, , b] = [1, 2, 3];
a // 1
b // 3
```
### Default values
```js
let a, b;

[a = 5, b = 7] = [1];
a // 1
b // 7
```
### Swapping variables
```js
let a = 1;
let b = 3;

[a, b] = [b, a];
a // 3
b // 1

const array = [1, 2, 3];
[array[2], array[1]] = [array[1], array[2]];
array // [1, 3, 2]
```
### Return from a function
```js
function f() {
  return [1, 2];
}

let a, b;
[a, b] = f();
a // 1
b // 2
```

## Objects
```js
const {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
a // 10
b // 20
rest // {c: 30, d: 40}
```
### New variable names
```js
const {p: x, q: y} = {p: 42, q: true};
x // 42
y // true
```
### Default values
```js
const {a = 10, b = 5} = {a: 3};
a // 3
b // 5

const {a: aa = 10, b: bb = 5} = {a: 3};
aa // 3
bb // 5
```
### Nested object
```js
const object = {
  a: {c: 2, d: 3},
  b: {e: 5, f: 7}
}

const {a: {c, d}, b: {e, f}} = object;
c // 2
d // 3
e // 5
f // 7
```