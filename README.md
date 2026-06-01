# Ex03 To-Do List using JavaScript
## Date:

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
#index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced To-Do List</title>

    <link rel="stylesheet" href="style.css">
</head>

<body>

    <div class="container">

        <div class="top-bar">
            <h1>Advanced To-Do List</h1>

            <button id="themeBtn">🌙</button>
        </div>

        <div class="input-section">

            <input 
                type="text" 
                id="taskInput" 
                placeholder="Enter task"
            >

            <input 
                type="date" 
                id="dueDate"
            >

            <select id="priority">
                <option value="High">High</option>
                <option value="Medium">Medium</option>
                <option value="Low">Low</option>
            </select>

            <button id="addBtn">Add</button>

        </div>

        <div class="filter-section">

            <button 
                class="filter-btn" 
                data-filter="all"
            >
                All
            </button>

            <button 
                class="filter-btn" 
                data-filter="completed"
            >
                Completed
            </button>

            <button 
                class="filter-btn" 
                data-filter="pending"
            >
                Pending
            </button>

        </div>

        <ul id="taskList"></ul>

    </div>

    <!-- IMPORTANT -->
    <script src="script.js"></script>

</body>
</html>
#style.css
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body{
    background: #0f172a;
    color: white;
    min-height: 100vh;

    display: flex;
    justify-content: center;
    align-items: center;

    padding: 20px;
    transition: 0.3s;
}

.container{
    background: #1e293b;
    width: 100%;
    max-width: 700px;

    padding: 25px;
    border-radius: 16px;
}

.top-bar{
    display: flex;
    justify-content: space-between;
    align-items: center;

    margin-bottom: 20px;
}

#themeBtn{
    background: #334155;
    border: none;
    color: white;

    padding: 10px;
    border-radius: 8px;
    cursor: pointer;
}

.input-section{
    display: grid;
    grid-template-columns: 2fr 1fr 1fr auto;

    gap: 10px;
    margin-bottom: 20px;
}

input,
select{
    padding: 12px;
    border: none;
    border-radius: 8px;
    outline: none;
}

#addBtn{
    padding: 12px;
    border: none;

    background: #2563eb;
    color: white;

    border-radius: 8px;
    cursor: pointer;
}

#addBtn:hover{
    background: #1d4ed8;
}

.filter-section{
    display: flex;
    gap: 10px;

    margin-bottom: 20px;
}

.filter-btn{
    padding: 10px 15px;

    border: none;
    border-radius: 8px;

    cursor: pointer;

    background: #334155;
    color: white;
}

ul{
    list-style: none;
}

li{
    background: #334155;

    padding: 15px;
    border-radius: 10px;

    margin-bottom: 12px;

    display: flex;
    justify-content: space-between;
    align-items: center;

    gap: 15px;
    flex-wrap: wrap;
}

.task-info{
    flex: 1;
}

.completed{
    text-decoration: line-through;
    opacity: 0.6;
}

.priority{
    margin-top: 5px;
    font-size: 14px;
}

.high{
    color: red;
}

.medium{
    color: orange;
}

.low{
    color: lightgreen;
}

.task-buttons{
    display: flex;
    gap: 8px;
}

.task-buttons button{
    border: none;

    padding: 8px 12px;
    border-radius: 6px;

    cursor: pointer;
}

.complete-btn{
    background: green;
    color: white;
}

.edit-btn{
    background: orange;
    color: white;
}

.delete-btn{
    background: crimson;
    color: white;
}

/* Light Mode */

.light-mode{
    background: #f1f5f9;
    color: black;
}

.light-mode .container{
    background: white;
}

.light-mode li{
    background: #e2e8f0;
}

.light-mode .filter-btn,
.light-mode #themeBtn{
    background: #cbd5e1;
    color: black;
}

/* Responsive */

@media(max-width: 700px){

    .input-section{
        grid-template-columns: 1fr;
    }

    li{
        flex-direction: column;
        align-items: flex-start;
    }

    .task-buttons{
        width: 100%;
    }

    .task-buttons button{
        flex: 1;
    }
}
#script.js
const addBtn = document.getElementById("addBtn");
const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");

const dueDate = document.getElementById("dueDate");
const priority = document.getElementById("priority");

const themeBtn = document.getElementById("themeBtn");

const filterButtons = document.querySelectorAll(".filter-btn");


// TASK ARRAY
let tasks = [];


// SAVE TASKS
function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}


// LOAD TASKS
function loadTasks() {

    const storedTasks = localStorage.getItem("tasks");

    if(storedTasks) {
        tasks = JSON.parse(storedTasks);
    }

    displayTasks();
}


// DISPLAY TASKS
function displayTasks(filter = "all") {

    taskList.innerHTML = "";

    tasks.forEach((task, index) => {

        // FILTER CONDITIONS
        if(filter === "completed" && !task.completed){
            return;
        }

        if(filter === "pending" && task.completed){
            return;
        }

        // CREATE LIST ITEM
        const li = document.createElement("li");

        // TASK INFO
        const taskInfo = document.createElement("div");
        taskInfo.classList.add("task-info");

        if(task.completed){
            taskInfo.classList.add("completed");
        }

        // PRIORITY CLASS
        let priorityClass = "";

        if(task.priority === "High"){
            priorityClass = "high";
        }
        else if(task.priority === "Medium"){
            priorityClass = "medium";
        }
        else{
            priorityClass = "low";
        }

        taskInfo.innerHTML = `
            <strong>${task.text}</strong>
            <div>Due: ${task.date || "No Date"}</div>
            <div class="priority ${priorityClass}">
                Priority: ${task.priority}
            </div>
        `;

        // BUTTON CONTAINER
        const buttonDiv = document.createElement("div");
        buttonDiv.classList.add("task-buttons");

        // DONE BUTTON
        const doneBtn = document.createElement("button");

        doneBtn.innerText = "Done";
        doneBtn.classList.add("complete-btn");

        doneBtn.addEventListener("click", function(){

            tasks[index].completed = !tasks[index].completed;

            saveTasks();
            displayTasks(filter);
        });

        // EDIT BUTTON
        const editBtn = document.createElement("button");

        editBtn.innerText = "Edit";
        editBtn.classList.add("edit-btn");

        editBtn.addEventListener("click", function(){

            const updatedTask = prompt("Edit Task", task.text);

            if(updatedTask !== null && updatedTask.trim() !== ""){

                tasks[index].text = updatedTask;

                saveTasks();
                displayTasks(filter);
            }
        });

        // DELETE BUTTON
        const deleteBtn = document.createElement("button");

        deleteBtn.innerText = "Delete";
        deleteBtn.classList.add("delete-btn");

        deleteBtn.addEventListener("click", function(){

            tasks.splice(index, 1);

            saveTasks();
            displayTasks(filter);
        });

        // APPEND BUTTONS
        buttonDiv.appendChild(doneBtn);
        buttonDiv.appendChild(editBtn);
        buttonDiv.appendChild(deleteBtn);

        // APPEND ELEMENTS
        li.appendChild(taskInfo);
        li.appendChild(buttonDiv);

        taskList.appendChild(li);
    });
}


// ADD TASK
addBtn.addEventListener("click", function(){

    const text = taskInput.value.trim();

    if(text === ""){
        alert("Please enter a task");
        return;
    }

    const task = {

        text: text,
        date: dueDate.value,
        priority: priority.value,
        completed: false
    };

    tasks.push(task);

    saveTasks();
    displayTasks();

    // CLEAR INPUTS
    taskInput.value = "";
    dueDate.value = "";
});


// FILTER BUTTONS
filterButtons.forEach(function(button){

    button.addEventListener("click", function(){

        const filter = button.dataset.filter;

        displayTasks(filter);
    });
});


// DARK / LIGHT MODE
themeBtn.addEventListener("click", function(){

    document.body.classList.toggle("light-mode");

    if(document.body.classList.contains("light-mode")){
        themeBtn.innerText = "☀️";
    }
    else{
        themeBtn.innerText = "🌙";
    }
});


// LOAD TASKS ON START
loadTasks();
## OUTPUT
<img width="1920" height="1080" alt="Screenshot (244)" src="https://github.com/user-attachments/assets/67afc47d-62c8-4731-8ff4-148888f5eda2" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
