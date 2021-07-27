[&#8678; get back to the start...](../README.md)
# Asynchrony. Promise & Fetch
## Synchronous code
```js
const num = 5;
console.log('Start');
for (let i = 0; i < num; i += 1) {
  console.log('i:', i);
}
console.log('Finish');

// Start
// 0
// 1
// 2
// 3
// 4
// Finish
```
## Asynchronous code
```js
console.log('Start');

setTimeout(() => {
  console.log('SetTimeout');
}, 3000);

console.log('Finish');

// Start
// Finish
// SetTimeout
```

### Promise
A Promise is in one of these states:

* `pending`: initial state, neither fulfilled nor rejected
* `fulfilled`: meaning that the operation was completed successfully
* `rejected`: meaning that the operation failed
```js
const person = {
  name: 'Max',
  job: 'developer'
}
const promise = new Promise((resolve, reject) => {
  if (person.job) {
    setTimeout(() => {
      resolve(person.job);
    }, 1000);
  } else {
    reject('rejected');
  }
});

console.log(promise); // Promise {<pending>}

// then, catch and finally
promise
  .then((data) => console.log('data:', data)) // data: developer
  .catch((error) => console.log(error)) // if false => rejected
  .finally(() => console.log('finally'));
  // data: developer
  // finally
  // or
  // rejected
  // finally
  ```

### Fetch
```js
const url = 'https://jsonplaceholder.typicode.com/todos1';

const result = fetch(url, {
  method: 'GET', // default value {method: 'GET'}
});

console.log('result:', result); // result: Promise {<pending>}

result
  .then((response) => {
    console.log(response); // Response {type: "cors", url: "https://jsonplaceholder.typicode.com/todos", redirected: false, status: 200, ok: true, …}

    if (!response.ok) throw new Error('Request error!');

    return response.json(); // or response.text()
  })
  .then((data) => console.log(data))
  .catch((error) => console.log('error:', error));
```
#### Example
```html
<body>

  <ol id="data-container">
    <span id="loader" hidden>Loading...</span>
  </ol>

</body>
```
```js
const container = document.querySelector('#data-container');

const toggleLoader = () => {
  const loader = document.querySelector('#loader');
  const isHidden = loader.hasAttribute('hidden');

  if (isHidden) {
    loader.removeAttribute('hidden');
  } else {
    loader.setAttribute('hidden', '');
  }
}

const createToDoElement = (text) => {
  const li = document.createElement('li');
  const a = document.createElement('a');
  a.href = '#';
  a.textContent = text;
  li.append(a);

  return li;
};

const getAllTodos = () => {
  toggleLoader();

  fetch(url)
    .then((response) => {
      if (!response.ok) throw new Error('Request error!');
      return response.json();
    })
    .then((data) => {
      data.forEach((item) => {
        container.append(createToDoElement(item.title));
      });
    })
    .catch((error) => console.log('error:', error))
    .finally(() => {
      toggleLoader();
    });
};

getAllTodos();
```

### Promise.all
```js
Promise.all([
  new Promise(),
  new Promise(),
  new Promise(),
]);
// fulfillment => all resolved
// rejection => if any one reject 
```
#### Example
```html
<body>

  <ol id="data-container">
    <span id="loader" hidden>Loading...</span>
  </ol>

</body>
```
```js
const url = 'https://jsonplaceholder.typicode.com/todos';
const ids = [42, 55, 73, 105, 133];
const container = document.querySelector('#data-container');

const toggleLoader = () => {
  const loader = document.querySelector('#loader');
  const isHidden = loader.hasAttribute('hidden');

  if (isHidden) {
    loader.removeAttribute('hidden');
  } else {
    loader.setAttribute('hidden', '');
  }
};


const createToDoElement = (text) => {
  const li = document.createElement('li');
  const a = document.createElement('a');
  a.href = '#';
  a.textContent = text;
  li.append(a);

  return li;
};

const getToDoById = ((ids) => {
  toggleLoader();

  const requests = ids.map((id) => fetch(`${url}/${id}`));

  Promise.all(requests)
    .then((responses) => {
      const data = responses.map((response) => response.json());
      return Promise.all(data);
    })
    .then((data) => {
      data.forEach((item) => container.append(createToDoElement(`id: ${item.id}, ${item.title}`)));
    })
    .catch((error) => console.log(error))
    .finally(() => {
      toggleLoader();
    });   
});

getToDoById(ids);
```

### Promise.race
returns one of first fulfills or rejects promises
```js
Promise.race([
  new Promise(),
  new Promise(),
  new Promise()
]);
```
#### Example
```js
const p1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('p1');
  }, 500);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('p2');
  }, 2000);
});

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('p3');
    // reject('p3');
  }, 100);
});

Promise.race([p1, p2, p3])
  .then((data) => console.log('data:', data))
  .catch((error) => console.log('error:', error))
  .finally(() => console.log('finally'));
  // data: p3
  // finally
  // if rejected:
  // error: p3
  // finally
```