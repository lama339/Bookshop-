<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        /* General Styles */
        body {
            font-family: 'Georgia', serif;
            margin: 0;
            padding: 0;
            background-image: url('leaves-background-with-metallic-foil_79603-956.jpg'); /* Set the uploaded image as background */
            background-size: cover;
            background-repeat: no-repeat;
            background-attachment: fixed;
            color: #2C5F2D; /* Dark green for text */
        }
        header {
            background-color: rgba(255, 255, 255, 0.9); /* Semi-transparent white */
            color: #2C5F2D;
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
            color: #2C5F2D;
            text-decoration: none;
            font-size: 18px;
            cursor: pointer;
        }
        section {
            padding: 20px;
            background-color: rgba(255, 255, 255, 0.9); /* Semi-transparent white */
            margin: 20px auto;
            width: 90%;
            max-width: 1200px;
            border-radius: 10px;
        }
        .hidden {
            display: none;
        }
        h2 {
            color: #2C5F2D;
            font-family: 'Georgia', serif;
        }
        footer {
            background-color: rgba(255, 255, 255, 0.9);
            color: #2C5F2D;
            text-align: center;
            padding: 10px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        .book-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
        }
        .book {
            border: 1px solid #EAEAEA;
            padding: 20px;
            width: 250px;
            background-color: white;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            text-align: center;
            border-radius: 10px;
        }
        .book img {
            width: 100px;
            height: 150px;
            margin-bottom: 10px;
        }
        button {
            background-color: #2C5F2D;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            opacity: 0.9;
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
        <h1>Welcome to Our Bookshop</h1>
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
        <h2>Home</h2>
        <div id="home-content"></div>
    </section>

    <!-- Books Section -->
    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container" id="book-list"></div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p id="contact-content">For inquiries, email us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
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
        <form id="edit-home-form">
            <h3>Edit Home Content</h3>
            <textarea id="home-edit" rows="5"></textarea>
            <button type="button" onclick="saveContent('home')">Save Changes</button>
        </form>

        <form id="edit-books-form">
            <h3>Manage Books</h3>
            <textarea id="books-edit" rows="5"></textarea>
            <button type="button" onclick="saveContent('books')">Save Changes</button>
        </form>

        <form id="edit-contact-form">
            <h3>Edit Contact Content</h3>
            <textarea id="contact-edit" rows="5"></textarea>
            <button type="button" onclick="saveContent('contact')">Save Changes</button>
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

        // Admin Credentials
        const adminUsername = "admin";
        const adminPassword = "password123";

        // Admin Login
        document.getElementById("login-form").addEventListener("submit", function (e) {
            e.preventDefault();
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;

            if (username === adminUsername && password === adminPassword) {
                navigateTo("admin-panel");
                loadContent();
            } else {
                document.getElementById("login-error").style.display = "block";
            }
        });

        // Save Content
        function saveContent(section) {
            const content = document.getElementById(`${section}-edit`).value;
            localStorage.setItem(`${section}-content`, content);
            alert(`${section} content saved!`);
            loadContent();
        }

        // Load Content
        function loadContent() {
            ['home', 'books', 'contact'].forEach((section) => {
                const content = localStorage.getItem(`${section}-content`);
                if (content) {
                    document.getElementById(`${section}-content`).innerHTML = content;
                    document.getElementById(`${section}-edit`).value = content;
                }
            });
        }

        // Initial Load
        loadContent();
    </script>
</body>
</html>
