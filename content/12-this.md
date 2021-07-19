[&#8678; get back to the start...](../README.md)
# This

```js
console.log(this); // Window {window: Window, self: Window, document: document, name: "", ...}
```

```js
const user = {
  name: 'Maxim',
  getName() {
    console.log(this);
    console.log(this.name);
  }
}

user.getName(); // {name: "Maxim", getName: ƒ} Maxim
```

```js
const developer = {
  name: 'Maxim',
  language: 'JavaScript',
  getInfo(age, experience) {
    console.log(this)
    console.log(this.name);
    console.log(age);
    console.log(experience);
  },
  getLanguage: () => {
    console.log(this);
    console.log(this.language);
  }
}

const info = developer.getInfo;

info(25, 3); 
// Window {window: Window, self: Window, document: document, name: "", location: Location, …}
// 
// 25
// 3

developer.getLanguage();
// Window {window: Window, self: Window, document: document, name: "", location: Location, …}
// undefined
```
## Bind
```js
info.bind(developer, 25, 3)();
// {name: "Maxim", language: "JavaScript", getInfo: ƒ, getLanguage: ƒ}
// Maxim
// 25
// 3
```
## Call
```js
info.call(developer, 25, 3);
```
## Apply
```js
info.apply(developer, [25, 3]);
```