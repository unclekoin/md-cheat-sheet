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
const getToDo = async () => {
  let isLoading = true;

  try {
    const request = await fetch('https://jsonplaceholder.typicode.com/todos/1');
    console.log(request); // Response {type: "cors", url: "https://jsonplaceholder.typicode.com/todos/1", redirected: false, status: 200, ok: true, …}

    if (!request.ok) throw Error('Invalid request!');

    const response = await request.json();
    console.log(response); // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } catch (error) {
    console.log(error);
  } finally {
    isLoading = false;
  } 
};

getToDo();
```
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
### Example with Promise.all
```js
const loadData = async () => {
  try {
    const ids = [1, 15, 33, 45, 103];
    const url = 'https://jsonplaceholder.typicode.com/todos';
    
    const responses = await Promise.all(ids.map((id) => fetch(`${url}/${id}`)));
    const data = await Promise.all(responses.map(response => response.json()));

    console.log(data);
    return data

  } catch (error) {
    console.error(error);
  }
};

loadData();

(async () => {
  const data = await loadData();
  console.log(data)
})();
```
```js
const url = 'https://jsonplaceholder.typicode.com/todos';

const getTodosByIds = async (ids) => {
  try {
    const responses = await Promise.all(ids.map((id) => fetch(`${url}/${id}`)));
    const allTasks = await Promise.all(responses.map((response) => response.json()));

    console.log(allTasks);
  } catch (error) {
    console.log(error);
  }
};

getTodosByIds([43, 21, 55, 100, 10]);
```
### try, catch, finally
At least one catch-block, or a finally-block, must be present.
```js
try {...} catch {...}
try {...} finally {...}
try {...} catch {...} finally {...}
```