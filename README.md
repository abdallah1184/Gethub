<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo List App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }

        .todo-container {
            max-width: 600px;
            margin: 20px auto;
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
        }

        form {
            display: flex;
            margin-bottom: 20px;
        }

        input[type="text"] {
            flex: 1;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            background-color: #007bff;
            color: #fff;
            cursor: pointer;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            border-bottom: 1px solid #eee;
        }

        .complete {
            text-decoration: line-through;
            opacity: 0.6;
        }

        .actions {
            display: flex;
        }

        .actions button {
            margin-left: 5px;
            padding: 5px;
            background-color: #dc3545;
            color: #fff;
        }
    </style>
</head>
<body>
    <div class="todo-container">
        <h1>Todo List</h1>
        <form id="todo-form">
            <input type="text" id="todo-input" placeholder="Add new todo..." required>
            <button type="submit">Add</button>
        </form>
        <ul id="todo-list"></ul>

        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const todoForm = document.getElementById('todo-form');
                const todoInput = document.getElementById('todo-input');
                const todoList = document.getElementById('todo-list');

                // Load todos from localStorage on page load
                let todos = JSON.parse(localStorage.getItem('todos')) || [];

                // Function to render todos
                function renderTodos() {
                    todoList.innerHTML = '';
                    todos.forEach((todo, index) => {
                        const li = document.createElement('li');
                        li.innerHTML = `
                            <span class="${todo.completed ? 'complete' : ''}">${todo.text}</span>
                            <div class="actions">
                                <button onclick="toggleComplete(${index})">${todo.completed ? 'Undo' : 'Complete'}</button>
                                <button onclick="deleteTodo(${index})">Delete</button>
                            </div>
                        `;
                        todoList.appendChild(li);
                    });
                }

                renderTodos();

                // Function to add new todo
                todoForm.addEventListener('submit', function(event) {
                    event.preventDefault();
                    const todoText = todoInput.value.trim();
                    if (todoText) {
                        const newTodo = {
                            text: todoText,
                            completed: false,
                            timestamp: new Date().getTime()
                        };
                        todos.unshift(newTodo); // Add to beginning of array
                        localStorage.setItem('todos', JSON.stringify(todos));
                        todoInput.value = ''; // Clear input
                        renderTodos();
                    }
                });

                // Function to toggle todo completion
                window.toggleComplete = function(index) {
                    todos[index].completed = !todos[index].completed;
                    localStorage.setItem('todos', JSON.stringify(todos));
                    renderTodos();
                };

                // Function to delete todo
                window.deleteTodo = function(index) {
                    todos.splice(index, 1);
                    localStorage.setItem('todos', JSON.stringify(todos));
                    renderTodos();
                };
            });
        </script>
    </div>
</body>
</html>
