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
        }

        header {
            background-color: #f8f8f8;
            padding: 20px;
            text-align: center;
        }

        h1 {
            font-size: 36px;
        }

        footer {
            text-align: center;
            margin-top: 20px;
        }

        a {
            text-decoration: none;
            color: black;
            margin: 0 10px;
        }

        #home-content, #contact-content {
            text-align: center;
            margin-top: 20px;
        }

        #book-list {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
        }

        .book-item {
            margin: 10px;
            text-align: center;
        }

        #admin-panel {
            width: 80%;
            margin: 0 auto;
            padding: 20px;
        }

        #admin-panel input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
        }

        #admin-panel button {
            padding: 10px 20px;
            margin-top: 10px;
            background-color: darkgreen;
            color: white;
            border: none;
            cursor: pointer;
        }

        #admin-panel button:hover {
            background-color: green;
        }

        #book-list-admin {
            margin-top: 20px;
        }
    </style>
</head>
<body>

<!-- Home Page -->
<div id="home-page">
    <header>
        <h1 style="color: darkgreen;">Welcome to Our Bookshop</h1>
    </header>

    <div id="home-content">
        <p id="home-text">Find your favorite books here!</p>
        <img id="home-image" src="default-home.jpg" alt="Bookshop Image">
    </div>

    <footer>
        <a href="#books-page">Books</a> |
        <a href="#contact-page">Contact</a> |
        <a href="#admin-page">Admin</a>
    </footer>
</div>

<!-- Books Page -->
<div id="books-page" style="display: none;">
    <header>
        <h1 style="color: darkgreen;">Our Books</h1>
    </header>

    <div id="book-list">
        <!-- Dynamically generated book content will go here -->
    </div>

    <footer>
        <a href="#home-page">Home</a> |
        <a href="#contact-page">Contact</a> |
        <a href="#admin-page">Admin</a>
    </footer>
</div>

<!-- Contact Page -->
<div id="contact-page" style="display: none;">
    <header>
        <h1 style="color: darkgreen;">Contact Us</h1>
    </header>

    <div id="contact-content">
        <p id="contact-text">Feel free to reach out to us!</p>
        <img id="contact-image" src="default-contact.jpg" alt="Contact Image">
    </div>

    <footer>
        <a href="#home-page">Home</a> |
        <a href="#books-page">Books</a> |
        <a href="#admin-page">Admin</a>
    </footer>
</div>

<!-- Admin Panel -->
<div id="admin-page" style="display: none;">
    <header>
        <h1 style="color: darkgreen;">Admin Panel</h1>
    </header>

    <div id="admin-panel">
        <h2>Edit Home Page</h2>
        <label for="home-text">Text:</label>
        <input type="text" id="home-text">
        <br>
        <label for="home-image">Home Image URL:</label>
        <input type="text" id="home-image">
        
        <h2>Edit Contact Page</h2>
        <label for="contact-text">Text:</label>
        <input type="text" id="contact-text">
        <br>
        <label for="contact-image">Contact Image URL:</label>
        <input type="text" id="contact-image">

        <h2>Edit Books</h2>
        <input type="text" id="book-title" placeholder="Book Title">
        <input type="text" id="book-price" placeholder="Book Price">
        <input type="text" id="book-image" placeholder="Book Image URL">
        <button id="add-book">Add Book</button>

        <div id="book-list-admin">
            <!-- Book list will be dynamically displayed here -->
        </div>

        <button id="save-changes">Save Changes</button>
    </div>

    <footer>
        <a href="#home-page">Home</a> |
        <a href="#books-page">Books</a> |
        <a href="#contact-page">Contact</a>
    </footer>
</div>

<script>
// Handle navigation between pages
const pages = ['home-page', 'books-page', 'contact-page', 'admin-page'];
const links = document.querySelectorAll('footer a');

links.forEach(link => {
    link.addEventListener('click', function (e) {
        e.preventDefault();
        const targetPage = link.getAttribute('href').substring(1);
        showPage(targetPage);
    });
});

function showPage(pageId) {
    pages.forEach(page => {
        document.getElementById(page).style.display = (page === pageId) ? 'block' : 'none';
    });
}

// JavaScript for Admin Panel functionality
document.addEventListener("DOMContentLoaded", function () {
    const homeTextInput = document.getElementById('home-text');
    const homeImageInput = document.getElementById('home-image');
    const contactTextInput = document.getElementById('contact-text');
    const contactImageInput = document.getElementById('contact-image');
    const bookTitleInput = document.getElementById('book-title');
    const bookPriceInput = document.getElementById('book-price');
    const bookImageInput = document.getElementById('book-image');
    const addBookButton = document.getElementById('add-book');
    const saveChangesButton = document.getElementById('save-changes');
    
    let books = JSON.parse(localStorage.getItem('books')) || [];

    function updateBookList() {
        const bookListContainer = document.getElementById('book-list-admin');
        bookListContainer.innerHTML = '';
        books.forEach((book, index) => {
            const bookItem = document.createElement('div');
            bookItem.classList.add('book-item');
            bookItem.innerHTML = `
                <h3>${book.title}</h3>
                <img src="${book.image}" alt="${book.title}" width="100">
                <p>$${book.price}</p>
                <button onclick="deleteBook(${index})">Delete</button>
            `;
            bookListContainer.appendChild(bookItem);
        });
    }

    addBookButton.addEventListener('click', function () {
        const book = {
            title: bookTitleInput.value,
            price: bookPriceInput.value,
            image: bookImageInput.value
        };
        books.push(book);
        localStorage.setItem('books', JSON.stringify(books));
        updateBookList();
    });

    function deleteBook(index) {
        books.splice(index, 1);
        localStorage.setItem('books', JSON.stringify(books));
        updateBookList();
    }

    saveChangesButton.addEventListener('click', function () {
        const homeText = homeTextInput.value;
        const homeImage = homeImageInput.value;
        const contactText = contactTextInput.value;
        const contactImage = contactImageInput.value;

        localStorage.setItem('homeText', homeText);
        localStorage.setItem('homeImage', homeImage);
        localStorage.setItem('contactText', contactText);
        localStorage.setItem('contactImage', contactImage);
    });

    function loadSavedContent() {
        const homeText = localStorage.getItem('homeText');
        const homeImage = localStorage.getItem('homeImage');
        const contactText = localStorage.getItem('contactText');
        const contactImage = localStorage.getItem('contactImage');

        if (homeText) homeTextInput.value = homeText;
        if (homeImage) homeImageInput.value = homeImage;
        if (contactText) contactTextInput.value = contactText;
        if (contactImage) contactImageInput.value = contactImage;
    }

    loadSavedContent();
    updateBookList();
});
</script>

</body>
</html>
