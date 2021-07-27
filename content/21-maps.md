[&#8678; get back to the start...](../README.md)
# Maps

A Map's keys can be any value (including functions, objects, or any primitive).

```js
const restaurant = new Map();

restaurant.set('name', 'Classico Italiano');
restaurant.set(1, 'Firenze, Italy');
restaurant.set(2, 'Lisbon, Portugal');

restaurant
  .set('categories', ['Italian', 'Pizzeria'])
  .set('open', 10)
  .set('close', 22)
  .set(true, 'We are open')
  .set(false, 'We are closed');

restaurant // Map(8) {"name" => "Classico Italiano", 1 => "Firenze, Italy", 2 => "Lisbon, Portugal", "categories" => Array(2), "open" => 10, "close" => 22, true => "We are open", false => "We are closed"}

restaurant.get('name'); // Classico Italiano
restaurant.get(true); // We are open
restaurant.get(1); // Firenze, Italy

const time = 20;

restaurant.get(time > restaurant.get('open') && time < restaurant.get('close')) // We are open

restaurant.has('categories') // true
restaurant.delete(2) // true
restaurant.size // 7

restaurant.clear();
restaurant // Map(0) {}
restaurant.size // 0

restaurant.set([1, 2], 'This is an array');
restaurant.get([1, 2]) // undefined

const array = [1, 2];
restaurant.set(array, 'TThis is an array');
restaurant.get(array) // This is an array
```

```js
const person = {
  name: 'Maxim',
  age: 25,
  job: 'Frontend developer'
};

Object.entries(person)
// (3) [Array(2), Array(2), Array(2)]
// 0: (2) ["name", "Maxim"]
// 1: (2) ["age", 25]
// 2: (2) ["job", "Frontend developer"]
// length: 3

const personMap = new Map(Object.entries(person));
personMap // // Map(3) {"name" => "Maxim", "age" => 25, "job" => "Frontend developer"}

for (const [key, value] of person) {
  console.log(key, value);
}

// Map to array
[...personMap] // array of arrays
[...personMap.entries()] // array of arrays
[...personMap.keys()] // array
[...personMap.value()] // array
```