[&#8678; get back to the start...](../README.md)
# Document Object Model (DOM)
## Getting Elements
### Deprecated Methods
```js
const tasksBlock = document.getElementById('tasks');
const navButtons = document.getElementsByClassName('main-navigation__button-item');
const buttons = document.getElementsByTagName('button');
```

### Modern Methods
```js
const tasksBlock = document.querySelector('#tasks');
const mainNavigation = document.querySelector('.main-navigation');
const firstButton = document.querySelector('button'); // first button
const navButton = document.querySelector('[data-button-id]'); // first button with data attribute
const navButton = document.querySelector('[data-button-id="3"]');

const navButtons = document.querySelectorAll('.main-navigation__button-item');

navButtons.forEach((button) => console.log(button));

const createTaskBlock = document.querySelector('.create-task-block');
const submitButton = createTaskBlock.querySelector('[type="submit"]');
```

## HTML elements Properties and Methods
### className
```js
const tasksWrapper = document.querySelector('.tasks__wrapper');
tasksWrapper.className; // tasks__wrapper
tasksWrapper.className = 'wrapper';
tasksWrapper.className; // wrapper
```

### id
```js
const tasksBlock = document.querySelector('#tasks');
tasksBlock.id; // tasks
tasksBlock.id = 'tasks-block';
tasksBlock.id; // tasks-block
```

### Inner Text, Text Content
```js
const submitButton = document.querySelector('.create-task-block__button');
submitButton.innerText;
submitButton.textContent;

submitButton.textContent = 'New text';
```

### innerHTML
```js
tasksBlock.innerHTML;
tasksBlock.innerHTML = '';
tasksBlock.innerHTML = '<p>New text</p>';
```

### children
```js
const createTaskForm = document.querySelector('.create-task-block');
createTaskForm.children; // only reading
```

### Data Attributes
```js
const navButton = document.querySelector('.main-navigation__button-item');
navButton.dataset.buttonId = '5';
navButton.dataset // DOMStringMap {buttonId: "5"}
navButton.dataset.buttonId; // 5
```

### style
```js
navButton.style;
navButton.style.fontWeight = '700';
navButton.style.boxShadow = 'inset 0 0 0 3px #ffffff';
```

## Creating and adding HTML elements to the DOM
### createElement
```js
const navButton = document.createElement('a');
navButton.className = 'main-navigation__button-item';
navButton.href = '#tasks_expired';
navButton.dataset.buttonId = '4'
navButton.textContent = 'Expired Tasks';
console.log(navButton);
```

### append, prepend
```js
const mainNavigation = document.querySelector('.main-navigation');
mainNavigation.prepend(navButton);
mainNavigation.append(navButton);
```

### nsertAdjasentElement
* `beforebegin`: Before the targetElement itself.
* `afterbegin`: Just inside the targetElement, before its first child.
* `beforeend`: Just inside the targetElement, after its last child.
* `afterend`: After the targetElement itself.
```js
mainNavigation.insertAdjacentElement('afterbegin', navButton);
```

### remove
```js
mainNavigation.remove();
```

### closest
return itself or the matching ancestor or null
```js
const itemText = document.querySelector('.task-item__text');
const taskItem = itemText.closest('.task-item')
```

### classList, Add, Remove, Toggle
```js
navButton.classList.add('main-navigation__button-item_selected');
navButton.classList.remove('main-navigation__button-item_selected');
navButton.classList.toggle('main-navigation__button-item_selected');
```

### Attributes Manipulations
```js
const taskInput = document.querySelector('.create-task-block__input');
taskInput.hasAttribute('type'); // return boolean
taskInput.getAttribute('value');
taskInput.value;
taskInput.removeAttribute('placeholder');
taskInput.setAttribute('placeholder', 'Some text');
```

## Events
### click
```js
const navButtons = document.querySelectorAll('.main-navigation__button-item');

navButtons.forEach((button) => {
  button.addEventListener('click', (event) => {
    const { target } = event;
    navButtons.forEach((button) => {
      button.classList.remove('main-navigation__button-item_selected');
    })
    target.classList.add('main-navigation__button-item_selected');
  });
});
```

### submit
The submit event fires when the user clicks a `submit button` or presses `Enter`.
```js
form.addEventListener('submit', (event) => {
  event.preventDefault();
  const { target } = event;
  const input = target.taskName; // taskName => input's name attribute value
  // <input name="taskName" class="create-task-block__input" type="text">

  if (input.value) {
    console.log(input.value);
    input.value = '';
  } else {
    alert('Enter your task...');
  }
});
```

### keydown & keyup
```js
document.addEventListener('keydown', (event) => {
  const { key } = event;
  const taskToDelete = document.querySelector(`[data-task-id="${key}"]`);
  
  if (taskToDelete) {
    const taskText = taskToDelete.querySelector('.task-item__text');
    const confirmDelet = confirm(`Delete task: "${taskText.textContent}"`);
    if (confirmDelet) taskToDelete.remove();
  }
});

document.addEventListener('keyup', (event) => {
  const { key } = event;
  console.log(key);
});
```

### mouseover, mouseout, mousemove
```js
const createTooltip = (text) => {
  const tooltip = document.createElement('span');
  tooltip.className = 'tooltip';
  tooltip.textContent = text;

  return tooltip;
}

document.addEventListener('mouseover', (event) => {
  const { target } = event;
  // const isDeleteButton = target.classList.contains('task-item__delete-button');
  const isDeleteButton = target.className.includes('task-item__delete-button');
  if (isDeleteButton) {
    const taskItemHTML = target.closest('.task-item');
    const taskText = taskItemHTML.querySelector('.task-item__text');
    const taskId = taskItemHTML?.dataset.taskId;
    if (taskId) {
      const tooltipHTML = createTooltip(`Delete task: "${taskText.textContent}"?`);
      target.append(tooltipHTML);
    }
  }
});

document.addEventListener('mouseout', (event) => {
  const { target } = event;
  const isDeleteButton = target.className.includes('task-item__delete-button');
  const tooltip = document.querySelector('.tooltip');
  if (isDeleteButton && tooltip) {
    tooltip.remove();
  }
});

document.addEventListener('mousemove', (event) => {
  console.log(event);
});
```

### contextmenu, change, input
```js
document.addEventListener('contextmenu', (event) => {
  event.preventDefault()
  console.log(event);
});

const isValidInput = (value) => {
  if (!value || value.includes('@')) {
    return false;
  }
  return true;
}

const taskBlock = document.querySelector('.create-task-block');
const input = taskBlock.querySelector('.create-task-block__input');

input.addEventListener('change', (event) => {
  const { target: { value } } = event;
  const errorMessageBlock = document.querySelector('.error-message-block');

  if (!isValidInput(value)) {
    const errorBlock = document.createElement('span');
    errorBlock.className = 'error-message-block';
    errorBlock.textContent = 'Error! Invalid data format!';
    taskBlock.append(errorBlock);
  } else if (errorMessageBlock) {
    errorMessageBlock.remove();
  }
});

input.addEventListener('input', (event) => {
  const { target: { value } } = event;
  console.log(value);
});
```

### Bubbling and Capture

[more information...](https://javascript.info/bubbling-and-capturing)

<img src="https://javascript.info/article/bubbling-and-capturing/eventflow.svg" alt="bubbling-and-capturing">

```html
<!-- HTML -->
  <form action="">Form
    <div>Div
      <p>P</p>
    </div>
  </form>
  <script src="index.js"></script>
```

```css
/* CSS  */
* {
  padding: 30px;
}

body {
  font-size: 30px;
  font-weight: 700;
  color: #2d3436;
}

form {
  background-color: #ffeaa7;
}

div {
  background-color: #fab1a0;
}

p {
  background-color: #ff7675;
}
```

```js
// Bubbling
allElemetns.forEach((element) => {
  element.addEventListener('click', (event) => {
    alert(`Bubbling: ${element.tagName}`);
  });
});

// Capture
allElemetns.forEach((element) => {
  element.addEventListener('click', (event) => {
    alert(`Capture: ${element.tagName}`);
  }, true); // addEventListener third parameter => true!
});

// stopPropagation
allElemetns.forEach((element) => {
  element.addEventListener('click', (event) => {
    if (event.currentTarget.tagName === 'FORM') event.stopPropagation();
    alert(`Bubbling: ${element.tagName}`);
  });
});
```

### Event delegation
```js
const mainNavigation = document.querySelector('.main-navigation');
const navButtons = document.querySelectorAll('.main-navigation__button-item');

mainNavigation.addEventListener('click', (event) => {
  const { target } = event;
  const navButton = target.closest('.main-navigation__button-item');

  if (navButton) {
    navButtons.forEach((button) => button.classList.remove('main-navigation__button-item_selected'));
    target.classList.add('main-navigation__button-item_selected');
  }
});
```