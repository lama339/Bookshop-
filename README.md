<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: url('https://www.toptal.com/designers/subtlepatterns/patterns/leafy.png');
            background-size: cover;
            color: black;
        }

        header {
            background-color: darkgreen;
            color: white;
            padding: 10px 0;
            text-align: center;
            font-size: 24px;
        }

        nav {
            background-color: darkgreen;
            padding: 10px;
            display: flex;
            justify-content: center;
        }

        nav a {
            color: white;
            padding: 10px;
            text-decoration: none;
            margin: 0 15px;
        }

        nav a:hover {
            background-color: #333;
        }

        .container {
            width: 80%;
            margin: 20px auto;
        }

        .section {
            padding: 20px;
            margin: 20px 0;
            background-color: rgba(255, 255, 255, 0.8);
        }

        .book-list {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }

        .book-item {
            border: 1px solid #ddd;
            padding: 10px;
            margin: 10px;
            width: 200px;
            text-align: center;
            background-color: white;
        }

        .book-item img {
            width: 100%;
            height: auto;
        }

        .admin-panel {
            display: none;
            background-color: white;
            padding: 20px;
            border: 1px solid #ddd;
        }

        .admin-panel input, .admin-panel textarea {
            width: 100%;
            padding: 8px;
            margin: 10px 0;
            border: 1px solid #ddd;
        }

        .admin-panel button {
            background-color: darkgreen;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
        }

        .admin-panel button:hover {
            background-color: green;
        }

        .login-form {
            width: 300px;
            margin: 100px auto;
            padding: 20px;
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 8px;
        }

        .login-form input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
        }

        .login-form button {
            background-color: darkgreen;
            color: white;
            border: none;
            padding: 10px;
            width: 100%;
        }

    </style>
</head>
<body>

<header>
    Bookshop
</header>

<nav>
    <a href="#" onclick="showSection('home')">Home</a>
    <a href="#" onclick="showSection('books')">Books</a>
    <a href="#" onclick="showSection('contact')">Contact</a>
    <a href="#" onclick="showSection('admin')">Admin</a>
</nav>

<div class="container">
    <div id="home" class="section">
        <h2>Welcome to Our Bookshop!</h2>
        <p id="home-text">Explore our collection of books available for you!</p>
        <img id="home-image" src="https://via.placeholder.com/400" alt="Bookshop Image">
    </div>

    <div id="books" class="section">
        <h2>Our Books</h2>
        <div id="book-list" class="book-list">
            <!-- Dynamic Book List -->
        </div>
    </div>

    <div id="contact" class="section">
        <h2>Contact Us</h2>
        <p>Email: contact@bookshop.com</p>
        <p>Phone: +123 456 7890</p>
    </div>

    <div id="admin" class="section">
        <h2>Admin Login</h2>
        <div id="login-form" class="login-form">
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <button onclick="login()">Login</button>
        </div>
        <div id="admin-panel" class="admin-panel">
            <h3>Edit Home Page</h3>
            <textarea id="home-text-edit" rows="4" placeholder="Edit home page text"></textarea>
            <input type="file" id="home-image-edit" accept="image/*">
            <br><br>
            <h3>Manage Books</h3>
            <input type="text" id="book-title" placeholder="Book Title">
            <input type="file" id="book-image" accept="image/*">
            <input type="number" id="book-price" placeholder="Book Price">
            <button onclick="addBook()">Add Book</button>
            <br><br>
            <button onclick="saveChanges()">Save Changes</button>
        </div>
    </div>
</div>

<script>
    const adminUsername = 'admin';
    const adminPassword = 'password';

    const books = [
        { title: 'Book 1', image: 'https://via.placeholder.com/150', price: 15 },
        { title: 'Book 2', image: 'https://via.placeholder.com/150', price: 20 },
        { title: 'Book 3', image: 'https://via.placeholder.com/150', price: 25 }
    ];

    const savedHomeText = localStorage.getItem('homeText') || "Explore our collection of books available for you!";
    const savedHomeImage = localStorage.getItem('homeImage') || "https://via.placeholder.com/400";

    // Render books and home page content
    function renderBooks() {
        const bookListElement = document.getElementById('book-list');
        bookListElement.innerHTML = '';
        books.forEach(book => {
            const bookItem = document.createElement('div');
            bookItem.classList.add('book-item');
            bookItem.innerHTML = `
                <img src="${book.image}" alt="${book.title}">
                <h3>${book.title}</h3>
                <p>$${book.price}</p>
            `;
            bookListElement.appendChild(bookItem);
        });
    }

    function showSection(section) {
        const sections = ['home', 'books', 'contact', 'admin'];
        sections.forEach(s => {
            document.getElementById(s).style.display = (s === section) ? 'block' : 'none';
        });

        if (section === 'home') {
            document.getElementById('home-text').innerText = savedHomeText;
            document.getElementById('home-image').src = savedHomeImage;
        }

        if (section === 'admin') {
            document.getElementById('login-form').style.display = 'block';
            document.getElementById('admin-panel').style.display = 'none';
        }
    }

    function login() {
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;

        if (username === adminUsername && password === adminPassword) {
            document.getElementById('login-form').style.display = 'none';
            document.getElementById('admin-panel').style.display = 'block';
        } else {
            alert('Invalid credentials');
        }
    }

    function saveChanges() {
        const newHomeText = document.getElementById('home-text-edit').value;
        const homeImageFile = document.getElementById('home-image-edit').files[0];

        if (newHomeText) {
            localStorage.setItem('homeText', newHomeText);
        }

        if (homeImageFile) {
            const reader = new FileReader();
            reader.onload = function (e) {
                localStorage.setItem('homeImage', e.target.result);
            };
            reader.readAsDataURL(homeImageFile);
        }

        alert('Changes saved!');
        showSection('home');
    }

    function addBook() {
        const title = document.getElementById('book-title').value;
        const price = document.getElementById('book-price').value;
        const imageFile = document.getElementById('book-image').files[0];

        if (title && price && imageFile) {
            const reader = new FileReader();
            reader.onload = function (e) {
                books.push({
                    title: title,
                    image: e.target.result,
                    price: parseFloat(price)
                });
                renderBooks();
                alert('Book added!');
            };
            reader.readAsDataURL(imageFile);
        } else {
            alert('Please fill in all fields');
        }
    }

    // Initial render
    renderBooks();
    showSection('home');
</script>

</body>
</html>
