[&#8678; get back to the start...](../README.md)
# Classes & Object-Oriented Programming (OOP)

## Function
```js
function Animal(name) {
  this.name = name;

  this.getName = function () {
    return this.name;
  }
}

const cat = new Animal('Cat');
console.log('cat', cat); // cat Animal {name: "Cat", getName: ƒ}
console.log(cat.name); // Cat
console.log(cat.getName()); // Cat
```

## Class
```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  getName() {
    console.log(this.name);
  }
}

const cat = new Animal('Cat');
console.log('cat', cat); // cat Animal {name: "Cat"}
console.log(cat.name); // Cat
console.log(cat.getName()); // Cat
```

## Four Principles of OOP
Encapsulation, Abstraction, Inheritance, and Polymorphism

### Inheritance

```js
class Plane {
  constructor(type, capacity) {
    this.type = type;
    this.capacity = capacity;
  }

  getFly() {
    console.log('Fly!');
  }
  
}

class MilitaryPlane extends Plane {
  constructor(type) {
    super(type, 0);
    this.guns = 0;
  }
  getFly() {
    console.log('Fly to attack!');
  }
  
  setGuns(guns) {
    this.guns = guns;
  }

  shoot() {
    console.log('Attention, I\'m shooting!')
  }
}

const plane = new Plane('passanger', 500);
console.log(plane);
plane.getFly();

const militaryPlane = new MilitaryPlane('military');
console.log(militaryPlane);
militaryPlane.getFly();
militaryPlane.shoot();

// instenceof
console.log(militaryPlane instanceof Plane); // true
console.log(militaryPlane instanceof MilitaryPlane); // true
```
### Encapsulation

```js
class Developer {
  #salary; // initialization

  constructor(name, language) {
    this.name = name;
    this.language = language;
    this.#salary = 300; // private field
  }

  // getter
  get getSalary() {
    return this.#salary;
  }

  // setter can't accept more than one parameter
  set setSalary(value) {
    this.#salary = value;
  }

  startCoding() {
    console.log(`${this.name} starts coding.`)
  }

  printLanguage() {
    console.log(`Programing language: ${this.language}`);
  }

  printSalary() {
    console.log(this.#salary);
  }

  #getInfo() {
    console.log('Information');
  }
}

class JuniorDevelopers extends Developer {
  constructor(name, language) {
    super(name, language);
  }
}

const developer = new Developer('Max', 'JavaScript');
const juniorDeveloper = new JuniorDevelopers('John', 'JavaScript');

console.log(developer);

// public fields and methods
console.log(developer.name);
console.log(developer.language);
developer.startCoding();
juniorDeveloper.startCoding();

// private fields and methods
console.log(developer.#salary);
console.log(developer.setSalary = 500);
console.log(developer.getSalary); 
// Uncaught SyntaxError: Private field '#salary' must be declared in an enclosing class
developer.printSalary();
developer.#getInfo();
// Uncaught SyntaxError: Private field '#getInfo' must be declared in an enclosing class
```

### Polymorphism
One interface, many implementations
```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  makeSound() {} // one interface
}

class Dog extends Animal {
  constructor(name) {
    super(name);
  }

  makeSound() {
    console.log('woof-woof'); // many implementations
  }
}


class Cat extends Animal {
  constructor(name) {
    super(name);
  }

  makeSound() {
    console.log('meow-meow'); // many implementations
  }
}
```

### Abstraction
Set of the most significant characteristics of the object
```js
class Footballer {
  constructor(name, club) {
    this.name = name;
    this.club = club;
  }

  shoot() {}
  celebrate() { }
  pass() {}
}

class Forward extends Footballer {
  constructor(name, club) {
    super(name, club);
  }

  shoot() {
    // ...
  }
  celebrate() {
    // ...
  }
  pass() {
    // ...
  }
}
```

### Static methods and properties
- don't use "this"
- fields and methods do not belong to a specific object
```js
class Car {
  static isCar(car) {
    return car instanceof Car;
  }

  // static initialParams = {
  //   name: 'Ford',
  //   speed: 200,
  // };

  static #initialParams = {
    name: 'Ford',
    speed: 200,
  };

  constructor(name, speed) {
    this.name = name || Car.#initialParams.name;
    this.speed = speed || Car.#initialParams.speed;
  }

  drive() {
    console.log(`${this.name} drives...`);
  }
}

const car = new Car('BMW', 300);

console.log(Car.isCar(car)); // true

const defaultCar = new Car();
console.log(defaultCar); // Car {name: "Ford", speed: 200}
```