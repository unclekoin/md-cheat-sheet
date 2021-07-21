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

<svg xmlns="http://www.w3.org/2000/svg" width="641" height="633" viewBox="0 0 641 633"><defs><style>@import url(https://fonts.googleapis.com/css?family=Open+Sans:bold,italic,bolditalic%7CPT+Mono);@font-face{font-family:'PT Mono';font-weight:700;font-style:normal;src:local('PT MonoBold'),url(/font/PTMonoBold.woff2) format('woff2'),url(/font/PTMonoBold.woff) format('woff'),url(/font/PTMonoBold.ttf) format('truetype')}</style></defs><g id="dom" fill="none" fill-rule="evenodd" stroke="none" stroke-width="1"><g id="eventflow.svg"><g id="eventflow" transform="translate(22 28)"><g id="nodes"><g id="Window-node" transform="translate(224.685)"><g id="Group"><path id="Rectangle-path" fill="#DCDCDC" stroke="#000" d="M0 0h134.427v34.567H0z"/><text id="Window" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="39.368" y="21.913">Window</tspan></text></g></g><g id="document-node" transform="translate(234.287 60.493)"><g id="Group"><path id="Rectangle-path" fill="#DCDCDC" stroke="#000" d="M0 0h115.223v34.567H0z"/><text id="Document" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="22.084" y="21.913">Document</tspan></text></g></g><g id="html-node" transform="translate(243.889 120.985)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;html&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="23.045" y="21.913">&lt;html&gt;</tspan></text></g><g id="body-node" transform="translate(243.889 181.478)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;body&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="23.045" y="21.913">&lt;body&gt;</tspan></text></g><g id="table-node" transform="translate(243.889 241.97)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;table&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="23.045" y="21.913">&lt;table&gt;</tspan></text></g><g id="tbody-node" transform="translate(243.889 302.463)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;tbody&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="19.204" y="21.913">&lt;tbody&gt;</tspan></text></g><g id="tr_1-node" transform="translate(80.656 380.239)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;tr&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="35.527" y="21.913">&lt;tr&gt;</tspan></text></g><g id="tr_2-node" transform="translate(426.325 380.239)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;tr&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="33.607" y="21.913">&lt;tr&gt;</tspan></text></g><g id="tr_1_td_1-node" transform="translate(13.443 458.015)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;td&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="31.686" y="21.913">&lt;td&gt;</tspan></text></g><g id="tr_1_td_1_text-node" transform="translate(0 518.507)"><ellipse id="Oval" cx="61.452" cy="30.246" fill="#4682B4" stroke="#000" rx="61.452" ry="30.246"/><text id="Shady-Grove" fill="#FFF" font-family="OpenSans-Regular, Open Sans" font-size="12.963" font-weight="normal"><tspan x="19.204" y="34.604">Shady Grove</tspan></text></g><g id="tr_1_td_2-node" transform="translate(147.87 458.015)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;td&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="33.607" y="21.913">&lt;td&gt;</tspan></text></g><g id="tr_1_td_2_text-node" transform="translate(134.427 518.507)"><ellipse id="Oval" cx="61.452" cy="30.246" fill="#4682B4" stroke="#000" rx="61.452" ry="30.246"/><text id="Aeolian" fill="#FFF" font-family="OpenSans-Regular, Open Sans" font-size="12.963" font-weight="normal"><tspan x="36.967" y="34.604">Aeolian</tspan></text></g><g id="tr_2_td_1-node" transform="translate(359.111 458.015)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#00F" stroke="#000" rx="5"/><text id="&lt;td&gt;" fill="#FFF" font-family="PTMono-Regular, PT Mono" font-size="15.555" font-weight="normal"><tspan x="27.846" y="22.37">&lt;td&gt;</tspan></text></g><g id="tr_2_td_1_text-node" transform="translate(345.669 518.507)"><ellipse id="Oval" cx="61.452" cy="30.246" fill="#4682B4" stroke="#000" rx="61.452" ry="30.246"/><g id="Group" fill="#FFF" font-family="OpenSans-Regular, Open Sans" font-size="12.963" font-weight="normal" transform="translate(12.482 14.555)"><text id="Over-the-River,"><tspan x=".48" y="14">Over the River,</tspan></text><text id="Charlie"><tspan x="25.925" y="31.284">Charlie</tspan></text></g></g><g id="tr_2_td_2-node" transform="translate(493.538 458.015)"><rect id="Rectangle-path" width="96.019" height="34.567" x="0" y="0" fill="#87CEFA" stroke="#000" rx="5"/><text id="&lt;td&gt;" fill="#000" font-family="OpenSans-Regular, Open Sans" font-size="15.555" font-weight="normal"><tspan x="33.607" y="21.913">&lt;td&gt;</tspan></text></g><g id="tr_2_td_2_text-node" transform="translate(480.096 518.507)"><ellipse id="Oval" cx="61.452" cy="30.246" fill="#4682B4" stroke="#000" rx="61.452" ry="30.246"/><text id="Dorian" fill="#FFF" font-family="OpenSans-Regular, Open Sans" font-size="12.963" font-weight="normal"><tspan x="39.848" y="34.604">Dorian</tspan></text></g></g><g id="edges" stroke="#000" stroke-width="2" transform="translate(61.452 34.567)"><path id="window-document" fill="#000" d="M230.446 0v19.876"/><path id="document-html" fill="#000" d="M230.446 60.493v19.876"/><path id="html-body" fill="#000" d="M230.446 120.985v19.876"/><path id="body-table" fill="#000" d="M230.446 181.478v19.876"/><path id="table-tbody" fill="#000" d="M230.446 241.97v19.876"/><path id="tbody-tr_1" d="M230.446 302.463c0 11.522-16.003 17.283-48.01 17.283H86.417c-12.802 0-19.204 6.626-19.204 19.876"/><path id="tbody-tr_2" d="M230.446 302.463c0 11.522 22.404 17.283 67.213 17.283h96.02c12.802 0 19.203 6.626 19.203 19.876"/><path id="tr_1-tr_2_td_1" d="M67.213 380.239c0 11.522-6.4 17.283-19.203 17.283H19.204C6.4 397.522 0 404.148 0 417.4"/><path id="tr_1-tr_2_td_2" d="M67.213 380.239c0 11.522 6.402 17.283 19.204 17.283h28.806c12.802 0 19.204 6.626 19.204 19.877"/><path id="tr_2-tr_2_td_1" d="M412.882 380.239c0 11.522-6.401 17.283-19.204 17.283h-28.805c-12.803 0-19.204 6.626-19.204 19.877"/><path id="tr_2-tr_2_td_2" d="M412.882 380.239c0 11.522 12.803 17.283 38.408 17.283h9.602c12.802 0 19.204 6.626 19.204 19.877"/><path id="tr_1_td_1-text" fill="#000" d="M0 458.015v19.876"/><path id="tr_1_td_2-text" fill="#000" d="M134.427 458.015v19.876"/><path id="tr_2_td_1-text" fill="#000" d="M345.669 458.015v19.876"/><path id="tr_2_td_2-text" fill="#000" d="M482.016 458.015v19.876"/></g><g id="event-flow" transform="translate(103.7 14.691)"><g id="target_phase" transform="translate(186.277 443.324)"><text id="Target" fill="#00F" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x=".96" y="34.148">Target</tspan></text><text id="Phase" fill="#00F" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x=".48" y="51.431">Phase</tspan></text><text id="(2)" fill="#00F" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="15.843" y="68.715">(2)</tspan></text><rect id="Rectangle-path" width="96.019" height="34.567" x="69.134" y="0" stroke="#000" stroke-width="5" rx="5"/></g><g id="capture_phase"><text id="Capture" fill="red" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="0" y="166.367">Capture</tspan></text><text id="Phase" fill="red" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="7.201" y="183.651">Phase</tspan></text><text id="(1)" fill="red" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="22.564" y="200.934">(1)</tspan></text><path id="capture_phase_arrow" stroke="red" stroke-width="3" d="M116.183.864c-38.408-2.592-38.408 40.617 2.88 49.258"/><g id="capture_phase_arrow-link" stroke="red" stroke-width="3" transform="translate(92.178 60.493)"><path id="capture_phase_arrow" d="M28.806.864c-38.408-2.592-38.408 40.617 2.88 49.258"/></g><g id="capture_phase_arrow-link" stroke="red" stroke-width="3" transform="translate(96.98 120.985)"><path id="capture_phase_arrow" d="M28.806.864c-38.408-2.592-38.408 40.617 2.88 49.258"/></g><g id="capture_phase_arrow-link" stroke="red" stroke-width="3" transform="translate(96.98 181.478)"><path id="capture_phase_arrow" d="M28.806.864c-38.408-2.592-38.408 40.617 2.88 49.258"/></g><g id="capture_phase_arrow-link" stroke="red" stroke-width="3" transform="translate(96.98 241.97)"><path id="capture_phase_arrow" d="M28.806.864c-38.408-2.592-38.408 40.617 2.88 49.258"/></g><path id="capture_phase_arrow2" stroke="red" stroke-width="3" d="M125.785 303.327c-38.408-2.593-38.408 82.097 175.715 69.134"/><path id="capture_phase_arrow3" stroke="red" stroke-width="3" d="M301.5 385.424c-94.099-2.593-94.099 51.85-62.412 64.813"/></g><g id="bubble_phase" transform="translate(250.61 12.963)"><text id="Bubbling" fill="green" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="72.975" y="239.822">Bubbling</tspan></text><text id="Phase" fill="green" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="83.057" y="257.106">Phase</tspan></text><text id="(3)" fill="green" font-family="OpenSans-Regular, Open Sans" font-size="17.284" font-weight="normal"><tspan x="98.42" y="274.39">(3)</tspan></text><path id="bubble_phase_arrow3" stroke="green" stroke-width="3" d="M112.342 437.275c132.507-56.172 132.507-67.406 67.214-64.814"/><path id="bubble_phase_arrow2" stroke="green" stroke-width="3" d="M182.436 358.634c38.408-8.641 38.408-50.986-180.516-59.628"/><path id="bubble_phase_arrow" stroke="green" stroke-width="3" d="M0 287.772c38.408-2.593 38.408-37.16 0-45.802"/><g id="bubble_phase_arrow-link" stroke="green" stroke-width="3" transform="translate(0 181.478)"><path id="bubble_phase_arrow" d="M0 45.801C38.408 43.21 38.408 8.641 0 0"/></g><g id="bubble_phase_arrow-link" stroke="green" stroke-width="3" transform="translate(0 120.985)"><path id="bubble_phase_arrow" d="M0 45.801C38.408 43.21 38.408 8.641 0 0"/></g><path id="bubble_phase_arrow4" stroke="green" stroke-width="3" d="M0 106.294c48.01-2.593 48.01-37.16 9.602-45.801"/><path id="bubble_phase_arrow" stroke="green" stroke-width="3" d="M9.602 45.801C57.612 43.21 57.612 8.641 19.204 0"/></g></g></g></g></g></svg>

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