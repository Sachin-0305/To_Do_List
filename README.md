# Ex03 To-Do List using JavaScript
## Date:25/08/2025

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Advanced To-Do App</title>
  <style>
    :root {
      --bg: #f4f6f8;
      --card: #fff;
      --text: #222;
      --primary: #007bff;
      --secondary: #6c757d;
      --danger: #dc3545;
      --success: #28a745;
      --warning: #ffc107;
    }
    body.dark {
      --bg: #1e1e1e;
      --card: #2a2a2a;
      --text: #f1f1f1;
    }
    body {
      font-family: Arial, sans-serif;
      background: var(--bg);
      color: var(--text);
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
      padding: 40px;
      transition: background 0.3s, color 0.3s;
    }
    .todo-container {
      background: var(--card);
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.2);
      width: 400px;
      transition: background 0.3s;
    }
    h2 {
      text-align: center;
      margin-bottom: 20px;
    }
    .top-bar {
      display: flex;
      justify-content: space-between;
      margin-bottom: 15px;
    }
    .top-bar button {
      border: none;
      padding: 8px 12px;
      border-radius: 8px;
      cursor: pointer;
      background: var(--secondary);
      color: #fff;
    }
    .top-bar .clear {
      background: var(--danger);
    }
    .top-bar .theme-toggle {
      background: var(--primary);
    }
    .input-box {
      display: flex;
      margin-bottom: 15px;
    }
    input[type="text"] {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 8px 0 0 8px;
      outline: none;
    }
    .add-btn {
      border: none;
      background: var(--primary);
      color: #fff;
      border-radius: 0 8px 8px 0;
      padding: 10px 15px;
      cursor: pointer;
    }
    .search-box {
      margin-bottom: 15px;
    }
    .search-box input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    ul {
      list-style: none;
      padding: 0;
      max-height: 300px;
      overflow-y: auto;
    }
    li {
      background: #f9f9f9;
      margin-bottom: 10px;
      padding: 10px;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: 0.3s;
    }
    body.dark li {
      background: #3a3a3a;
    }
    li.completed span {
      text-decoration: line-through;
      color: gray;
    }
    .actions button {
      margin-left: 5px;
      border: none;
      padding: 5px 8px;
      border-radius: 6px;
      cursor: pointer;
      color: #fff;
    }
    .done { background: var(--success); }
    .edit { background: var(--warning); color: #000; }
    .delete { background: var(--danger); }
    .counter {
      text-align: center;
      margin-top: 15px;
      font-size: 14px;
    }
  </style>
</head>
<body>
  <div class="todo-container">
    <h2>✨To-Do</h2>
    <div class="top-bar">
      <button class="clear" onclick="clearCompleted()">Clear Completed</button>
      <button class="theme-toggle" onclick="toggleTheme()">🌙</button>
    </div>
    <div class="input-box">
      <input type="text" id="taskInput" placeholder="Enter a new task">
      <button class="add-btn" onclick="addTask()">Add</button>
    </div>
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="Search tasks..." onkeyup="filterTasks()">
    </div>
    <ul id="taskList"></ul>
    <div class="counter" id="counter"></div>
  </div>

  <script>
    let taskList = document.getElementById("taskList");
    let counter = document.getElementById("counter");

    // Load tasks
    window.onload = function() {
      let savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
      savedTasks.forEach(task => addTaskToUI(task.text, task.completed));
      updateCounter();
      if(localStorage.getItem("theme") === "dark") {
        document.body.classList.add("dark");
      }
    };

    function addTask() {
      let input = document.getElementById("taskInput");
      let taskText = input.value.trim();
      if (taskText === "") return alert("Please enter a task!");
      addTaskToUI(taskText, false);
      saveTasks();
      input.value = "";
    }

    function addTaskToUI(taskText, completed) {
      let li = document.createElement("li");
      if (completed) li.classList.add("completed");

      let span = document.createElement("span");
      span.textContent = taskText;

      let actions = document.createElement("div");
      actions.classList.add("actions");

      let doneBtn = document.createElement("button");
      doneBtn.textContent = "✔";
      doneBtn.classList.add("done");
      doneBtn.onclick = () => {
        li.classList.toggle("completed");
        saveTasks();
        updateCounter();
      };

      let editBtn = document.createElement("button");
      editBtn.textContent = "✎";
      editBtn.classList.add("edit");
      editBtn.onclick = () => {
        let newTask = prompt("Edit task:", span.textContent);
        if (newTask) {
          span.textContent = newTask;
          saveTasks();
        }
      };

      let deleteBtn = document.createElement("button");
      deleteBtn.textContent = "🗑";
      deleteBtn.classList.add("delete");
      deleteBtn.onclick = () => {
        li.remove();
        saveTasks();
        updateCounter();
      };

      actions.appendChild(doneBtn);
      actions.appendChild(editBtn);
      actions.appendChild(deleteBtn);

      li.appendChild(span);
      li.appendChild(actions);
      taskList.appendChild(li);
      updateCounter();
    }

    function saveTasks() {
      let tasks = [];
      document.querySelectorAll("#taskList li").forEach(li => {
        tasks.push({
          text: li.querySelector("span").textContent,
          completed: li.classList.contains("completed")
        });
      });
      localStorage.setItem("tasks", JSON.stringify(tasks));
    }

    function clearCompleted() {
      document.querySelectorAll("#taskList li.completed").forEach(li => li.remove());
      saveTasks();
      updateCounter();
    }

    function filterTasks() {
      let search = document.getElementById("searchInput").value.toLowerCase();
      document.querySelectorAll("#taskList li").forEach(li => {
        let text = li.querySelector("span").textContent.toLowerCase();
        li.style.display = text.includes(search) ? "flex" : "none";
      });
    }

    function updateCounter() {
      let total = document.querySelectorAll("#taskList li").length;
      let completed = document.querySelectorAll("#taskList li.completed").length;
      let pending = total - completed;
      counter.textContent = `Total: ${total} | ✅ Completed: ${completed} | ⏳ Pending: ${pending}`;
    }

    function toggleTheme() {
      document.body.classList.toggle("dark");
      localStorage.setItem("theme", document.body.classList.contains("dark") ? "dark" : "light");
    }
  </script>
</body>
</html>

```

## OUTPUT
<img width="778" height="861" alt="image" src="https://github.com/user-attachments/assets/e4e690d9-c456-420d-a5ba-6f05d50dabe4" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
