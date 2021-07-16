[&#8678; get back to the start...](../README.md)
# Type Conversion in JavaScript

JavaScript is a dynamically typed programming language.

## Types Conversion
1. to string
```js
String(10); // '10' - explicit conversion

'1' + 20 // '120' - implicit conversion
```
2. to number
```js
Number('5'); // 5

+'5' // 5
```

3. to boolean
```js
Boolean('hello') // true
Boolean(5) // true
```
Falsy: null, undefined, NaN, 0, ""   
Nullish: null & undefined