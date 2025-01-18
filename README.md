const express = require('express');
const bodyParser = require('body-parser');
const fs = require('fs');
const path = require('path');

const app = express();

// Load data from JSON file
const dataFilePath = path.join(__dirname, 'data.json');
let data = { books: [], admin: { username: 'admin', password: 'password123' } };

// Load or initialize data
if (fs.existsSync(dataFilePath)) {
    data = JSON.parse(fs.readFileSync(dataFilePath, 'utf-8'));
} else {
    fs.writeFileSync(dataFilePath, JSON.stringify(data, null, 2));
}

// Middleware
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'public')));
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, 'views'));

// Routes
app.get('/', (req, res) => {
    res.send(`
        <!DOCTYPE html>
        <html>
        <head>
            <title>Bookshop</title>
            <style>
                body { font-family: Arial; margin: 0; padding: 0; background: #f4f4f4; }
                header { background: darkgreen; color: white; padding: 10px; text-align: center; }
                nav a { margin: 0 15px; color: white; text-decoration: none; }
                footer { background: darkgreen; color: white; text-align: center; padding: 10px; margin-top: 20px; }
                .content { padding: 20px; text-align: center; }
            </style>
        </head>
        <body>
            <header>
                <h1>Bookshop</h1>
                <nav>
                    <a href="/">Home</a>
                    <a href="/books">Books</a>
                    <a href="/contact">Contact</a>
                    <a href="/admin">Admin</a>
                </nav>
            </header>
            <div class="content">
                <h2>Welcome to Our Bookshop</h2>
                <p>Your one-stop destination for amazing books!</p>
            </div>
            <footer>© 2025 Bookshop. All Rights Reserved.</footer>
        </body>
        </html>
    `);
});

app.get('/books', (req, res) => {
    const bookHtml = data.books
        .map(
            (book) => `
        <div style="border: 1px solid darkgreen; margin: 20px; padding: 10px; max-width: 300px;">
            <img src="${book.image}" alt="${book.title}" style="max-width: 100%; height: auto;" />
            <h3>${book.title}</h3>
            <p>Price: ${book.price}</p>
        </div>`
        )
        .join('');
    res.send(`
        <html>
        <head><title>Books</title></head>
        <body>
            <h1>Books</h1>
            ${bookHtml}
            <footer>© 2025 Bookshop. All Rights Reserved.</footer>
        </body>
        </html>
    `);
});

app.get('/contact', (req, res) => {
    res.send(`
        <html>
        <head><title>Contact Us</title></head>
        <body>
            <h1>Contact Us</h1>
            <p>Email: info@bookshop.com</p>
            <p>Phone: +123 456 7890</p>
            <footer>© 2025 Bookshop. All Rights Reserved.</footer>
        </body>
        </html>
    `);
});

app.get('/admin', (req, res) => {
    res.send(`
        <html>
        <head>
            <title>Admin Panel</title>
            <script>
                async function submitForm(event, endpoint) {
                    event.preventDefault();
                    const formData = new FormData(event.target);
                    const data = Object.fromEntries(formData.entries());
                    const response = await fetch(endpoint, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(data)
                    });
                    const result = await response.json();
                    alert(result.message);
                    if (result.success) window.location.reload();
                }
                async function saveChanges() {
                    const response = await fetch('/admin/save', { method: 'POST' });
                    const result = await response.json();
                    alert(result.message);
                }
            </script>
        </head>
        <body>
            <h1>Admin Panel</h1>
            <form onsubmit="submitForm(event, '/admin/login')">
                <input type="text" name="username" placeholder="Username" required />
                <input type="password" name="password" placeholder="Password" required />
                <button type="submit">Login</button>
            </form>
            <form onsubmit="submitForm(event, '/admin/add-book')">
                <input type="text" name="title" placeholder="Book Title" required />
                <input type="text" name="image" placeholder="Image URL" required />
                <input type="text" name="price" placeholder="Price" required />
                <button type="submit">Add Book</button>
            </form>
            <form onsubmit="submitForm(event, '/admin/delete-book')">
                <input type="text" name="title" placeholder="Book Title" required />
                <button type="submit">Delete Book</button>
            </form>
            <button onclick="saveChanges()">Save Changes</button>
        </body>
        </html>
    `);
});

// API Endpoints
app.post('/admin/login', (req, res) => {
    const { username, password } = req.body;
    if (username === data.admin.username && password === data.admin.password) {
        res.json({ success: true, message: 'Login successful' });
    } else {
        res.json({ success: false, message: 'Invalid credentials' });
    }
});

app.post('/admin/add-book', (req, res) => {
    const { title, image, price } = req.body;
    data.books.push({ title, image, price });
    res.json({ success: true, message: 'Book added successfully' });
});

app.post('/admin/delete-book', (req, res) => {
    const { title } = req.body;
    data.books = data.books.filter((book) => book.title !== title);
    res.json({ success: true, message: 'Book deleted successfully' });
});

app.post('/admin/save', (req, res) => {
    fs.writeFileSync(dataFilePath, JSON.stringify(data, null, 2));
    res.json({ success: true, message: 'Changes saved successfully' });
});

// Start the server
const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Server is running at http://localhost:${PORT}`);
});
