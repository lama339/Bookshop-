<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Almanhal Bookshop (المنهل)</title>
  <style>
    /* Global Styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Arial', sans-serif;
      background-color: #f9f9f9;
      color: #333;
    }

    header {
      background-color: #2c3e50;
      padding: 20px;
      color: white;
      text-align: center;
    }

    nav {
      background-color: #34495e;
      display: flex;
      justify-content: center;
      padding: 10px 0;
    }

    nav a {
      color: white;
      text-decoration: none;
      margin: 0 15px;
      padding: 5px 10px;
    }

    nav a:hover {
      background-color: #16a085;
      border-radius: 5px;
    }

    main {
      padding: 20px;
    }

    .book-container {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 20px;
    }

    .book-item {
      background-color: white;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 15px;
      text-align: center;
      transition: box-shadow 0.3s;
    }

    .admin-form {
      max-width: 400px;
      margin: 50px auto;
      background-color: white;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
    }

    .admin-form input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 5px;
    }

    .admin-form button {
      width: 100%;
      padding: 10px;
      background-color: #16a085;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .admin-form button:hover {
      background-color: #1abc9c;
    }
  </style>
</head>

<body>
  <header>
    <h1>Almanhal Bookshop (المنهل)</h1>
  </header>

  <nav>
    <a href="#" onclick="showPage('home')">Home</a>
    <a href="#" onclick="showPage('bookshop')">Bookshop</a>
    <a href="#" onclick="showPage('admin')">Admin</a>
  </nav>

  <main id="content"></main>

  <footer>
    <p>Contact: +123 456 789 | Instagram: @almanhal_bookshop</p>
  </footer>

  <script>
    let books = JSON.parse(localStorage.getItem("books")) || [
      { title: "The Great Gatsby", price: "$10", image: "https://via.placeholder.com/150" },
      { title: "1984", price: "$12", image: "https://via.placeholder.com/150" },
    ];

    let homeContent = localStorage.getItem("homeContent") || "Welcome to Almanhal Bookshop! Your go-to place for all kinds of books!";
    let loggedIn = false;

    function showPage(page) {
      const content = document.getElementById('content');
      if (page === 'home') {
        content.innerHTML = `
          <h2>Home Page</h2>
          <p>${homeContent}</p>
        `;
      } else if (page === 'bookshop') {
        content.innerHTML = `
          <h2>Available Books</h2>
          <div class="book-container" id="bookList"></div>
        `;
        displayBooks();
      } else if (page === 'admin') {
        if (loggedIn) {
          showAdminPanel();
        } else {
          showLoginForm();
        }
      }
    }

    function displayBooks() {
      const bookList = document.getElementById('bookList');
      bookList.innerHTML = books.map(book => `
        <div class="book-item">
          <img src="${book.image}" alt="${book.title}">
          <h3>${book.title}</h3>
          <p>Price: ${book.price}</p>
        </div>
      `).join('');
    }

    function showLoginForm() {
      const content = document.getElementById('content');
      content.innerHTML = `
        <div class="admin-form">
          <h2>Admin Login</h2>
          <input type="text" id="username" placeholder="Enter Username">
          <input type="password" id="password" placeholder="Enter Password">
          <button onclick="adminLogin()">Login</button>
        </div>
      `;
    }

    function adminLogin() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;

      if (username === "almanhal3" && password === "al.bookshop25") {
        loggedIn = true;
        alert("Login successful!");
        showAdminPanel();
      } else {
        alert("Invalid credentials. Please try again.");
      }
    }

    function showAdminPanel() {
      const content = document.getElementById('content');
      content.innerHTML = `
        <h2>Edit Home Page Content</h2>
        <textarea id="homeContent" rows="5" style="width:100%;">${homeContent}</textarea>
        <button onclick="saveHomeContent()">Save Home Content</button>

        <h2>Edit Books</h2>
        ${books.map((book, index) => `
          <div class="book-item">
            <input type="text" id="title-${index}" value="${book.title}">
            <input type="text" id="price-${index}" value="${book.price}">
            <input type="text" id="image-${index}" value="${book.image}">
            <button onclick="saveBook(${index})">Save</button>
            <button onclick="deleteBook(${index})">Delete</button>
          </div>
        `).join('')}

        <h2>Add New Book</h2>
        <input type="text" id="new-title" placeholder="Title">
        <input type="text" id="new-price" placeholder="Price">
        <input type="text" id="new-image" placeholder="Image URL">
        <button onclick="addBook()">Add Book</button>
      `;
    }

    function saveHomeContent() {
      homeContent = document.getElementById("homeContent").value;
      localStorage.setItem("homeContent", homeContent);
      alert("Home content saved!");
    }

    function saveBook(index) {
      const title = document.getElementById(`title-${index}`).value;
      const price = document.getElementById(`price-${index}`).value;
      const image = document.getElementById(`image-${index}`).value;

      books[index] = { title, price, image };
      localStorage.setItem("books", JSON.stringify(books));
      alert("Book updated!");
    }

    function deleteBook(index) {
      books.splice(index, 1);
      localStorage.setItem("books", JSON.stringify(books));
      showAdminPanel();
      alert("Book deleted!");
    }

    function addBook() {
      const title = document.getElementById("new-title").value;
      const price = document.getElementById("new-price").value;
      const image = document.getElementById("new-image").value;

      if (title && price && image) {
        books.push({ title, price, image });
        localStorage.setItem("books", JSON.stringify(books));
        showAdminPanel();
        alert("Book added!");
      } else {
        alert("Please fill in all fields!");
      }
    }

    // Load the Home page initially
    showPage('home');
  </script>
</body>
</html>
