<?php
session_start();
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    if (isset($_POST['username']) && isset($_POST['password'])) {
        if ($_POST['username'] == 'admin' && $_POST['password'] == 'admin123') {
            $_SESSION['loggedin'] = true;
            header("Location: #admin");
        } else {
            $error = "Invalid username or password.";
        }
    }
}

// Save Changes functionality
if (isset($_POST['save_changes'])) {
    $data = json_decode(file_get_contents('config.json'), true);

    if (isset($_POST['home_text'])) {
        $data['home_text'] = $_POST['home_text'];
    }
    if (isset($_POST['contact_info'])) {
        $data['contact_info'] = $_POST['contact_info'];
    }
    if (isset($_POST['book_title']) && isset($_POST['book_image']) && isset($_POST['book_price'])) {
        $new_book = [
            'title' => $_POST['book_title'],
            'image' => $_POST['book_image'],
            'price' => $_POST['book_price']
        ];
        $data['books'][] = $new_book;
    }

    file_put_contents('config.json', json_encode($data, JSON_PRETTY_PRINT));
    header("Location: #admin");
}

$config = json_decode(file_get_contents('config.json'), true);
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
        }
        header {
            background-color: #004d00;
            padding: 20px;
            color: white;
            text-align: center;
        }
        nav ul {
            list-style: none;
            padding: 0;
        }
        nav ul li {
            display: inline;
            margin-right: 20px;
        }
        a {
            color: white;
            text-decoration: none;
        }
        h1, h2 {
            color: darkgreen;
        }
        section {
            padding: 20px;
            margin: 10px;
        }
        .book-list {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
        }
        .book-item {
            margin: 10px;
            padding: 10px;
            border: 1px solid #ccc;
        }
        form input, form textarea {
            width: 100%;
            margin-bottom: 10px;
            padding: 10px;
        }
        button {
            background-color: darkgreen;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
        }
        .error {
            color: red;
        }
    </style>
</head>
<body>

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

<!-- Home Page -->
<section id="home">
    <div class="content">
        <h2>Welcome to our Bookshop</h2>
        <p><?php echo $config['home_text']; ?></p>
    </div>
</section>

<!-- Books Page -->
<section id="books">
    <h2>Our Books</h2>
    <div class="book-list">
        <?php
        foreach ($config['books'] as $book) {
            echo "<div class='book-item'>";
            echo "<img src='{$book['image']}' alt='{$book['title']}'>";
            echo "<h3>{$book['title']}</h3>";
            echo "<p>Price: {$book['price']}</p>";
            echo "</div>";
        }
        ?>
    </div>
</section>

<!-- Contact Page -->
<section id="contact">
    <h2>Contact Us</h2>
    <form method="post">
        <label for="name">Name</label>
        <input type="text" id="name" name="name">
        <label for="message">Message</label>
        <textarea id="message" name="message"></textarea>
        <button type="submit">Send Message</button>
    </form>
</section>

<!-- Admin Panel -->
<section id="admin">
    <h2>Admin Panel</h2>

    <?php if (!isset($_SESSION['loggedin'])): ?>
        <h3>Login</h3>
        <form method="post">
            <label for="username">Username</label>
            <input type="text" id="username" name="username">
            <label for="password">Password</label>
            <input type="password" id="password" name="password">
            <button type="submit">Login</button>
        </form>
        <?php if (isset($error)) echo "<p class='error'>$error</p>"; ?>
    <?php else: ?>
        <h3>Update Home Text</h3>
        <form method="post">
            <textarea name="home_text"><?php echo $config['home_text']; ?></textarea>
            <button type="submit" name="save_changes">Save Changes</button>
        </form>

        <h3>Update Contact Info</h3>
        <form method="post">
            <textarea name="contact_info"><?php echo $config['contact_info']; ?></textarea>
            <button type="submit" name="save_changes">Save Changes</button>
        </form>

        <h3>Add New Book</h3>
        <form method="post">
            <input type="text" name="book_title" placeholder="Book Title">
            <input type="text" name="book_image" placeholder="Image URL">
            <input type="number" name="book_price" placeholder="Price">
            <button type="submit" name="save_changes">Add Book</button>
        </form>
    <?php endif; ?>
</section>

</body>
</html>

<?php
// Data in config.json (initial structure)
file_put_contents('config.json', json_encode([
    "home_text" => "Welcome to our Bookshop. Browse and discover new books!",
    "contact_info" => "You can reach us via email at info@bookshop.com.",
    "books" => [
        [
            "title" => "Book 1",
            "image" => "https://example.com/book1.jpg",
            "price" => "$10.99"
        ],
        [
            "title" => "Book 2",
            "image" => "https://example.com/book2.jpg",
            "price" => "$12.99"
        ]
    ]
], JSON_PRETTY_PRINT));
?>
