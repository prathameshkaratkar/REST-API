Sure, here's a README.md file with 30 practical-oriented questions and answers about building a REST API with Node.js, Express.js, and MongoDB:

```markdown
# REST API with Node.js, Express.js, and MongoDB

## Practical Questions and Answers

1. **How do you create a new Express.js application from scratch?**
   To create a new Express.js application from scratch, you can follow these steps:
   1. Create a new directory for your project: `mkdir my-app`
   2. Navigate to the directory: `cd my-app`
   3. Initialize a new Node.js project: `npm init -y`
   4. Install Express.js: `npm install express`
   5. Create an `app.js` file and add the following code:
      ```javascript
      const express = require('express');
      const app = express();
      const port = 3000;

      app.get('/', (req, res) => {
        res.send('Hello, World!');
      });

      app.listen(port, () => {
        console.log(`Server is running on port ${port}`);
      });
      ```
   6. Run the application: `node app.js`
   7. Visit `http://localhost:3000` in your web browser to see the "Hello, World!" message.

2. **How do you define a route to handle GET requests for retrieving all documents from a MongoDB collection?**
   To define a route that handles GET requests for retrieving all documents from a MongoDB collection, you can use the following code:
   ```javascript
   const mongoose = require('mongoose');
   const Book = require('./models/book'); // Assuming you have a Book model

   // Connect to MongoDB
   mongoose.connect('mongodb://localhost/mydatabase', {
     useNewUrlParser: true,
     useUnifiedTopology: true
   });

   app.get('/books', async (req, res) => {
     try {
       const books = await Book.find();
       res.json(books);
     } catch (err) {
       res.status(500).json({ message: err.message });
     }
   });
   ```

3. **How do you define a schema and model for a MongoDB collection using Mongoose?**
   To define a schema and model for a MongoDB collection using Mongoose, you can follow these steps:
   1. Create a new file, e.g., `models/book.js`
   2. Define the schema:
      ```javascript
      const mongoose = require('mongoose');

      const bookSchema = new mongoose.Schema({
        title: { type: String, required: true },
        author: { type: String, required: true },
        published: { type: Date },
        pages: { type: Number },
        genre: { type: String, enum: ['Fiction', 'Non-Fiction'] }
      });
      ```
   3. Create the model:
      ```javascript
      const Book = mongoose.model('Book', bookSchema);
      module.exports = Book;
      ```

4. **How do you create a new document in a MongoDB collection using Mongoose?**
   To create a new document in a MongoDB collection using Mongoose, you can use the following code:
   ```javascript
   const Book = require('./models/book');

   const newBook = new Book({
     title: 'The Great Gatsby',
     author: 'F. Scott Fitzgerald',
     published: new Date('1925-04-10'),
     pages: 180,
     genre: 'Fiction'
   });

   newBook.save()
     .then(() => console.log('New book created'))
     .catch(err => console.error(err));
   ```

5. **How do you define a route to handle POST requests for creating a new document in a MongoDB collection?**
   To define a route that handles POST requests for creating a new document in a MongoDB collection, you can use the following code:
   ```javascript
   const Book = require('./models/book');

   app.post('/books', async (req, res) => {
     const book = new Book({
       title: req.body.title,
       author: req.body.author,
       published: req.body.published,
       pages: req.body.pages,
       genre: req.body.genre
     });

     try {
       const newBook = await book.save();
       res.status(201).json(newBook);
     } catch (err) {
       res.status(400).json({ message: err.message });
     }
   });
   ```
   Note: You'll need to use middleware like `express.json()` or `body-parser` to parse the request body.

6. **How do you define a route to handle PUT requests for updating an existing document in a MongoDB collection?**
   To define a route that handles PUT requests for updating an existing document in a MongoDB collection, you can use the following code:
   ```javascript
   const Book = require('./models/book');

   app.put('/books/:id', async (req, res) => {
     try {
       const book = await Book.findByIdAndUpdate(req.params.id, req.body, { new: true });
       if (!book) {
         return res.status(404).json({ message: 'Book not found' });
       }
       res.json(book);
     } catch (err) {
       res.status(400).json({ message: err.message });
     }
   });
   ```
   This code uses the `findByIdAndUpdate` method from Mongoose to find the document by its `id` and update it with the new data from the request body.

7. **How do you define a route to handle DELETE requests for removing a document from a MongoDB collection?**
   To define a route that handles DELETE requests for removing a document from a MongoDB collection, you can use the following code:
   ```javascript
   const Book = require('./models/book');

   app.delete('/books/:id', async (req, res) => {
     try {
       const book = await Book.findByIdAndDelete(req.params.id);
       if (!book) {
         return res.status(404).json({ message: 'Book not found' });
       }
       res.json({ message: 'Book deleted' });
     } catch (err) {
       res.status(500).json({ message: err.message });
     }
   });
   ```
   This code uses the `findByIdAndDelete` method from Mongoose to find the document by its `id` and remove it from the collection.

8. **How do you implement middleware in an Express.js application?**
   To implement middleware in an Express.js application, you can use the following code:
   ```javascript
   const logRequest = (req, res, next) => {
     console.log(`${req.method} ${req.path}`);
     next();
   };

   app.use(logRequest); // Apply middleware to all routes

   // Or apply middleware to specific routes
   app.get('/books', logRequest, (req, res) => {
     // Route handler
   });
   ```
   In this example, the `logRequest` function is a custom middleware that logs the HTTP method and path of each incoming request. The `app.use(logRequest)` line applies the middleware to all routes, while the second example shows how to apply the middleware to a specific route.

9. **How do you implement error handling in an Express.js application?**
   To implement error handling in an Express.js application, you can use the following code:
   ```javascript
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ message: 'Something went wrong' });
   });
   ```
   This code defines an error-handling middleware function that catches all errors in the application. It logs the error stack trace to the console and sends a JSON response with a 500 status code and an error message.

10. **How do you implement CORS (Cross-Origin Resource Sharing) in an Express.js application?**
    To implement CORS in an Express.js application, you can use the `cors` middleware package:
    1. Install the package: `npm install cors`
    2. Import and use the middleware in your application:
       ```javascript
       const cors = require('cors');

       app.use(cors()); // Allow all origins
       // or
       app.use(cors({
         origin: 'http://example.com' // Allow specific origin
       }));
       ```
    The `cors` middleware automatically handles CORS requests based on the provided configuration options.

Sure, here are the remaining questions and answers:

12. **How do you implement authentication with JWT (JSON Web Tokens) in an Express.js application?**
    To implement authentication with JWT in an Express.js application, you can follow these steps:
    1. Install the required packages: `npm install jsonwebtoken bcryptjs`
    2. Create a route for user login/authentication:
       ```javascript
       const bcrypt = require('bcryptjs');
       const jwt = require('jsonwebtoken');

       app.post('/login', async (req, res) => {
         const user = await User.findOne({ email: req.body.email });
         if (!user) {
           return res.status(400).json({ message: 'Invalid email or password' });
         }

         const isMatch = await bcrypt.compare(req.body.password, user.password);
         if (!isMatch) {
           return res.status(400).json({ message: 'Invalid email or password' });
         }

         const token = jwt.sign({ userId: user._id }, 'your_secret_key', { expiresIn: '1h' });
         res.json({ token });
       });
       ```
    3. Protect routes by adding a middleware to verify the JWT token:
       ```javascript
       const verifyToken = (req, res, next) => {
         const token = req.headers.authorization?.split(' ')[1];
         if (!token) {
           return res.status(401).json({ message: 'No token provided' });
         }

         jwt.verify(token, 'your_secret_key', (err, decoded) => {
           if (err) {
             return res.status(403).json({ message: 'Failed to authenticate token' });
           }

           req.userId = decoded.userId;
           next();
         });
       };

       app.get('/protected', verifyToken, (req, res) => {
         // Protected route handler
         res.json({ message: 'Access granted' });
       });
       ```

13. **How do you implement data validation in an Express.js application?**
    To implement data validation in an Express.js application, you can use the `express-validator` package:
    1. Install the package: `npm install express-validator`
    2. Import and use the validation functions in your routes:
       ```javascript
       const { body, validationResult } = require('express-validator');

       app.post('/books', [
         body('title').notEmpty().withMessage('Title is required'),
         body('author').notEmpty().withMessage('Author is required'),
         // Add more validation rules as needed
       ], async (req, res) => {
         const errors = validationResult(req);
         if (!errors.isEmpty()) {
           return res.status(400).json({ errors: errors.array() });
         }

         // If validation passes, continue with creating a new book
         const book = new Book(req.body);
         // ...
       });
       ```

14. **How do you handle file uploads in an Express.js application?**
    To handle file uploads in an Express.js application, you can use the `multer` middleware package:
    1. Install the package: `npm install multer`
    2. Set up multer and define the file upload route:
       ```javascript
       const multer = require('multer');
       const upload = multer({ dest: 'uploads/' });

       app.post('/upload', upload.single('file'), (req, res) => {
         if (!req.file) {
           return res.status(400).json({ message: 'No file uploaded' });
         }

         res.json({ message: 'File uploaded successfully', file: req.file });
       });
       ```
    In this example, `upload.single('file')` is a middleware function that expects a file field named `file` in the request. The uploaded files are stored in the `uploads/` directory.

15. **How do you implement environment variables in a Node.js application?**
    To implement environment variables in a Node.js application, you can use the `dotenv` package:
    1. Install the package: `npm install dotenv`
    2. Create a `.env` file in your project root and add your environment variables:
       ```
       PORT=3000
       MONGODB_URI=mongodb://localhost/mydatabase
       JWT_SECRET=your_secret_key
       ```
    3. In your application code, load the environment variables:
       ```javascript
       const dotenv = require('dotenv');
       dotenv.config();

       const port = process.env.PORT || 3000;
       const mongodbUri = process.env.MONGODB_URI;
       const jwtSecret = process.env.JWT_SECRET;
       ```

16. **How do you handle asynchronous operations in Node.js using Promises?**
    In Node.js, you can handle asynchronous operations using Promises:
    ```javascript
    const fetchData = () => {
      return new Promise((resolve, reject) => {
        // Simulating an asynchronous operation
        setTimeout(() => {
          const data = { message: 'Success!' };
          resolve(data);
        }, 2000);
      });
    };

    fetchData()
      .then(data => console.log(data))
      .catch(err => console.error(err));
    ```
    In this example, `fetchData` returns a Promise that resolves after 2 seconds with some data. The `.then` method is used to handle the resolved Promise, and the `.catch` method is used to handle any errors.

17. **How do you handle asynchronous operations in Node.js using async/await?**
    In Node.js, you can handle asynchronous operations using the `async/await` syntax:
    ```javascript
    const fetchData = () => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          const data = { message: 'Success!' };
          resolve(data);
        }, 2000);
      });
    };

    async function getData() {
      try {
        const data = await fetchData();
        console.log(data);
      } catch (err) {
        console.error(err);
      }
    }

    getData();
    ```
    In this example, `fetchData` returns a Promise as before. The `getData` function is marked as `async`, which allows the use of `await` to wait for the Promise to resolve. The `try/catch` block is used to handle any errors.

18. **How do you connect to a MongoDB database in Node.js using Mongoose?**
    To connect to a MongoDB database in Node.js using Mongoose, you can use the following code:
    ```javascript
    const mongoose = require('mongoose');

    mongoose.connect('mongodb://localhost/mydatabase', {
      useNewUrlParser: true,
      useUnifiedTopology: true
    })
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB', err));
    ```
    This code connects to a MongoDB database named `mydatabase` running on `localhost`. The `useNewUrlParser` and `useUnifiedTopology` options are required to prevent deprecation warnings.

19. **How do you query data in Mongoose using query filters and options?**
    In Mongoose, you can query data using various query filters and options:
    ```javascript
    const Book = require('./models/book');

    // Find books with a specific title
    Book.find({ title: 'The Great Gatsby' })
      .then(books => console.log(books))
      .catch(err => console.error(err));

    // Find books published after a certain date and sort by author
    Book.find({ published: { $gt: new Date('2000-01-01') } })
      .sort({ author: 1 }) // 1 for ascending, -1 for descending
      .limit(10)
      .then(books => console.log(books))
      .catch(err => console.error(err));
    ```
    In this example, the first query finds books with the title "The Great Gatsby". The second query finds books published after January 1, 2000, sorts them by author in ascending order, and limits the results to 10 documents.

20. **How do you update documents in a MongoDB collection using Mongoose?**
    To update documents in a MongoDB collection using Mongoose, you can use the following methods:
    ```javascript
    const Book = require('./models/book');

    // Update a single document
    Book.findByIdAndUpdate