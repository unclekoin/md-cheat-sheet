[&#8678; get back to the start...](../README.md)
# Date & Time

0 - January, 11 - December  
0 - Sunday, 6 - Saturday

```js
const date = new Date();
```

```js
// year, month, date, hours, minutes, seconds, milliseconds

const date = new Date(2021, 0, 8, 16, 20, 15, 200);
// Fri Jan 08 2021 16:20:15 ...

date.getFullYear(); // 2021
date.getMonth(); // 0
date.getDate(); // 8
date.getHours(); // 16
date.getMinutes(); // 20
date.getSeconds(); // 15
date.getMilliseconds(); // 200

date.getDay(); // day of week => 5
```

## Changing parameters

```js
date.setFullYear(2022);
date.setMonth(6);
date.setDate(12);
...
date // Tue Jul 12 2022 16:20:15 ...
```

## Difference between the dates

### getTime

```js
const dateOne = new Date(2021, 0, 8);
const dateTwo = new Date (2021, 06, 12);

// milliseconds since January 1, 1970
dateOne.getTime() // 1610053200000
dateTwo.getTime() // 1626037200000

dateTwo - dateOne // 15984000000
```

## Code execution speed

```js
// new Date().getTime() === Date.now()

const startTime = Date.now();

for (let i = 0; i < 1000000; i += 1) {
  // ...
}

const endTime = Date.now();


startTime // 1626343276690
endTime // 1626343276692
endTime - startTime // 2
```
## toDateString
```js
new Date(1993, 6, 28, 14, 39, 7).toDateString(); // "Wed Jul 28 1993"