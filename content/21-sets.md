[&#8678; get back to the start...](../README.md)
# Sets

```js
const str = 'hello world'
const set = new Set(str);
// Set(8) {"h", "e", "l", "o", " ", "w", "r", "d"}
```

```js
const arr = [1, 2, 3, 1, 2, 5];
const set = new Set(arr); // Set(4) {1, 2, 3, 5}

set[0] // undefined
[...set] // (4) [1, 2, 3, 5] 
[...set][0] // 1

set.size // 4
set.has(5) // true
set.add(15) // Set(5) {1, 2, 3, 5, 15}
set.delete(3) // true
set // Set(4) {1, 2, 5, 15}
set.clear()
set // Set(0) {}
```

```js
for (const item of set) {
  console.log(item)
}

// 1 2 5 15
```