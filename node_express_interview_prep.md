# The Master Node.js & Express.js Interview Prep Guide
## Full-Stack Node, Express & Database Integration for Java/Python Developers

This guide is designed for backend developers with a Python/Java background preparing for Node.js and Express.js full-stack interviews. Below is a comprehensive glossary of Node & Express terms, followed by 20 high-frequency Q&As and 15 structured modules.

---

## 📂 Table of Contents
1. [Node & Express Concept Glossary (Core Runtime & APIs)](#node--express-concept-glossary-core-runtime--apis)
2. [20 High-Frequency Express + Node Questions & Answers](#20-high-frequency-express--node-questions--answers)
3. [Module 1: Express Fundamentals](#module-1-express-fundamentals)
4. [Module 2: Routing](#module-2-routing)
5. [Module 3: Middleware Pattern](#module-3-middleware-pattern)
6. [Module 4: Request & Response Objects](#module-4-request--response-objects)
7. [Module 5: REST API Development](#module-5-rest-api-development)
8. [Module 6: Error Handling](#module-6-error-handling)
9. [Module 7: Authentication & Authorization](#module-7-authentication--authorization)
10. [Module 8: Database Integration](#module-8-database-integration)
11. [Module 9: Async Programming in Node](#module-9-async-programming-in-node)
12. [Module 10: Security](#module-10-security)
13. [Module 11: File Uploads](#module-11-file-uploads)
14. [Module 12: Environment Configuration](#module-12-environment-configuration)
15. [Module 13: Logging](#module-13-logging)
16. [Module 14: Performance Optimization](#module-14-performance-optimization)
17. [Module 15: Production Deployment](#module-15-production-deployment)

---

## Node & Express Concept Glossary (Core Runtime & APIs)

Use this glossary to study basic, intermediate, and advanced backend terms often tested in Node and Express interviews:

*   **Event Loop:** The core mechanism in Node.js that orchestrates non-blocking, asynchronous execution. It schedules and processes callbacks through six distinct phases (Timers, Pending, Poll, Check, etc.).
*   **Libuv:** The underlying multi-platform C support library used by Node.js. It manages the Event Loop, asynchronous network requests, and provides a Thread Pool (default 4 threads) to handle heavy blocking actions (cryptography, file decompression).
*   **Blocking vs. Non-blocking I/O:**
    *   **Blocking:** Code execution halts until the requested operation (like reading a database or disk file) completes.
    *   **Non-blocking:** Code registers a callback and continues executing immediately. The runtime triggers the callback when the I/O finishes.
*   **Buffer:** A raw chunk of memory allocated outside the V8 heap. It holds binary data raw chunks before processing (essential for dealing with network sockets or file reading streams).
*   **Streams:** Data-handling structures that process files chunk-by-chunk. Types:
    *   `Readable`: Stream to read data from (e.g. `fs.createReadStream`).
    *   `Writable`: Stream to write data to (e.g. `fs.createWriteStream`).
    *   `Duplex`: Both readable and writable (e.g. TCP sockets).
    *   `Transform`: A duplex stream that alters data as it writes (e.g. `zlib` compression).
*   **Backpressure:** The bottleneck condition occurring in stream handling when a Readable stream reads data faster than the destination Writable stream can write it.
*   **CommonJS (CJS) vs. ES Modules (ESM):**
    *   **CommonJS:** Synchronous module loading used in legacy Node (`require`/`module.exports`).
    *   **ES Modules:** Modern, asynchronous language module loading standard (`import`/`export`).
*   **Middleware:** Functions executing inside Express's request-response cycle. They have access to the request object (`req`), response (`res`), and a `next()` callback to pass control.
*   **Express Router:** A mini, modular instance of an Express application. It groups routes together for cleaner, modular file architectures.
*   **JSON Web Token (JWT):** A stateless authentication token containing three Base64URL-encoded parts: Header (algorithm), Payload (user claims), and Signature (secret verification key).
*   **Session vs. Cookie Auth:**
    *   **Session:** Stateful authentication. The server keeps session details in a DB/Redis cache and tracks the client using a unique Session ID cookie.
    *   **JWT:** Stateless authentication. The token contains cryptographically verifiable user claims, stored by client and verified by server without DB calls.
*   **ORM vs. ODM:**
    *   **ORM (Object-Relational Mapper):** Maps code objects to relational SQL database tables (e.g., Sequelize for Postgres).
    *   **ODM (Object-Document Mapper):** Maps code objects to document-based NoSQL collections (e.g., Mongoose for MongoDB).
*   **Connection Pooling:** A cache of open, reusable database connections. It prevents the latency overhead of establishing a new connection on every single API query.
*   **Database Migration:** Incremental, version-controlled JS files containing database schema changes (creating tables, altering fields) so schemas stay uniform across environments.
*   **Clustering:** A technique using Node's `cluster` module to spawn duplicate processes (workers) sharing the same TCP port, scaling applications to utilize all CPU cores.
*   **Worker Threads:** Spawning auxiliary OS threads sharing memory inside the same Node process (using `worker_threads`) to calculate heavy CPU tasks without blocking the main Event Loop.
*   **Helmet:** A middleware that automatically sets HTTP response headers (like `X-Content-Type-Options`, `X-Frame-Options`) to guard against browser-based vulnerability exploits.
*   **CORS (Cross-Origin Resource Sharing):** A browser-enforced security check that blocks code running on one domain (e.g. React on port 3000) from querying a backend on another domain unless the backend explicitly permits it.
*   **Rate Limiting:** Restricting the quantity of HTTP queries a client IP can submit inside a time window (mitigates DDoS and brute-force attacks).
*   **Reverse Proxy:** A server placed in front of application instances (like Nginx) that handles SSL termination, static files caching, load balancing, and routing.
*   **Process Manager (PM2):** A production manager daemon that runs Node applications in the background, clusters them across CPU cores, and restarts processes automatically on failure or code updates with zero downtime.
*   **Multer:** Middleware for parsing incoming `multipart/form-data` request payloads, used to handle file uploads.
*   **Winston / Morgan:** Logging utilities: Winston handles multi-transport (file, database, console) structured logging; Morgan acts as an HTTP request logging formatter.
*   **Supertest:** A test client library that sends virtual HTTP requests to Express routers for unit/integration testing without binding to real server network ports.
*   **Arity:** The count of parameters a function expects. Express uses function arity to identify Error-Handling middleware (which must have exactly 4 arguments: `(err, req, res, next)`).
*   **`process.nextTick()`:** Schedules a callback to execute immediately after the current operation finishes, prioritizing it before the Event Loop enters its next phase.
*   **`setImmediate()`:** Schedules a callback to execute during the **Check phase** of the Event Loop, after the Poll phase completes.

---

## 20 High-Frequency Express + Node Questions & Answers

#### Q1: What is Express?
Express is a minimal, flexible, and unopinionated web application framework for Node.js. It provides a robust set of features for web and mobile applications, acting primarily as a routing and middleware wrapper around Node's core server components.

#### Q2: Why Express instead of the native http module?
Node's native `http` module is verbose. Implementing routing, parsing request bodies, setting status codes, and serving static files requires writing boilerplates of manual conditional statements. Express simplifies this by abstracts routing, headers, body parser bindings, and middleware architectures.

#### Q3: Explain middleware.
Middleware is a function that has access to the request object (`req`), response object (`res`), and the `next` function in the application's request-response cycle. It can execute custom code, mutate req/res properties, end the cycle, or call `next()` to invoke the next middleware.

#### Q4: How does `next()` work?
`next()` is a callback argument passed to middleware. When executed, it tells Express to pass execution control to the next sequential middleware function registered in the router matching pipeline.

#### Q5: What happens if `next()` isn't called?
If a middleware function does not call `next()` and does not send a response (like `res.send()`), the request-response cycle is left hanging. The client request will eventually time out.

#### Q6: What is Express Router?
Express Router is an isolated instance of middleware and routes. It is used to split route configurations into separate, modular files, organizing the application structure (similar to Spring Controllers).

#### Q7: Route parameters vs. query parameters.
*   **Route parameters (`req.params`):** Embedded in the URL path to identify specific resources (e.g. `/users/:id` -> `/users/123`).
*   **Query parameters (`req.query`):** Key-value pairs appended at the end of a URL after `?` to sort, filter, or paginate (e.g. `/users?limit=10`).

#### Q8: PUT vs. PATCH.
*   **PUT:** Replaces the entire target resource with the requested payload. Missing properties are overwritten to null/empty values.
*   **PATCH:** Performs a partial update to the resource, modifying only the properties specified in the payload.

#### Q9: How do you handle errors?
Errors are captured using try/catch blocks in routes and passed to error-handling middleware by calling `next(error)`. The error-handling middleware receives `(err, req, res, next)` as arguments and returns an appropriate HTTP status response.

#### Q10: How do you implement authentication?
By using stateless **JSON Web Tokens (JWT)**. Upon successful login, the server signs a token and returns it. For protected routes, an authentication middleware extracts the token from the `Authorization: Bearer <token>` header, verifies it, and attaches the user payload to `req.user`.

#### Q11: JWT vs. Sessions.
*   **Sessions:** State-based. A session ID is stored in a client cookie and validated against a database or cache store (e.g., Redis) on the server.
*   **JWT:** Stateless. The server signs user details in a token. The client sends it in the request header, and the server validates it cryptographically without database queries.

#### Q12: How do you secure an Express app?
1. Use `Helmet` to set secure HTTP headers.
2. Limit requests using `express-rate-limit`.
3. Configure `CORS` to restrict unauthorized origins.
4. Sanitize input to prevent SQL/NoSQL injections.
5. Store secrets inside `.env` configurations.

#### Q13: How do you validate request data?
Using validation middleware libraries like `express-validator` or schema parsers like `Joi` or `Zod` before the controller logic executes.

#### Q14: How do you upload files?
Using the `multer` middleware, which parses incoming `multipart/form-data` request bodies, extracts files, and stores them in memory buffers or writes them to disk.

#### Q15: How do you structure a large Express project?
By organizing code into a modular architecture:
*   `controllers/`: Handle HTTP logic.
*   `routes/`: Define API endpoints.
*   `middlewares/`: Request/Error interceptors.
*   `models/`: Schema definitions.
*   `config/`: Environment configurations.

#### Q16: How do you connect Express to a database?
Using database clients or ORMs/ODMs (Mongoose for MongoDB, Sequelize or Knex for PostgreSQL/MySQL). Establish the connection pool at startup and export the connection model instances to the controller modules.

#### Q17: How do you optimize Express performance?
1. Enable GZIP compression using `compression` middleware.
2. Cache database responses using Redis.
3. Cluster Node processes across CPU cores using PM2.
4. Use Streams to process large files.

#### Q18: What is CORS?
Cross-Origin Resource Sharing is a browser security mechanism that blocks frontends running on one origin (e.g., port 3000) from querying backend APIs running on a different origin (e.g., port 8080) unless the backend explicitly sends CORS headers authorization.

#### Q19: How do you manage environment variables?
Using the `dotenv` package to load configurations from a `.env` file into Node’s global object `process.env`.

#### Q20: How does Express handle asynchronous code?
Express executes route handlers sequentially. To prevent unhandled promise rejections, async handlers must wrap code in `try...catch` blocks and pass exceptions to `next(err)`. (Express 5 handles uncaught async errors natively).

---

## Module 1: Express Fundamentals

*   **What is Express?** A minimalist, routing and middleware web framework for Node.js.
*   **Application Lifecycle:** Incoming HTTP Request -> Express Router matching -> Middleware chains -> Controller handler logic -> Outgoing HTTP Response.

### Code Example: Creating a Basic Server
```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

// Built-in JSON parser middleware (similar to Spring's message converters)
app.use(express.json());

app.get('/health', (req, res) => {
  res.status(200).json({ status: "UP", timestamp: new Date() });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

## Module 2: Routing

Express routes match HTTP methods (GET, POST, PUT, DELETE) and paths.

### A. HTTP Methods & Dynamic Routes
```javascript
const express = require('express');
const router = express.Router();

// GET /api/v1/users/:id
router.get('/users/:id', (req, res) => {
  const userId = req.params.id; // Path parameter
  res.json({ id: userId, name: "Alice" });
});

// GET /api/v1/users?limit=10
router.get('/users', (req, res) => {
  const limit = req.query.limit || 5; // Query parameters
  res.json({ limit, data: [] });
});

// Wildcard fallback route
router.get('*', (req, res) => {
  res.status(404).json({ error: "Route not found" });
});

module.exports = router;
```

---

## Module 3: Middleware Pattern

Middleware functions intercept and process requests before they hit controllers.

### Middleware Execution Order & Spring Boot Comparison
*   *Spring Boot comparison:* Middleware is equivalent to **Servlet Filters** or **Spring Interceptors** (e.g., checking authentication, logging).

```javascript
// 1. Application-level: Runs for all routes
app.use((req, res, next) => {
  console.log(`${req.method} request to ${req.url}`);
  next(); // Must call next() to prevent request from hanging!
});

// 2. Route-level Auth middleware
const checkAdmin = (req, res, next) => {
  if (req.headers['x-role'] !== 'admin') {
    return res.status(403).json({ error: "Forbidden: Admin access required" });
  }
  next(); // User is admin, proceed
};

// Application matching:
app.get('/admin/dashboard', checkAdmin, (req, res) => {
  res.send("Welcome Admin");
});
```

---

## Module 4: Request & Response Objects

Understanding structural properties of Express parameters:

### A. Request Objects (`req`)
*   `req.params`: Object containing route parameters (e.g., `{ id: '123' }`).
*   `req.query`: Object containing query string parameters (e.g., `{ search: 'item' }`).
*   `req.body`: Parsed request body data payload.
*   `req.headers`: Request headers (like authorization or content-type).
*   `req.cookies`: User cookies (requires `cookie-parser`).

### B. Response Objects (`res`)
*   `res.status(code)`: Sets HTTP status code.
*   `res.send()`: Sends plain text, HTML, or raw buffer response.
*   `res.json()`: Automatically sends a JSON response with correct headers.
*   `res.redirect(url)`: Redirects client to another URL.
*   `res.download(path)`: Prompts the client to download a file from the server.

---

## Module 5: REST API Development

A complete CRUD controller demonstrating pagination, sorting, and standard HTTP statuses.

```javascript
const express = require('express');
const router = express.Router();

let products = [
  { id: 1, name: "Keyboard", price: 50 },
  { id: 2, name: "Mouse", price: 30 }
];

// GET /products?page=1&size=2&sortBy=price
router.get('/', (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const size = parseInt(req.query.size) || 10;
  const sortBy = req.query.sortBy || 'id';

  // Sort and Paginate
  const sorted = [...products].sort((a, b) => a[sortBy] > b[sortBy] ? 1 : -1);
  const startIndex = (page - 1) * size;
  const paginated = sorted.slice(startIndex, startIndex + size);

  res.status(200).json({
    page,
    size,
    total: products.length,
    data: paginated
  });
});

// POST /products
router.post('/', (req, res) => {
  if (!req.body.name || !req.body.price) {
    return res.status(400).json({ error: "Missing required fields" });
  }
  const newItem = { id: Date.now(), name: req.body.name, price: req.body.price };
  products.push(newItem);
  res.status(201).json(newItem); // 201 Created
});

// DELETE /products/:id
router.delete('/:id', (req, res) => {
  const id = parseInt(req.params.id);
  products = products.filter(p => p.id !== id);
  res.status(204).send(); // 204 No Content (standard for delete success)
});

module.exports = router;
```

---

## Module 6: Error Handling

### Synchronous vs. Asynchronous Error Catching
```javascript
const express = require('express');
const app = express();

// A. Synchronous errors: Express catches them automatically
app.get('/sync-err', (req, res) => {
  throw new Error("Synchronous failure!"); // Caught by error middleware
});

// B. Asynchronous errors: Must pass to next(err) in Express 4
app.get('/async-err', async (req, res, next) => {
  try {
    const data = await fetchFromDB();
  } catch (err) {
    next(err); // Pass async error to Express handler
  }
});

// C. Centralized Error Handler Middleware
app.use((err, req, res, next) => {
  const statusCode = err.status || 500;
  res.status(statusCode).json({
    error: {
      message: err.message || "Internal Server Error",
      status: statusCode
    }
  });
});
```

---

## Module 7: Authentication & Authorization

Securing Express routes using JSON Web Tokens (JWT) and Bcrypt.

```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const express = require('express');
const router = express.Router();
const SECRET = "JWT_SECRET_KEY";

const users = []; // In-memory database

// 1. Register User (Password Hashing)
router.post('/register', async (req, res, next) => {
  try {
    const hashedPassword = await bcrypt.hash(req.body.password, 10);
    const user = { username: req.body.username, password: hashedPassword };
    users.push(user);
    res.status(201).json({ message: "User registered successfully" });
  } catch (err) {
    next(err);
  }
});

// 2. Login User (Generate Token)
router.post('/login', async (req, res, next) => {
  const user = users.find(u => u.username === req.body.username);
  if (!user) return res.status(400).json({ error: "User not found" });

  try {
    if (await bcrypt.compare(req.body.password, user.password)) {
      const accessToken = jwt.sign({ username: user.username }, SECRET, { expiresIn: '15m' });
      res.json({ accessToken });
    } else {
      res.status(403).json({ error: "Invalid password" });
    }
  } catch (err) {
    next(err);
  }
});

module.exports = router;
```

---

## Module 8: Database Integration

### A. MongoDB Integration (ODM Mongoose)
```javascript
const mongoose = require('mongoose');

// Connection pooling is managed automatically by Mongoose
mongoose.connect('mongodb://localhost:27017/shopDb')
  .then(() => console.log('Connected to MongoDB'));

const ProductSchema = new mongoose.Schema({
  name: { type: String, required: true },
  price: Number
});

const Product = mongoose.model('Product', ProductSchema);
```

### B. Postgres Integration (ORM Sequelize with Transactions)
```javascript
const { Sequelize, DataTypes } = require('sequelize');
const sequelize = new Sequelize('postgres://user:pass@localhost:5432/db');

const User = sequelize.define('User', {
  balance: DataTypes.DECIMAL
});

// Transaction Example
async function transferFunds(fromId, toId, amount) {
  const transaction = await sequelize.transaction();
  try {
    const sender = await User.findByPk(fromId, { transaction });
    const receiver = await User.findByPk(toId, { transaction });

    sender.balance -= amount;
    receiver.balance += amount;

    await sender.save({ transaction });
    await receiver.save({ transaction });

    await transaction.commit(); // Save changes
  } catch (err) {
    await transaction.rollback(); // Undo changes on error
    throw err;
  }
}
```

---

## Module 9: Async Programming in Node

Understanding Promise chains, handling exceptions, and avoiding callback hell in Express.

```javascript
// Callback Hell (Legacy)
fs.readFile('f1.txt', (err, data1) => {
  fs.readFile('f2.txt', (err, data2) => {
    // Nested hell
  });
});

// Modern solution: Async/Await with cleaner code flow
const fs = require('fs').promises;

async function readFiles() {
  try {
    const [data1, data2] = await Promise.all([
      fs.readFile('f1.txt', 'utf8'),
      fs.readFile('f2.txt', 'utf8')
    ]);
    console.log(data1, data2);
  } catch (err) {
    console.error("Error reading files: ", err);
  }
}
```

---

## Module 10: Security

Mitigating common security vulnerabilities.

```javascript
const express = require('express');
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');
const cors = require('cors');
const app = express();

// 1. Set secure HTTP response headers
app.use(helmet());

// 2. Enable CORS with configurations
app.use(cors({ origin: 'http://localhost:3000' }));

// 3. Limit API queries (prevents DDoS/brute-force)
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per window
});
app.use('/api/', limiter);

// 4. Input Sanitization (Sanitize user inputs to prevent SQL/NoSQL injections)
app.post('/login', (req, res) => {
  const cleanUsername = String(req.body.username).replace(/[$/{}]/g, ""); // NoSQL Injection filter
  // Proceed with safe query...
});
```

---

## Module 11: File Uploads

File uploads are handled in Express using the `multer` middleware.

```javascript
const express = require('express');
const multer = require('multer');
const path = require('path');
const app = express();

// 1. Configure Multer Storage Engine
const storage = multer.diskStorage({
  destination: './uploads/',
  filename: (req, file, cb) => {
    cb(null, `${file.fieldname}-${Date.now()}${path.extname(file.originalname)}`);
  }
});

// 2. Define Upload Middleware with validations
const upload = multer({
  storage: storage,
  limits: { fileSize: 1000000 }, // 1MB limit
  fileFilter: (req, file, cb) => {
    const filetypes = /jpeg|jpg|png/;
    const extname = filetypes.test(path.extname(file.originalname).toLowerCase());
    if (extname) return cb(null, true);
    cb(new Error("Only images (jpeg/jpg/png) are allowed!"));
  }
});

// 3. Route Handler for single file upload
app.post('/upload', upload.single('avatar'), (req, res) => {
  if (!req.file) return res.status(400).send("No file uploaded");
  res.json({ message: "File uploaded successfully!", path: req.file.path });
});
```

---

## Module 12: Environment Configuration

Use `dotenv` to load configurations from a `.env` file.

```javascript
// 1. Load config as early as possible in entry script
require('dotenv').config();

const express = require('express');
const app = express();

// Access variables
const dbUrl = process.env.DATABASE_URL;
const isProd = process.env.NODE_ENV === 'production';

console.log(`Database connected to: ${dbUrl} (Production: ${isProd})`);
```

---

## Module 13: Logging

Use `winston` for structured production logging and `morgan` for developer console outputs.

```javascript
const express = require('express');
const morgan = require('morgan');
const winston = require('winston');
const app = express();

// Winston Configuration
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});

// If not in production, log to console as well
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

app.use(morgan('combined')); // HTTP log analyzer

app.get('/', (req, res) => {
  logger.info("Accessing index endpoint");
  res.send("Logged!");
});
```

---

## Module 14: Performance Optimization

1.  **GZIP Compression:** Use `compression` middleware to reduce bundle sizes sent to browser.
```javascript
const compression = require('compression');
app.use(compression());
```
2.  **Streaming Files:** Stream large file transfers instead of loading them into RAM.
3.  **Connection Pooling:** Verify your Postgres/Sequelize connections leverage connection pooling (default in Sequelize) to prevent connection allocation bottlenecks.

---

## Module 15: Production Deployment

### Process Management using PM2
Deploying in production requires running your application as a background daemon process. PM2 handles clustering, keeps the process alive, and scales it across CPU cores.

```bash
# Install PM2 globally
npm install pm2 -g

# Start app in cluster mode using all CPU cores
pm2 start server.js -i max

# Verify status
pm2 status
```

### Health Check Endpoint
A load balancer (like AWS ALB or Nginx) uses a health check endpoint to verify that the app instance is healthy before routing user traffic to it.

```javascript
app.get('/health', async (req, res) => {
  try {
    // Perform database connection health checks
    await db.authenticate();
    res.status(200).send("OK");
  } catch (err) {
    res.status(503).send("Database offline"); // 503 Service Unavailable
  }
});
```
