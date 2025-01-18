<?php
session_start();

// Predefined username and password
$valid_username = "almanhal";
$valid_password = "almanhal123";

// Check login credentials
if ($_POST['username'] === $valid_username && $_POST['password'] === $valid_password) {
    $_SESSION['authenticated'] = true;
    header("Location: #admin");
} else {
    if ($_SERVER['REQUEST_METHOD'] == 'POST') {
        echo "Invalid login.";
    }
}

$homeText = "Welcome to Our Bookshop. Browse through a wide collection of books.";
$contactText = "If you have any questions, feel free to reach out to us.";
$books = [
    ["title" => "Book Title 1", "price" => "$15.99", "image" => "images/book1.jpg"],
    ["title" => "Book Title 2", "price" => "$20.99", "image" => "images/book2.jpg"]
];

if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['homeText']) && isset($_POST['contactText'])) {
    $homeText = $_POST['homeText'];
    $contactText = $_POST['contactText'];
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bookshop</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: Arial, sans-serif;
    }

    .background-image {
      background-image: url('https://images.app.goo.gl/X92EKtHc2d7KxrAm6'); /* Replace with your actual image URL */
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

    #book-list {
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

    #admin-content {
      width: 80%;
      margin: auto;
      text-align: center;
    }

    textarea {
      width: 100%;
      height: 200px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="background-image">
    <header>
      <h1>Bookshop</h1>
      <nav>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#books">Books</a></li>
          <li><a href="#contact">Contact</a></li>
          <li><a href="#admin">Admin</a></li>
        </ul>
      </nav>
    </header>

    <!-- Home Section -->
    <main id="home">
      <h2>Welcome to Our Bookshop</h2>
      <p id="home-text"><?= $homeText ?></p>
    </main>

    <!-- Books Section -->
    <section id="books">
      <h2>Our Books</h2>
      <div id="book-list">
        <?php foreach ($books as $book): ?>
        <div class="book-item">
          <img src="<?= $book['image'] ?>" alt="Book Image">
          <h3><?= $book['title'] ?></h3>
          <p><?= $book['price'] ?></p>
        </div>
        <?php endforeach; ?>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact">
      <h2>Contact Us</h2>
      <p id="contact-text"><?= $contactText ?></p>
    </section>

    <!-- Admin Panel Section -->
    <section id="admin">
      <h2>Admin Panel</h2>
      <?php if (!isset($_SESSION['authenticated'])): ?>
        <form method="POST">
          <label for="username">Username:</label>
          <input type="text" name="username" id="username" required>
          
          <label for="password">Password:</label>
          <input type="password" name="password" id="password" required>
          
          <button type="submit">Login</button>
        </form>
      <?php else: ?>
        <form method="POST">
          <h3>Edit Home Page Text</h3>
          <textarea name="homeText"><?= $homeText ?></textarea>
          
          <h3>Edit Contact Page Text</h3>
          <textarea name="contactText"><?= $contactText ?></textarea>
          
          <h3>Books</h3>
          <div>
            <label for="book-title">Book Title:</label>
            <input type="text" id="book-title">
            <label for="book-price">Price:</label>
            <input type="text" id="book-price">
            <button type="button" onclick="addBook()">Add Book</button>
          </div>

          <h4>Books List:</h4>
          <div id="books-list"></div>

          <button type="submit">Save Changes</button>
        </form>
      <?php endif; ?>
    </section>
  </div>

  <script>
    const booksListDiv = document.getElementById("books-list");

    let books = <?= json_encode($books); ?>;
    
    function addBook() {
      const title = document.getElementById("book-title").value;
      const price = document.getElementById("book-price").value;

      if (title && price) {
        books.push({ title: title, price: price, image: "images/book3.jpg" });
        displayBooks();
      }
    }

    function displayBooks() {
      booksListDiv.innerHTML = "";
      books.forEach((book) => {
        const div = document.createElement("div");
        div.innerHTML = `<p>${book.title} - ${book.price}</p>`;
        booksListDiv.appendChild(div);
      });
    }

    displayBooks();
  </script>
</body>
</html>
