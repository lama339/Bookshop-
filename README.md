<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Almanhal Bookshop (المنهل)</title>
  <style>
    /* Global Styles */
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background-color: #f9f9f9;
    }

    header, footer {
      background-color: #0044cc;
      color: white;
      padding: 10px;
    }

    nav a {
      text-decoration: none;
      color: white;
      margin: 0 15px;
    }

    nav a:hover {
      text-decoration: underline;
    }

    main {
      padding: 20px;
    }

    input[type="text"], input[type="password"] {
      width: 80%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 5px;
    }

    button {
      background-color: #007b5e;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background-color: #005f46;
    }

    .book-item {
      display: inline-block;
      width: 200px;
      margin: 15px;
      padding: 10px;
      background-color: #f4f4f4;
      border: 1px solid #ddd;
      text-align: center;
      border-radius: 8px;
    }

    .book-item img {
      width: 100%;
      height: auto;
      border-radius: 5px;
    }

    .book-item h3 {
      color: #333;
      font-size: 18px;
    }

    .book-item p {
      color: #007b5e;
      font-weight: bold;
    }
  </style>
</head>

<body>
  <header>
    <h1>Almanhal Bookshop (المنهل)</h1>
    <nav>
      <a href="#" onclick="loadPage('home')">Home</a> |
      <a href="#" onclick="loadPage('bookshop')">Bookshop</a> |
      <a href="#" onclick="loadPage('admin')">Admin</a>
    </nav>
  </header>

  <main id="content">
    <!-- Content will be dynamically loaded here -->
  </main>

  <footer>
    <p>Phone: +123 456 789 | Instagram: @almanhal_bookshop</p>
  </footer>

  <script>
    const books = [
      { title: "The Great Gatsby", price: "$10", image: "https://via.placeholder.com/100" },
      { title: "1984", price: "$12", image: "https://via.placeholder.com/100" },
      { title: "Moby Dick", price: "$15", image: "https://via.placeholder.com/100" }
    ];

    function loadPage(page) {
      if (page === 'home') {
        document.getElementById('content').innerHTML = `
          <h2>Edit This Section</h2>
          <textarea id="editable-text" rows="4" cols="50">Welcome to the best place for books!</textarea>
          <button id="saveTextBtn">Save Text</button>
        `;

        const editableText = document.getElementById("editable-text");
        const saveTextBtn = document.getElementById("saveTextBtn");

        editableText.value = localStorage.getItem("welcomeText") || editableText.value;

        saveTextBtn.addEventListener("click", () => {
          localStorage.setItem("welcomeText", editableText.value);
          alert("Text saved successfully!");
        });
      } else if (page === 'bookshop') {
        document.getElementById('content').innerHTML = `
          <input type="text" id="searchBar" placeholder="Search for books...">
          <div id="bookList"></div>
        `;

        displayBooks(books);

        const searchBar = document.getElementById("searchBar");
        searchBar.addEventListener("input", () => {
          const filteredBooks = books.filter(book => 
            book.title.toLowerCase().includes(searchBar.value.toLowerCase())
          );
          displayBooks(filteredBooks);
        });
      } else if (page === 'admin') {
        document.getElementById('content').innerHTML = `
          <label for="username">Username:</label>
          <input type="text" id="username" placeholder="Enter username">
          <br>
          <label for="password">Password:</label>
          <input type="password" id="password" placeholder="Enter password">
          <br>
          <button id="loginBtn">Login</button>
          <p id="loginStatus"></p>
        `;

        const loginBtn = document.getElementById("loginBtn");
        const loginStatus = document.getElementById("loginStatus");

        loginBtn.addEventListener("click", () => {
          const username = document.getElementById("username").value;
          const password = document.getElementById("password").value;

          if (username === "almanhal3" && password === "al.bookshop25") {
            loginStatus.textContent = "Login Successful!";
            loginStatus.style.color = "green";
          } else {
            loginStatus.textContent = "Invalid Username or Password";
            loginStatus.style.color = "red";
          }
        });
      }
    }

    function displayBooks(bookArray) {
      const bookList = document.getElementById("bookList");
      bookList.innerHTML = bookArray.map(book => `
        <div class="book-item">
          <img src="${book.image}" alt="${book.title}">
          <h3>${book.title}</h3>
          <p>Price: ${book.price}</p>
        </div>
      `).join("");
    }

    // Load Home page initially
    loadPage('home');
  </script>
</body>
</html>
