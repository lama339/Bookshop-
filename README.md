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

    /* Bookshop Styles */
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

    .book-item img {
      max-width: 100%;
      height: auto;
      border-radius: 5px;
    }

    .book-item:hover {
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }

    .book-item h3 {
      font-size: 18px;
      color: #2c3e50;
    }

    .book-item p {
      color: #16a085;
      font-weight: bold;
    }

    /* Footer Styles */
    footer {
      background-color: #2c3e50;
      color: white;
      text-align: center;
      padding: 10px 0;
    }

    /* Admin Form Styles */
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

  <main id="content">
    <!-- Dynamic Content Will Be Loaded Here -->
  </main>

  <footer>
    <p>Contact: +123 456 789 | Instagram: @almanhal_bookshop</p>
  </footer>

  <script>
    const books = [
      { title: "The Great Gatsby", price: "$10", image: "https://via.placeholder.com/150" },
      { title: "1984", price: "$12", image: "https://via.placeholder.com/150" },
      { title: "To Kill a Mockingbird", price: "$15", image: "https://via.placeholder.com/150" },
      { title: "Pride and Prejudice", price: "$8", image: "https://via.placeholder.com/150" },
    ];

    function showPage(page) {
      const content = document.getElementById('content');
      if (page === 'home') {
        content.innerHTML = `
          <h2>Welcome to Almanhal Bookshop</h2>
          <p>Your go-to place for all kinds of books!</p>
        `;
      } else if (page === 'bookshop') {
        content.innerHTML = `
          <input type="text" id="searchBar" placeholder="Search for books..." oninput="filterBooks()">
          <div class="book-container" id="bookList"></div>
        `;
        displayBooks(books);
      } else if (page === 'admin') {
        content.innerHTML = `
          <div class="admin-form">
            <h2>Admin Login</h2>
            <input type="text" id="username" placeholder="Enter Username">
            <input type="password" id="password" placeholder="Enter Password">
            <button onclick="adminLogin()">Login</button>
            <p id="loginStatus"></p>
          </div>
        `;
      }
    }

    function displayBooks(bookArray) {
      const bookList = document.getElementById('bookList');
      bookList.innerHTML = bookArray.map(book => `
        <div class="book-item">
          <img src="${book.image}" alt="${book.title}">
          <h3>${book.title}</h3>
          <p>Price: ${book.price}</p>
        </div>
      `).join('');
    }

    function filterBooks() {
      const searchValue = document.getElementById('searchBar').value.toLowerCase();
      const filteredBooks = books.filter(book => book.title.toLowerCase().includes(searchValue));
      displayBooks(filteredBooks);
    }

    function adminLogin() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;
      const loginStatus = document.getElementById('loginStatus');

      if (username === "almanhal3" && password === "al.bookshop25") {
        loginStatus.textContent = "Login Successful!";
        loginStatus.style.color = "green";
      } else {
        loginStatus.textContent = "Invalid Username or Password";
        loginStatus.style.color = "red";
      }
    }

    // Load the Home page initially
    showPage('home');
  </script>
</body>
</html>
