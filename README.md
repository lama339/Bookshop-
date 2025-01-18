<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        /* General Styles */
        body {
            font-family: "Georgia", serif;
            margin: 0;
            padding: 0;
            background-image: url('https://www.transparenttextures.com/patterns/green-leaves.png'); /* Leaves background */
            background-color: #fefcf0; /* Light beige */
            color: #4a4a4a; /* Dark gray text */
        }
        header {
            background-color: #8fbc8f; /* Soft green */
            color: white;
            padding: 20px;
            text-align: center;
        }
        nav ul {
            list-style: none;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            gap: 20px;
        }
        nav ul li a {
            color: white;
            text-decoration: none;
            font-size: 18px;
            cursor: pointer;
        }
        section {
            padding: 20px;
            text-align: center;
        }
        h2 {
            color: #4a4a4a;
        }
        footer {
            background-color: #8fbc8f;
            color: white;
            text-align: center;
            padding: 10px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        .book-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center; /* Center books */
            gap: 20px;
            margin-top: 20px;
        }
        .book {
            border: 2px solid #8fbc8f;
            border-radius: 10px;
            padding: 10px;
            width: 200px;
            background-color: white;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        button {
            background-color: #4caf50;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 5px;
            font-size: 14px;
        }
        button:hover {
            background-color: #45a049;
        }
        input, textarea {
            margin: 5px 0;
            padding: 10px;
            width: 100%;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        form {
            max-width: 400px;
            margin: 20px auto;
            padding: 20px;
            background-color: white;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <h1>Bookshop</h1>
        <nav>
            <ul>
                <li><a onclick="navigateTo('home')">Home</a></li>
                <li><a onclick="navigateTo('books')">Books</a></li>
                <li><a onclick="navigateTo('contact')">Contact</a></li>
                <li><a onclick="navigateTo('admin-login')">Admin</a></li>
            </ul>
        </nav>
    </header>

    <!-- Home Section -->
    <section id="home">
        <h2>Welcome to Our Bookshop</h2>
        <p>Discover the best books at the most affordable prices!</p>
    </section>

    <!-- Books Section -->
    <section id="books" class="hidden">
        <h2>Our Books</h2>
        <div class="book-container" id="book-list">
            <!-- Example books -->
            <div class="book">
                <h3>The Great Gatsby</h3>
                <p>Author: F. Scott Fitzgerald</p>
                <p>Price: $10.99</p>
            </div>
            <div class="book">
                <h3>1984</h3>
                <p>Author: George Orwell</p>
                <p>Price: $9.99</p>
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p>If you have any questions, feel free to email us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
    </section>

    <!-- Admin Login Section -->
    <section id="admin-login" class="hidden">
        <h2>Admin Login</h2>
        <form id="login-form">
            <input type="text" id="username" placeholder="Username" required>
            <input type="password" id="password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>
        <p id="login-error" style="color: red; display: none;">Invalid credentials!</p>
    </section>

    <!-- Admin Panel Section -->
    <section id="admin-panel" class="hidden">
        <h2>Admin Panel</h2>
        <form id="add-book-form">
            <h3>Add a New Book</h3>
            <input type="text" id="book-title" placeholder="Book Title" required>
            <input type="text" id="book-author" placeholder="Author" required>
            <input type="number" id="book-price" placeholder="Price" required>
            <button type="submit">Add Book</button>
        </form>
    </section>

    <!-- Footer -->
    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>

    <!-- JavaScript -->
    <script>
        // Navigation
        function navigateTo(sectionId) {
            document.querySelectorAll("section").forEach((section) => section.classList.add("hidden"));
            document.getElementById(sectionId).classList.remove("hidden");
        }

        // Admin Login
        const adminUsername = "admin";
        const adminPassword = "password123";

        document.getElementById("login-form").addEventListener("submit", function (e) {
            e.preventDefault();
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;

            if (username === adminUsername && password === adminPassword) {
                alert("Login successful!");
                navigateTo("admin-panel");
            } else {
                document.getElementById("login-error").style.display = "block";
            }
        });

        // Books Management
        const books = [];
        const bookList = document.getElementById("book-list");

        document.getElementById("add-book-form").addEventListener("submit", function (e) {
            e.preventDefault();
            const title = document.getElementById("book-title").value;
            const author = document.getElementById("book-author").value;
            const price = document.getElementById("book-price").value;

            books.push({ title, author, price });
            updateBooks();
        });

        function updateBooks() {
            bookList.innerHTML = "";
            books.forEach((book) => {
                const bookDiv = document.createElement("div");
                bookDiv.className = "book";
                bookDiv.innerHTML = `
                    <h3>${book.title}</h3>
                    <p>Author: ${book.author}</p>
                    <p>Price: $${book.price}</p>
                `;
                bookList.appendChild(bookDiv);
            });
        }
    </script>
</body>
</html>
