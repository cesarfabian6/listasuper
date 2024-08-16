<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lista de Supermercado</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
        }
        #loginForm, #shoppingListForm {
            display: flex;
            margin-bottom: 20px;
        }
        input[type="text"], input[type="password"] {
            flex-grow: 1;
            padding: 10px;
            margin-right: 10px;
        }
        button {
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            background-color: #f9f9f9;
            margin-bottom: 10px;
            padding: 10px;
            display: flex;
            align-items: center;
        }
        li span {
            flex-grow: 1;
        }
        .delete {
            background-color: #f44336;
            margin-left: 10px;
        }
        .delete:hover {
            background-color: #da190b;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Lista de Supermercado</h1>
        <div id="loginSection">
            <form id="loginForm">
                <input type="text" id="username" placeholder="Nombre de usuario" required>
                <input type="password" id="password" placeholder="Contraseña" required>
                <button type="submit">Iniciar Sesión</button>
            </form>
        </div>
        <div id="listSection" style="display:none;">
            <p>Bienvenido, <span id="userDisplay"></span>! <button id="logout">Cerrar Sesión</button></p>
            <form id="shoppingListForm">
                <input type="text" id="newItem" placeholder="Agregar nuevo artículo" required>
                <button type="submit">Agregar</button>
            </form>
            <ul id="shoppingList"></ul>
        </div>
    </div>

    <script>
        const users = [
            { username: 'usuario1', password: 'contraseña1' },
            { username: 'usuario2', password: 'contraseña2' }
        ];
        let currentUser = null;
        let shoppingList = [];

        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            const user = users.find(u => u.username === username && u.password === password);
            if (user) {
                currentUser = user;
                document.getElementById('loginSection').style.display = 'none';
                document.getElementById('listSection').style.display = 'block';
                document.getElementById('userDisplay').textContent = username;
            } else {
                alert('Usuario o contraseña incorrectos');
            }
        });

        document.getElementById('logout').addEventListener('click', function() {
            currentUser = null;
            shoppingList = [];
            document.getElementById('loginSection').style.display = 'block';
            document.getElementById('listSection').style.display = 'none';
            document.getElementById('shoppingList').innerHTML = '';
        });

        document.getElementById('shoppingListForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const newItem = document.getElementById('newItem').value;
            if (newItem) {
                shoppingList.push({ id: Date.now(), name: newItem, completed: false });
                document.getElementById('newItem').value = '';
                renderList();
            }
        });

        function renderList() {
            const list = document.getElementById('shoppingList');
            list.innerHTML = '';
            shoppingList.forEach(item => {
                const li = document.createElement('li');
                li.innerHTML = `
                    <input type="checkbox" ${item.completed ? 'checked' : ''}>
                    <span>${item.name}</span>
                    <button class="delete">Eliminar</button>
                `;
                li.querySelector('input').addEventListener('change', function() {
                    item.completed = !item.completed;
                });
                li.querySelector('.delete').addEventListener('click', function() {
                    shoppingList = shoppingList.filter(i => i.id !== item.id);
                    renderList();
                });
                list.appendChild(li);
            });
        }
    </script>
</body>
</html>
