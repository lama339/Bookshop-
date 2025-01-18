<?php
session_start();

// Predefined username and password
$valid_username = "almanhal";
$valid_password = "almanhal123";

// Handle login and saving
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    if ($_POST['username'] === $valid_username && $_POST['password'] === $valid_password) {
        $_SESSION['authenticated'] = true;
    } else {
        echo "<script>alert('Invalid login credentials');</script>";
    }
}

// Check if user is authenticated
$isAuthenticated = isset($_SESSION['authenticated']) && $_SESSION['authenticated'] == true;

// Variables for page content
$homeText = "Welcome to Our Bookshop. Browse through a wide collection of books.";
$contactText = "If you have any questions, feel free to reach out to us.";
$books = [
    ['title' => 'Book Title 1', 'price' => '$15.99', 'image' => 'images/book1.jpg'],
    ['title' => 'Book Title 2', 'price' => '$20.99', 'image' => 'images/book2.jpg'],
];

// Save changes to text and books
if ($_SERVER['REQUEST_METHOD'] == 'POST' && $isAuthenticated) {
    if (isset($_POST['homeText'])) {
        $homeText = $_POST['homeText'];
    }
    if (isset($_POST['contactText'])) {
        $contactText = $_POST['contactText'];
    }
    if (isset($_POST['bookTitle']) && isset($_POST['bookPrice'])) {
        $books[] = [
            'title' => $_POST['bookTitle'],
            'price' => $_POST['bookPrice'],
            'image' => 'images/new_book.jpg' // Placeholder for new book image
        ];
    }
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="background-image" style="background-image: url('images/background.jpg');">
        <header>
            <h1>Bookshop</h1>
            <nav>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#books">Books</a></li>
                    <li><a href="#contact">Contact</a></li>
                    <li><a href="#admin">Admin</a></li>
                </ul>
            </nav>
        </header>

        <div id="home">
            <h2>Welcome to Our Bookshop</h2>
            <p><?php echo $homeText; ?></p>
        </div>

        <div id="books">
            <h2>Our Books</h2>
            <div class="book-list">
                <?php foreach ($books as $book): ?>
                    <div class="book-item">
                        <img src="<?php echo $book['image']; ?>" alt="Book Image">
                        <h3><?php echo $book['title']; ?></h3>
                        <p><?php echo $book['price']; ?></p>
                    </div>
                <?php endforeach; ?>
            </div>
        </div>

        <div id="contact">
            <h2>Contact Us</h2>
            <p><?php echo $contactText; ?></p>
        </div>

        <?php if ($isAuthenticated): ?>
            <div id="admin">
                <h2>Admin Panel</h2>
                <form action="" method="POST">
                    <h3>Edit Home Page Text</h3>
                    <textarea name="homeText" required><?php echo $homeText; ?></textarea>
                    <h3>Edit Contact Page Text</h3>
                    <textarea name="contactText" required><?php echo $contactText; ?></textarea>

                    <h3>Add a New Book</h3>
                    <input type="text" name="bookTitle" placeholder="Book Title" required>
                    <input type="text" name="bookPrice" placeholder="Price" required>

                    <button type="submit">Save Changes</button>
                </form>
            </div>
        <?php else: ?>
            <div id="admin-login">
                <h2>Admin Login</h2>
                <form action="" method="POST">
                    <label for="username">Username:</label>
                    <input type="text" name="username" id="username" required>
                    <label for="password">Password:</label>
                    <input type="password" name="password" id="password" required>
                    <button type="submit">Login</button>
                </form>
            </div>
        <?php endif; ?>
    </div>
</body>
</html>

<style>
    body {
        font-family: Arial, sans-serif;
    }

    .background-image {
        background-size: cover;
        height: 100vh;
        padding: 20px;
        color: black;
    }

    header h1 {
        color: darkgreen;
        text-align: center;
        font-size: 3em;
    }

    nav ul {
        display: flex;
        justify-content: center;
        list-style: none;
    }

    nav ul li {
        margin: 0 20px;
    }

    nav ul li a {
        color: black;
        text-decoration: none;
        font-size: 1.2em;
    }

    h2 {
        text-align: center;
        font-size: 2em;
    }

    .book-list {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
    }

    .book-item {
        text-align: center;
        margin: 20px;
    }

    .book-item img {
        width: 150px;
        height: 200px;
    }

    #admin {
        background-color: rgba(255, 255, 255, 0.8);
        padding: 20px;
        border-radius: 10px;
        margin-top: 30px;
    }

    #admin-login {
        text-align: center;
        padding: 20px;
        background-color: rgba(255, 255, 255, 0.8);
        border-radius: 10px;
    }

    input, textarea {
        width: 100%;
        padding: 10px;
        margin: 10px 0;
        font-size: 1em;
    }

    button {
        padding: 10px 20px;
        font-size: 1.2em;
        background-color: darkgreen;
        color: white;
        border: none;
        cursor: pointer;
    }

    button:hover {
        background-color: green;
    }
</style>

<script>
    // Additional JavaScript code can be added if necessary
</script>
