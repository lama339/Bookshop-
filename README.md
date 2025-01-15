# Bookshop-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: pink; /* Pink background */
            color: #000; /* Black text */
        }
        header {
            background-color: #333; /* Dark pink */
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
        }
        .hidden {
            display: none;
        }
        h2 {
            color: #333;
        }
        footer {
            background-color: #FF6F61;
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
            gap: 20px;
        }
        .book {
            border: 1px solid #ccc;
            padding: 10px;
            width: 200px;
            background-color: white;
        }
        button {
            background-color: #FF6F61;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            opacity: 0.9;
        }
        input {
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
                <li><a onclick="navigateTo('admin')">Admin</a></li>
            </ul>
        </nav>
    </header>

    <!-- Home Section -->
    <section id="home">
        <h2>Welcome to Our Bookshop</h2>
        <p>Explore a wide range of books and discover your next favorite read!</p>
    </section>

    <!-- Books Section -->
    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container" id="book-list"></div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p>If you have any questions, email us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
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

        <div>
            <h3>Manage Books</h3>
            <ul id="admin-book-list"></ul>
        </div>
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

        // Handle Admin Login
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

        // Books Data
        const books = [];
        const bookList = document.getElementById("book-list");
        const adminBookList = document.getElementById("admin-book-list");

        // Add Book
        document.getElementById("add-book-form").addEventListener("submit", function (e) {
            e.preventDefault();
            const title = document.getElementById("book-title").value;
            const author = document.getElementById("book-author").value;
            const price = document.getElementById("book-price").value;

            books.push({ title, author, price });
            updateBookList();
            updateAdminBookList();

            // Clear form
            e.target.reset();
        });

        // Update Book List (Customer View)
        function updateBookList() {
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

        // Update Admin Book List (Admin View)
        function updateAdminBookList() {
            adminBookList.innerHTML = "";
            books.forEach((book, index) => {
                const li = document.createElement("li");
                li.textContent = `${book.title} by ${book.author} - $${book.price}`;
                const deleteButton = document.createElement("button");
                deleteButton.textContent = "Delete";
                deleteButton.onclick = () => {
                    books.splice(index, 1);
                    updateBookList();
                    updateAdminBookList();
                };
                li.appendChild(deleteButton);
                adminBookList.appendChild(li);
            });
        }
    </script>
</body>

        }
        
    </style>
</head>
<body>
    <section id="home">
        <h2>Home</h2>
        <p>Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container"></div>
    </section>

    
    
        // Load Books
        const books = Array.from({ length: 100 }, (_, i) => ({
            id: i + 1,
            title: `Book Title ${i + 1}`,
            author: `Author ${i + 1}`,
            price: (Math.random() * 20 + 5).toFixed(2),
        }));

        const bookContainer = document.querySelector(".book-container");
        books.forEach((book) => {
            const bookDiv = document.createElement("div");
            bookDiv.className = "book";
            bookDiv.innerHTML = `
                <h3>${book.title}</h3>
                <p>Author: ${book.author}</p>
                <p>Price: $${book.price}</p>
                <button onclick="addToCart(${book.id}, '${book.title}', ${book.price})">Add to Cart</button>
            `;
            bookContainer.appendChild(bookDiv);
        });

        // Cart Functionality
        const cart = [];
        const cartItemsList = document.getElementById("cart-items");
        const cartTotal = document.getElementById("cart-total");

        function addToCart(id, name, price) {
            const existing = cart.find((item) => item.id === id);
            if (existing) {
                existing.quantity++;
            } else {
                cart.push({ id, name, price, quantity: 1 });
            }
            updateCart();
        }

        function updateCart() {
            cartItemsList.innerHTML = "";
            let total = 0;

            cart.forEach((item) => {
                total += item.price * item.quantity;
                const li = document.createElement("li");
                li.textContent = `${item.name} - $${item.price} x ${item.quantity}`;
                cartItemsList.appendChild(li);
            });

            total += 3; // Delivery Fee
            cartTotal.textContent = total.toFixed(2);
        }

        // Handle Checkout Form
        document.getElementById("checkout-form").addEventListener("submit", (e) => {
            e.preventDefault();

            const name = document.getElementById("customer-name").value;
            const address = document.getElementById("address").value;
            const city = document.getElementById("city").value;
            const phone = document.getElementById("phone").value;

            if (cart.length === 0) {
                alert("Your cart is empty. Please add items to the cart before purchasing.");
                return;
            }

            const orderDetails = cart.map((item) => `${item.name} x${item.quantity}`).join("\n");
            const totalAmount = cart.reduce((sum, item) => sum + item.price * item.quantity, 0) + 3;

            const message = `
                New Order from ${name}:

                Order Details:
                ${orderDetails}

                Address: ${address}, ${city}
                Phone: ${phone}

                Total: $${totalAmount.toFixed(2)}
            `;

            // Send email via EmailJS
            emailjs.send("your_service_id", "your_template_id", {
                to_email: "your_email@example.com", // Replace with your email
                subject: "New Order",
                message: message,
            })
            .then(() => {
                alert("Your order has been placed successfully!");
                cart.length = 0;
                updateCart();
                e.target.reset();
            })
            .catch(() => alert("Failed to send order. Please try again."));
        });
    </script>
</body>
</html>
