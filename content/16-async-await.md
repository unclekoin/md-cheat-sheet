[&#8678; get back to the start...](../README.md)
# Asynchrony. Async Await
## Fetch
```js
const url = 'https://jsonplaceholder.typicode.com';
  
const getURL = (url, path = '') => {
  return path ? `${url}/${path}` : url;
}

fetch(getURL(url, 'users'))
  .then((response) => response.json())
  .then((users) => {
    console.log(users);
    const firstUserId = users[0]?.id;
    console.log(firstUserId);
    fetch(`${getURL(url, 'todos')}?userId=${firstUserId}`)
      .then((response) => response.json())
      .then(todos => console.log(todos))
      .catch((error) => console.log(error));
  })
  .catch((error) => console.log(error));
```
## Async Await
```js
const url = 'https://jsonplaceholder.typicode.com';
const getURL = (url, path = '') => (path ? `${url}/${path}` : url);

const getToDos = async () => {
  try {
    const response = await fetch(getURL(url, 'users'));
    console.log(response); // Response {type: "cors", url: "https://jsonplaceholder.typicode.com/users", redirected: false, status: 200, ok: true, …}
    if (!response.ok) throw new Error('Invalid request!');

    const users = await response.json();
    console.log(users); // (10) [{…}, {…}, {…}, ...]

    const firstUserId = users[0]?.id;

    const todosResponse = await fetch(
      `${getURL(url, 'todos')}?userId=${firstUserId}`
    );

    if (!todosResponse.ok) throw new Error('Invalid request!');

    const todos = await todosResponse.json();
    console.log(todos);
  } catch (error) {
    console.log(error);
  } finally {
    console.log('finally');
  }
};

console.log(getToDos()); // Promise {<fulfilled>:...}
```