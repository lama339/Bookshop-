<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Almanhal Bookshop</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    /* Global Styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      display: flex;
      font-size: 16px;
    }

    /* Sidebar Styles */
    .sidebar {
      width: 250px;
      background-color: #2c3e50;
      height: 100vh;
      position: fixed;
      left: 0;
      top: 0;
      color: white;
      display: flex;
      flex-direction: column;
      padding: 20px;
    }

    .sidebar nav {
      margin-top: 20px;
    }

    .sidebar a {
      display: block;
      padding: 10px;
      text-decoration: none;
      color: white;
      transition: 0.3s;
    }

    .sidebar a:hover {
      background-color: #34495e;
    }

    button {
      background: none;
      border: none;
      color: white;
      font-size: 1.5rem;
      cursor: pointer;
      margin-bottom: 10px;
    }

    /* Content Section */
    main {
      margin-left: 270px;
      padding: 20px;
      display: none;
    }

    section {
      display: none;
    }

    section.active {
      display: block;
    }

    /* Admin Section Styles */
    form {
      display: flex;
      flex-direction: column;
      width: 300px;
    }

    input[type="text"],
    input[type="password"] {
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

    button[type="submit"]:hover {
      background-color: #2ecc71;
    }

    /* Responsive Design */
    @media screen and (max-width: 768px) {
      .sidebar {
        width: 100px;
      }

      .sidebar a {
        text-align: center;
        font-size: 12px;
      }

      main {
        margin-left: 110px;
      }
    }
  </style>
</head>
<body>

<!-- Sidebar -->
<div class="sidebar">
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
    <div id="book-list">
      <div>
        <p>1. Book Title A - $10.00</p>
        <p>2. Book Title B - $15.00</p>
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

<!-- JavaScript -->
<script>
  const validUsername = "almanhal3";
  const validPassword = "al.bookshop25";

  function toggleSidebar() {
    const menu = document.getElementById("menu");
    if (menu.style.display === "block") {
      menu.style.display = "none";
    } else {
      menu.style.display = "block";
    }
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

    return false; // Prevent form submission
  }

  function showAdminEditOptions() {
    document.getElementById("home").innerHTML = `
      <h2>Edit Home Page</h2>
      <textarea rows="5" cols="40">Welcome to Almanhal Bookshop</textarea>
      <button onclick="saveChanges()">Save</button>
    `;

    document.getElementById("books").innerHTML = `
      <h2>Edit Books</h2>
      <textarea rows="5" cols="40">1. Book Title A - $10.00\n2. Book Title B - $15.00</textarea>
      <button onclick="saveChanges()">Save</button>
    `;

    document.getElementById("contact").innerHTML = `
      <h2>Edit Contact Page</h2>
      <textarea rows="5" cols="40">Email: contact@almanhalbookshop.com\nPhone: +123-456-7890</textarea>
      <button onclick="saveChanges()">Save</button>
    `;
  }

  function saveChanges() {
    alert("Changes saved successfully!");
  }
</script>

</body>
</html>
