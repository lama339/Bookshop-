<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Almanhal Bookshop</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    /* Reset and Global Styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Arial", sans-serif;
    }

    body {
      display: flex;
      background-color: #f8f9fa;
    }

    /* Sidebar */
    .sidebar {
      width: 250px;
      background-color: #2c3e50;
      height: 100vh;
      position: fixed;
      padding: 20px;
      color: white;
      transition: 0.3s;
    }

    .sidebar.collapsed {
      width: 60px;
    }

    .sidebar a {
      display: block;
      padding: 10px;
      text-decoration: none;
      color: white;
    }

    button {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
    }

    /* Main Content */
    main {
      margin-left: 270px;
      padding: 20px;
      width: 100%;
    }

    section {
      display: none;
      padding: 20px;
      background-color: #ffffff;
      border-radius: 5px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    section.active {
      display: block;
    }

    h1, h2 {
      color: #2c3e50;
    }

    /* Book List */
    .book-list {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
    }

    .book-item {
      background-color: #f1f1f1;
      padding: 15px;
      text-align: center;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .book-item img {
      max-width: 100%;
      height: 150px;
      object-fit: cover;
      border-radius: 8px;
    }

    /* Admin Panel */
    form {
      display: flex;
      flex-direction: column;
      width: 300px;
    }

    input[type="text"],
    input[type="password"],
    input[type="file"] {
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 5px;
    }

    button[type="submit"],
    button.edit {
      padding: 10px;
      background-color: #27ae60;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<!-- Sidebar -->
<div class="sidebar" id="sidebar">
  <button onclick="toggleSidebar()">☰</button>
  <nav id="menu">
    <a href="#" onclick="showSection('home')">Home</a>
    <a href="#" onclick="showSection('books')">Books</a>
    <a href="#" onclick="showSection('contact')">Contact</a>
    <a href="#" onclick="showSection('admin')">Admin Panel</a>
  </nav>
</div>

<!-- Main Content -->
<main>
  <!-- Home Section -->
  <section id="home" class="active">
    <h1>Welcome to Almanhal Bookshop</h1>
    <p>Your one-stop shop for all kinds of books.</p>
  </section>

  <!-- Books Section -->
  <section id="books">
    <h2>Books</h2>
    <div class="book-list" id="bookList">
      <div class="book-item">
        <img src="https://via.placeholder.com/150" alt="Book Image">
        <h3>Book Title A</h3>
        <p>Price: $10.00</p>
      </div>
      <div class="book-item">
        <img src="https://via.placeholder.com/150" alt="Book Image">
        <h3>Book Title B</h3>
        <p>Price: $15.00</p>
      </div>
    </div>
  </section>

  <!-- Contact Section -->
  <section id="contact">
    <h2>Contact Us</h2>
    <p>Email: contact@almanhalbookshop.com</p>
    <p>Phone: +123-456-7890</p>
  </section>

  <!-- Admin Section -->
  <section id="admin">
    <h2>Admin Panel</h2>
    <form onsubmit="return handleLogin()">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" placeholder="Enter username">
      
      <label for="password">Password:</label>
      <input type="password" id="password" name="password" placeholder="Enter password">
      
      <button type="submit">Login</button>
    </form>
  </section>
</main>

<script>
  const validUsername = "almanhal3";
  const validPassword = "al.bookshop25";

  function toggleSidebar() {
    const sidebar = document.getElementById("sidebar");
    sidebar.classList.toggle("collapsed");
  }

  function showSection(sectionId) {
    const sections = document.querySelectorAll('section');
    sections.forEach(section => {
      section.classList.remove('active');
    });

    const targetSection = document.getElementById(sectionId);
    targetSection.classList.add('active');
  }

  function handleLogin() {
    const username = document.getElementById("username").value;
    const password = document.getElementById("password").value;

    if (username === validUsername && password === validPassword) {
      alert("Login successful!");
      showAdminEditOptions();
    } else {
      alert("Invalid username or password.");
    }

    return false;
  }

  function showAdminEditOptions() {
    document.getElementById("books").innerHTML = `
      <h2>Edit Books</h2>
      <form onsubmit="return addBook(event)">
        <input type="file" id="bookImage" accept="image/*" required>
        <input type="text" id="bookTitle" placeholder="Book Title" required>
        <input type="text" id="bookPrice" placeholder="Price" required>
        <button type="submit">Add Book</button>
      </form>
      <div class="book-list" id="bookList"></div>
    `;
  }

  function addBook(event) {
    event.preventDefault();
    const bookList = document.getElementById("bookList");
    const bookImage = document.getElementById("bookImage").files[0];
    const bookTitle = document.getElementById("bookTitle").value;
    const bookPrice = document.getElementById("bookPrice").value;

    if (bookImage) {
      const reader = new FileReader();
      reader.onload = function(e) {
        const bookItem = document.createElement("div");
        bookItem.className = "book-item";
        bookItem.innerHTML = `
          <img src="${e.target.result}" alt="${bookTitle}">
          <h3>${bookTitle}</h3>
          <p>Price: ${bookPrice}</p>
        `;
        bookList.appendChild(bookItem);
      };
      reader.readAsDataURL(bookImage);
    }
  }
</script>

</body>
</html>
