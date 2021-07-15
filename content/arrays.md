[&#8678; get back to the start...](./README.md)
# Arrays

### Add elements
returt new length of array
```js
array.push(el, [el]);
array.unshift(el, [el]);
```

### Remove elements
return removed element
```js
array.pop();
array.shift();
```

### Change element
```js
array[index] = value;
```

## Iterate of array

### for
```js
for (let i = 0; i < array.lenght; i += 1) {
  console.log(array[i]);
}
```

### for of
```js
for (const itme of array) {
  console.log(item);
}
```

### forEach
```js
array.forEach((item, [index, array]) => {
  console.log(item, index, array);
});
```

## Methods of arrays
### map
return array changed elemets
```js
array.map((item, [index, array]) => {
  return item * 2;
});
```

### filter
return new filetred array
```js
array.fileter((item, [index, array]) => {
  return item === property; // boolean
});
```

### find
return first founded element
```js
array.find((item, [index, array]) => {
  return item > property // boolean
});
```

### findIndex
return first index founded element or -1
```js
array.findIndex((item, [index, array]) => {
  return item === property // boolean
});
```

### some 
return boolean (at least one element => true)
```js
array.some((item, [index, array]) => {
  return item === property; // boolean
});
```

### evry 
return boolean (all elements => true)
```js
array.evry((item, [index, array]) => {
  return item === property; // boolean
});
```

### reduce
return accumulator
```js
array.reduce((acc, item, [index, array]) => {
  return acc + item;
}, initialValue);
```

### sort
!!! MODIFIES ARRAY !!!  
sort numbers
```js
array.sort((a, b) => a - b);
```

sort strings
```js
// a => z
array.sort();
```
```js
// z => a
array.sort((a, b) => {
  if (a < b) return 1;
  if (a > b) return -1;
  return 0;
});
```
### splice
!!! MODIFIES ARRAY !!!  
remove or replace element(s) return removed element(s)
```js
array.splice(start,[end, replacement]);
```

### slice
return new array
```js
array.slice(start, [end]);
```

### indexOf
return index or -1
```js
array.indexOf(element);
```

### includes
return boolean
```js
array.includes(element)
```

### split & join
```js
string.split(separator); // return array
array.join(separator); // return string
```

### reverse
```js
array.reverse(); // [a, b, c] => [c, b, a]
```

## Arrays concatenation

### concat
return new array
```js
arrayOne.concat(arrayTwo, arrayN, elementOne, elementN);
```

### spread operator (...)
```js
[...arrayOne, ...arrayTwo, elemet]
```
