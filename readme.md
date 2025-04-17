# Task Manager Code Explanation

This document  provides a detailed explanation of the task manager web application. The application is a simple but complete task management system built with HTML, CSS, & JavaScript. 

## Table of Contents

1. [HTML Structure](#html-structure)
2. [CSS Structure](#html-styling)
3. [JavaScript Funtionality](#javascript-funtionality)
    - [DOM Elements](#dom-elements)
    - [Data Management](#data-management)
    - [Task Operations](#task-operations)
    - [UI Rendering](#ui-rendering)
    - [Event Handling](#event-handling)
4. [Data Management](#data-management)
5. [Event Handling](#event-handling)

## HTML Structure

The **HTML structure** defines the user interface of the Task Manager Application: 

```HTML
<body>
    <div class="container">
        <h1>Task Manager</h1>

        <div class="is">
            <input type="text" id="ti" placeholder="no">
            <button id="a-b" onclick="addNew()">add</button>
        </div>

        <ul id="t-l" class="t-l">

        </ul>

        <div id="n-t">
            No tasks yet bi-
        </div>

        <div class="s-b">
            <span id="t-c">Total: 0 tasks</span>
            <span id="cc">Completed: 0</span>
        </div>

        <button id="c-a" class="c-a" onclick="cancel()">Clear All Tasks</button>
    </div>
</body>
```
Key HTML Components:
- A container `<div>` that wraps the entire application
- A heading that displays the title
- An input section with a text field and an **ADD** button
- An empty, unordered list (`<ul>`) where tasks will be displayed
- A message that shows when there are no tasks
- A status bar showing task counts
- A **Clear All Tasks** button

## HTML Styling

The CSS Styling defines the visual appearnece of the Task Manager.

```CSS
        * {
            padding:0;
            margin:0;
            box-sizing: border-box;
            font-family: "Playwrite MX Guides", cursive;
            font-weight: 400;
            font-style: normal;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
        }
        h1 {
            text-align: center;
        }
        .is {
            display: flex;
            margin-bottom: 20px;
        }
        #ti {
            flex: 1;
            padding: 10px;
            font-size: 16px;
            flex-direction: column;
        }
        #a-b {
            padding: 10px 15px;
            color: white;
            cursor: pointer;
            background-color: green;
        }
        .c-a {
            display: block;
            margin-top: 20px;
            cursor:pointer;
            color: white;
            font-size: 16px;
            background-color: orange;
        }
        #n-t {
            text-align: center ;
            color: grey;
            padding: 20px 0px;
            font-style: italic;
        }
        .s-b{
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            padding-top:15px;
        }
        #t-l {
            list-style-type:none ;
        }
        .t-l {
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 12px;
            background-color: whitesmoke;
            margin-bottom: 8px;
        }
        .t-t {
            flex: 1;
            margin-left: 10px;
        }
        .comp {
            text-decoration: line-through;
            color: grey;
        }
        .d-b {
            color: white;
            background-color: red;
        }
```
Key CSS Features:
- **Reset styles**
- **Container styling** : Creates a centered white card with rounded corners and subtle shadow
- **Input Selection** : Uses flexbox to position the input field and button with text
- **Task Items** : Styles each task with background-color, spacing, and flexbox layout
- **Button styles** : Defines appearence for *ADD*, *DELETE*, and *CLEAR ALL* buttons
- **Status Indicator** : Styles for completed tasks (~~strikethrough~~)
- **Responsive Design** : Uses relative units and max-width to ensure responsive behavior

## JavaScript Funtionality

The JavaScript code handles all the dynamic behavior of the Task Manager Application

### DOM Elements
```JavaScript
const taskInput = document.getElementById('ti');
const addButton = document.getElementById('a-b');
const taskList = document.getElementById('t-l');
const noTasks = document.getElementById('n-t');
const clearAll = document.getElementById('c-a');
const taskCount = document.getElementById('t-c');
const completedCount = document.getElementById('cc');
let tasks = [];
```
This section selects all the necessary DOM elements that will be manipulated and intitialzes an empty tasks array.

### Data Management
```javascript
function saveTasks() {
    localStorage.setItem('tasks', JSON.stringify(tasks));
}
```
```javascript
function loadTasks() {
    const saved = localStorage.getItem('tasks');
    if (saved) {tasks = JSON.parse(saved);
    renderTasks();}
}
```
These functions handle persistant storage:
- `loadTasks()` : Retrieves tasks from localStorage when the page loads.
- `saveTasks()` : Saves the current tasks to localStorage whenever changes are made

### Task Operations

```javascript
 function addNew() {
    const tasktext = taskInput.value.trim();
    if (tasktext) {
        const newTask = {
            id: Date.now(),
            text: tasktext,
            completed: false,
            createdAt: new Date().toISOString()
        };
        tasks.push(newTask);
        saveTasks();
        renderTasks();
    };
};
```
```javascript
function deleteTask() {
    tasks = tasks.filter(function (task) {
        return task.id !== taskId 
    });
    saveTasks();
    renderTasks();
}
```
```javascript
function saveTasks() {
    localStorage.setItem('tasks', JSON.stringify(tasks));
};
```
```javascript
function toggleTaskCompletion() {
    for (let i=0;i<tasks.length;i++) {
        if (tasks[i].id === task.id) {
            tasks[i].completed =! tasks[i];
            break;
        }
    }
    saveTasks();
    renderTasks();
}
```
```javascript
function cancel() {
    if (tasks.length > 0) {
        const confirmed = confirm('Are you sure you want to continue?');
        if (confirmed) {
            tasks = [];
            saveTasks();
            renderTasks();
        }
    }
}
```
These functions implement the core task operations:
- `addTask()` : creates a new task object, adds it to the array and updates the UI
- `deleteTask()` : removes a specific task by ID using array filtering
- `toggleTaskCompletion(taskID)` : toggles the completed status of a task
- `cancel()` : removes all tasks after confirmation

### UI Rendering

```javascript 
function updateTaskCounts() {
    let totalTasks = tasks.length;
    let completedTasks = tasks.filter(function(task) {
        return task.completed;
    }).length
    taskCount.textContent = `Total: ${totalTasks} tasks`;
    completedCount.textContent = `Completed: ${completedTasks}`
}
```
```javascript
function renderTasks() {
    if (tasks.length !== 0) {
        noTasks.style.display = 'none'
    } else { noTasks.style.display = 'block'}
    tasks.forEach(function(task) {
        const li = document.createElement('li');
        li.textContent = task.text;
        li.className = 't-l';  
        const checkbox = document.createElement('input');
        checkbox.type = 'checkbox';
        checkbox.checked = task.completed;
        checkbox.addEventListener('change', function() {
            toggleTaskCompletion(task.id);
        });
        const span = document.createElement('span');
        span.className = task.completed ? 'task-text completed' : 'texk-text';
        span.textContent = task.text;

        const deletebtn  = document.createElement('button');
        deletebtn.className = 'd-b';
        deletebtn.textContent = 'delete'
        deletebtn.addEventListener('click', () => {
            deleteTask();
        });
    li.append(checkbox);
    li.append(span);
    li.append(deletebtn);
    taskList.append(li);})
}
```
These functions handle UI updates:
- `updateTaskCounts()` : calculates and displays total and completed task counts
- `renderTasks()` : renders the entire task list and updates the status bar

### Event Handling

The final section sets up event listners:
- Click handler for the **ADD** button
- Keypress handlers for the Enter key in the input field
- Click handler for the **CLEAR ALL** button
- Inital call `loadTasks()` to load tasks when the page loads

## Data Management
The application uses a simple but effective data structure:
1. **Task Object Structure** :
    ```javascript
    const newTask = {
            id: Date.now(),
            text: tasktext,
            completed: false,
            createdAt: new Date().toISOString()
        };
    ```
2. **Storage Method**
    - The application uses `localStorage` for persistant storage
    - Tasks are stored a JSON string and then parsed back to an array when needed
    - This allows tasks to persist even when the browser is closed and reopened

## Event Flow

The typical flow of the operation is:
1. User adds a task -> `addTasks()` -> `saveTasks()` -> `renderTasks()`
2. User toggles the completion -> `toggleTaskCompletion` -> `saveTasks()` -> `renderTasks()`
3. User deletes task -> `deleteTask()` -> `saveTasks()` -> `renderTasks()`
4. User clears all tasks -> `cancel()` -> `saveTasks()` -> `renderTasks()`

This pattern ensures that:
1. The data model (tasks array) is updated first
2. Changes are persistent to `localStorage`
3. The UI is updated to reflect the current state
