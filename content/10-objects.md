[&#8678; get back to the start...](../README.md)
# Objects

```js
const object {name: 'Max'};

const key = 'name';

consosle.log(object.name);
consosle.log(object['name']);
consosle.log(object[key]);
```

### Add element
```js
object.key = value;
```

### Delete element
```js
delete object.key;
```

### Change value

```js
object.key = newValue;
```
## Iteration
### for in
```js
for (key in object) {
  console.log(key);
  console.log(object[key]);
}
```


### keys, values, entries
```js
Object.keys(object) // array keys
Object.values(object) // array values
Object.entries(object) // array of arrays => [key, value]
```

### Keys
TYPE KEYS ALWAYS === STRING or SYMBOL
```js
const object = {
  name: 'Maxim',
  10: 12345,
  undefined: undefined,
  [false]: false
}
```

## Symbol as key
```js
const id = Symbol('id');

const object = {
  [id] = 1, // Symbol(id): 1
  [id] = 2, // Symbol(id): 2
  [id] = 3, // Symbol(id): 3
}

console.log(object[id]); // 1
```

## in
```js 
console.log('key' in object); // boolean
```

## Join
### Spread operator (...)
```js
const joinObject = {...objectOne, ...objectN, key: value};
```

### Object.assign
```js
Object.assign(objectOne, objectTwo);
Object.assign({}, objectTwo);
```

## Optional chaining (?.)
```js
if (object && object.key && object.key.oneMoreKey) {
  console.log('Ok!');
} else {
  console.log('Error!');
}
```

```js
if (object?.key?.oneMoreKey) {
  console.log('Ok!');
} else {
  console.log('Error!');
}
```