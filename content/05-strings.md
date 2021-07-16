[&#8678; get back to the start...](../README.md)
# Strings

## Methods
* str.length // number
* str.toUpperCase() // new string
* str.toLowerCase() // new string
* str.indexOf(substr) // first index or -1
* str.includes(substr) // boolean
* str.slice(start, [ end ]) // substring
* str.substring(start, [ end ]) // substring
* str.replace(substr, replacement) // change first finded substring
* str.replaceAll(substr, replacement) // change all
* str.repeat(number)
* str.trim()
---
* str.split(separator) // array
* arr.join(separator) // string
---

## Capitalize
```js 
str[0].toUpperCase() + str.slece(1);
str.replace(str[0], str[0].toUpperCase);
```
## Padding

```js
'123'.padStart(8, '0');     // "00000123"
'123'.padEnd(8, '0');     // "12300000"
```

```js
const cardNumber = '123456789045';
const lastFourDigits = cardNumber.slice(-4);
const maskedNumber = lastFourDigits.padStart(cardNumber.length, '*') // "********9045"
```