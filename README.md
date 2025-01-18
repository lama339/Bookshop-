<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1 id="site-title">Bookshop</h1>
    </header>
    <section id="home-content">
        <h2 id="home-title">Welcome to Our Bookshop</h2>
        <p id="home-description">Discover a wide range of books that cater to every interest. Whether you're into fiction, non-fiction, or special genres, we have something for everyone!</p>
        <img id="home-image" src="images/default.jpg" alt="Bookshop Image">
    </section>
    <footer>
        <nav>
            <a href="index.html">Home</a>
            <a href="books.html">Books</a>
            <a href="contact.html">Contact</a>
            <a href="admin.php">Admin</a>
        </nav>
    </footer>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Books - Bookshop</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Bookshop</h1>
    </header>
    <section id="books-list">
        <!-- Dynamically loaded books will appear here -->
    </section>
    <footer>
        <nav>
            <a href="index.html">Home</a>
            <a href="books.html">Books</a>
            <a href="contact.html">Contact</a>
            <a href="admin.php">Admin</a>
        </nav>
    </footer>
    <script src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contact - Bookshop</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Bookshop</h1>
    </header>
    <section id="contact-info">
        <h2>Contact Us</h2>
        <p id="contact-description">Feel free to reach out to us for any inquiries, feedback, or book requests!</p>
        <img id="contact-image" src="images/default_contact.jpg" alt="Contact Image">
    </section>
    <footer>
        <nav>
            <a href="index.html">Home</a>
            <a href="books.html">Books</a>
            <a href="contact.html">Contact</a>
            <a href="admin.php">Admin</a>
        </nav>
    </footer>
</body>
</html>
<?php
// Admin login verification
session_start();
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    if ($_POST['username'] == 'admin' && $_POST['password'] == 'adminpassword') {
        $_SESSION['logged_in'] = true;
    }
}

if (!isset($_SESSION['logged_in'])) {
    header('Location: login.php');
    exit;
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Admin Panel</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Admin Panel</h1>
    </header>
    <section id="admin-section">
        <h2>Home Page Content</h2>
        <label for="home-title">Title:</label>
        <input type="text" id="home-title" value="Welcome to Our Bookshop">
        <label for="home-description">Description:</label>
        <textarea id="home-description">Discover a wide range of books that cater to every interest...</textarea>
        <label for="home-image">Select Image:</label>
        <input type="file" id="home-image" accept="image/*">
        
        <h2>Contact Page Content</h2>
        <label for="contact-description">Description:</label>
        <textarea id="contact-description">Feel free to reach out to us for any inquiries...</textarea>
        <label for="contact-image">Select Image:</label>
        <input type="file" id="contact-image" accept="image/*">

        <h2>Books</h2>
        <div id="books-list-admin">
            <!-- Dynamically load book data here -->
        </div>

        <button id="save-changes">Save Changes</button>
    </section>

    <footer>
        <a href="index.html">Home</a>
        <a href="books.html">Books</a>
        <a href="contact.html">Contact</a>
    </footer>
    <script src="script.js"></script>
</body>
</html>
<?php
// Assuming a simple file-based approach for simplicity
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $data = $_POST['data'];
    file_put_contents('data.json', json_encode($data));
}
?>
body {
    font-family: Arial, sans-serif;
}

header {
    background-color: #4CAF50;
    color: white;
    padding: 10px;
    text-align: center;
}

footer {
    background-color: #333;
    color: white;
    text-align: center;
    padding: 10px;
}

nav a {
    margin: 0 10px;
    color: white;
    text-decoration: none;
}

section {
    padding: 20px;
}

#site-title {
    color: darkgreen;
}

#home-image, #contact-image {
    width: 100%;
    height: auto;
}

#books-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.book-item {
    border: 1px solid #ccc;
    margin: 10px;
    padding: 10px;
    text-align: center;
}

.book-item img {
    width: 100px;
    height: 150px;
}
document.addEventListener('DOMContentLoaded', function() {
    loadHomeContent();
    loadBooks();
});

function loadHomeContent() {
    const homeTitle = localStorage.getItem('homeTitle') || 'Welcome to Our Bookshop';
    const homeDescription = localStorage.getItem('homeDescription') || 'Discover a wide range of books that cater to every interest...';
    const homeImage = localStorage.getItem('homeImage') || 'images/default.jpg';

    document.getElementById('home-title').innerText = homeTitle;
    document.getElementById('home-description').innerText = homeDescription;
    document.getElementById('home-image').src = homeImage;
}

function loadBooks() {
    const books = JSON.parse(localStorage.getItem('books')) || [];
    const booksList = document.getElementById('books-list');
    booksList.innerHTML = '';

    books.forEach(book => {
        const bookElement = document.createElement('div');
        bookElement.classList.add('book-item');
        bookElement.innerHTML = `
            <img src="${book.image}" alt="${book.title}">
            <h3>${book.title}</h3>
            <p>$${book.price}</p>
        `;
        booksList.appendChild(bookElement);
    });
}

document.getElementById('save-changes').addEventListener('click', function() {
    const homeTitle = document.getElementById('home-title').value;
    const homeDescription = document.getElementById('home-description').value;
    const homeImage = document.getElementById('home-image').files[0]?.name || 'images/default.jpg';
    
    localStorage.setItem('homeTitle', homeTitle);
    localStorage.setItem('homeDescription', homeDescription);
    localStorage.setItem('homeImage', homeImage);
    
    alert('Changes Saved!');
});
