<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task List with Interactive Pet</title>
    <style>
        /* Wine Color Theme */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #800000; /* Wine color background */
            color: #fff;
            text-align: center;
        }

        h1 {
            color: #fff;
            font-size: 2em;
            margin-bottom: 20px;
        }

        .container {
            width: 80%;
            max-width: 600px;
            margin: 0 auto;
            padding: 30px;
            background-color: #5c0e00; /* Darker shade of wine */
            border-radius: 10px;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
        }

        .button {
            padding: 12px 20px;
            background-color: #5c6bc0;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
        }

        .button:hover {
            background-color: #3f51b5;
        }

        /* Login Page */
        #loginPage {
            display: block;
        }

        /* To-Do List Page */
        #todoPage {
            display: none;
        }

        /* Interactive Pet Page */
        #petPage {
            display: none;
        }

        /* Motivational Message Page */
        #motivationalPage {
            display: none;
        }

        .task-list ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            background-color: #fff;
            padding: 15px;
            margin: 10px 0;
            border-radius: 5px;
            border: 1px solid #ddd;
            color: #333;
        }

        .pet {
            font-size: 100px;
            margin: 20px;
        }

        .motivational-message {
            color: #fff;
            font-size: 24px;
            text-align: center;
            margin-top: 20px;
            font-style: italic;
        }

        .message {
            color: #bbb;
            font-size: 18px;
            margin-top: 20px;
        }

        .sweet-message {
            color: #fff;
            font-size: 24px;
            text-align: center;
            margin-top: 20px;
            display: none;
        }

        /* Task Button Styling */
        #finishButton {
            margin-top: 20px;
            width: 100%;
        }

        /* Input Field Styling */
        input[type="text"], input[type="password"] {
            padding: 12px;
            margin: 10px 0;
            font-size: 16px;
            width: 100%;
            border-radius: 5px;
            border: 1px solid #444;
            background-color: #fff;
            color: #333;
        }

        input[type="text"]:focus, input[type="password"]:focus {
            outline: none;
            border-color: #5c6bc0;
        }

    </style>
</head>
<body>

    <!-- Login Page -->
    <div id="loginPage" class="container">
        <h1>Welcome! Please Log In</h1>
        <input type="text" id="username" placeholder="Username">
        <input type="password" id="password" placeholder="Password">
        <button class="button" onclick="login()">Login</button>
    </div>

    <!-- To-Do List Page -->
    <div id="todoPage" class="container">
        <h1>Your To-Do List</h1>

        <!-- Task List -->
        <div class="task-list">
            <ul id="taskList"></ul>
        </div>

        <!-- Add Task -->
        <div class="add-task">
            <input type="text" id="taskInput" placeholder="Enter a new task">
            <button class="button" onclick="addTask()">Add Task</button>
        </div>

        <!-- Finish Tasks Button -->
        <button class="button" id="finishButton" onclick="finishTasks()">Finish Tasks</button>

        <!-- Sweet Completion Message -->
        <div id="sweetMessage" class="sweet-message">
            Great job! You completed your tasks! 🎉
        </div>

        <!-- Interactive Pet -->
        <div id="pet" class="pet">😢</div>

        <!-- Message -->
        <div class="message" id="message">Your pet is sad. Complete a task to make it happy!</div>
    </div>

    <!-- Motivational Page -->
    <div id="motivationalPage" class="container">
        <h1>Keep Going! You're Almost There</h1>
        <div class="motivational-message">
            "Believe you can and you're halfway there."
        </div>
        <button class="button" onclick="goBackToTodoPage()">Back to To-Do List</button>
    </div>

    <script>
        let todoList = [];
        let tasksCompleted = 0;

        // Login function
        function login() {
            const username = document.getElementById('username').value.trim();
            const password = document.getElementById('password').value.trim();

            if (username === 'admin' && password === '1234') {
                document.getElementById('loginPage').style.display = 'none';  // Hide login page
                document.getElementById('todoPage').style.display = 'block';  // Show To-Do page
            } else {
                alert('Invalid username or password');
            }
        }

        // Add task function
        function addTask() {
            const taskInput = document.getElementById('taskInput');
            const taskText = taskInput.value.trim();

            if (taskText) {
                todoList.push({ text: taskText, completed: false });
                taskInput.value = '';
                displayTasks();
                updatePetMood();  // Update pet mood after task completion
            }
        }

        // Display tasks function
        function displayTasks() {
            const taskListElement = document.getElementById('taskList');
            taskListElement.innerHTML = '';

            if (todoList.length === 0) {
                taskListElement.innerHTML = '<li>No tasks yet!</li>';
                document.getElementById('finishButton').style.display = 'none'; // Hide finish button if no tasks
            } else {
                todoList.forEach((task, index) => {
                    const taskItem = document.createElement('li');
                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.checked = task.completed;
                    checkbox.addEventListener('change', () => toggleTask(index));

                    const span = document.createElement('span');
                    span.textContent = task.text;
                    if (task.completed) {
                        span.classList.add('completed');
                    }

                    taskItem.appendChild(checkbox);
                    taskItem.appendChild(span);
                    taskListElement.appendChild(taskItem);
                });
                document.getElementById('finishButton').style.display = 'block'; // Show finish button if tasks exist
            }
        }

        // Toggle task completion
        function toggleTask(index) {
            todoList[index].completed = !todoList[index].completed;
            tasksCompleted = todoList.filter(task => task.completed).length;
            displayTasks();
            updatePetMood();  // Update pet mood based on completed tasks
        }

        // Finish tasks button handler
        function finishTasks() {
            todoList.forEach(task => task.completed = true); // Mark all tasks as completed
            tasksCompleted = todoList.length;
            displayTasks();
            updatePetMood();  // Update pet mood after finishing all tasks
            document.getElementById('sweetMessage').style.display = 'block'; // Show sweet message
            setTimeout(() => {
                document.getElementById('sweetMessage').style.display = 'none'; // Hide message after 4 seconds
            }, 4000);
            // Go to motivational page after finishing tasks
            document.getElementById('todoPage').style.display = 'none';
            document.getElementById('motivationalPage').style.display = 'block';
        }

        // Update pet mood based on task completion
        function updatePetMood() {
            const pet = document.getElementById('pet');
            const message = document.getElementById('message');

            if (tasksCompleted > 0) {
                pet.innerText = "😊";  // Happy pet emoji
                message.innerText = "Great job! Your pet is happy now!";
            } else {
                pet.innerText = "😢";  // Sad pet emoji
                message.innerText = "Your pet is sad. Complete a task to make it happy!";
            }
        }

        // Go back to To-Do List page
        function goBackToTodoPage() {
            document.getElementById('motivationalPage').style.display = 'none';
            document.getElementById('todoPage').style.display = 'block';
        }

        // Initial setup
        displayTasks();
        updatePetMood();
    </script>

</body>
</html>
