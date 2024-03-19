Backend (Express)

1.Set up Express Project:
mkdir code-snippet-backend
cd code-snippet-backend
npm init -y
npm install express mysql2 body-parser

2.Create MySQL Database:
create a database named 'code_snippets'.
CREATE DATABASE code_snippets;
Then, create a table named snippets with the following schema:
CREATE TABLE snippets (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    code_language VARCHAR(50) NOT NULL,
    stdin TEXT,
    source_code TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

3.Implement REST API Endpoints:
Create a file named 'routes/snippets.js':
const express = require('express');
const router = express.Router();
const pool = require('../db');

// GET all snippets
router.get('/', (req, res) => {
    pool.query('SELECT * FROM snippets', (error, results) => {
        if (error) throw error;
        res.json(results);
    });
});

// POST a new snippet
router.post('/', (req, res) => {
    const { username, code_language, stdin, source_code } = req.body;
    pool.query('INSERT INTO snippets (username, code_language, stdin, source_code) VALUES (?, ?, ?, ?)', [username, code_language, stdin, source_code], (error, results) => {
        if (error) throw error;
        res.status(201).send('Snippet added successfully.');
    });
});

module.exports = router;

4.Connect Express with MySQL:
Create a file named 'db.js' to set up the database connection pool:
const mysql = require('mysql2');

const pool = mysql.createPool({
    connectionLimit: 10,
    host: 'localhost',
    user: 'your_mysql_username',
    password: 'your_mysql_password',
    database: 'code_snippets'
});

module.exports = pool.promise();

5.Implement Middleware:
Create a file named 'app.js' for the main Express application:
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const snippetsRouter = require('./routes/snippets');

const app = express();

app.use(cors());
app.use(bodyParser.json());

app.use('/snippets', snippetsRouter);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

6.Frontend (React)
Set up React Project:
npx create-react-app code-snippet-frontend
cd code-snippet-frontend
npm install axios react-router-dom

Design Page 1:
Modify src/App.js to create a form component for submitting code snippets.

Design Page 2:
Modify src/App.js to create a component to display submitted entries in a tabular format.

Fetch Data from Backend:
Implement Axios requests to fetch and display code snippets from the backend.

Deploy Frontend:
Deploy the React frontend to a hosting service like Netlify or Vercel.
