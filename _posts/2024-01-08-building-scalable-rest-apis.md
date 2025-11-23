---
layout: default
title: "Building Scalable REST APIs with Node.js"
date: 2024-01-08
categories: [Node.js, REST API, Backend Development]
author: Hahahaki
---

# Building Scalable REST APIs with Node.js

REST APIs are the backbone of modern web applications, enabling communication between frontend and backend systems. In this guide, we'll explore best practices for building scalable, maintainable REST APIs using Node.js and Express.

## Project Setup

First, let's set up a new Node.js project:

```bash
mkdir my-api
cd my-api
npm init -y
npm install express dotenv cors helmet morgan
npm install --save-dev nodemon
```

## Project Structure

A well-organized project structure is crucial for maintainability:

```
my-api/
├── src/
│   ├── config/
│   │   └── database.js
│   ├── controllers/
│   │   └── userController.js
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── utils/
│   │   └── logger.js
│   └── app.js
├── .env
├── .gitignore
└── package.json
```

## Basic Express Setup

Create `src/app.js`:

```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

const app = express();

// Middleware
app.use(helmet()); // Security headers
app.use(cors()); // Enable CORS
app.use(morgan('combined')); // Logging
app.use(express.json()); // Parse JSON bodies
app.use(express.urlencoded({ extended: true }));

// Routes
app.get('/health', (req, res) => {
  res.status(200).json({ status: 'OK', timestamp: new Date() });
});

// Error handling
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({
    error: 'Internal Server Error',
    message: process.env.NODE_ENV === 'development' ? err.message : undefined
  });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

module.exports = app;
```

## RESTful Routing

Implement proper RESTful routes:

```javascript
// src/routes/userRoutes.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

// GET /api/users - Get all users
router.get('/', userController.getAllUsers);

// GET /api/users/:id - Get single user
router.get('/:id', userController.getUserById);

// POST /api/users - Create new user
router.post('/', userController.createUser);

// PUT /api/users/:id - Update user
router.put('/:id', userController.updateUser);

// DELETE /api/users/:id - Delete user
router.delete('/:id', userController.deleteUser);

module.exports = router;
```

## Controller Pattern

Keep your routes clean by using controllers:

```javascript
// src/controllers/userController.js
const User = require('../models/User');

exports.getAllUsers = async (req, res, next) => {
  try {
    const { page = 1, limit = 10 } = req.query;
    const users = await User.find()
      .limit(limit * 1)
      .skip((page - 1) * limit)
      .exec();

    const count = await User.countDocuments();

    res.json({
      users,
      totalPages: Math.ceil(count / limit),
      currentPage: page
    });
  } catch (error) {
    next(error);
  }
};

exports.getUserById = async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.json(user);
  } catch (error) {
    next(error);
  }
};

exports.createUser = async (req, res, next) => {
  try {
    const user = new User(req.body);
    await user.save();
    res.status(201).json(user);
  } catch (error) {
    if (error.name === 'ValidationError') {
      return res.status(400).json({ error: error.message });
    }
    next(error);
  }
};

exports.updateUser = async (req, res, next) => {
  try {
    const user = await User.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true, runValidators: true }
    );
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.json(user);
  } catch (error) {
    next(error);
  }
};

exports.deleteUser = async (req, res, next) => {
  try {
    const user = await User.findByIdAndDelete(req.params.id);
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.status(204).send();
  } catch (error) {
    next(error);
  }
};
```

## Input Validation

Always validate input data:

```javascript
const { body, validationResult } = require('express-validator');

const validateUser = [
  body('email').isEmail().normalizeEmail(),
  body('name').trim().isLength({ min: 1 }),
  body('age').optional().isInt({ min: 0, max: 120 }),

  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    next();
  }
];

router.post('/', validateUser, userController.createUser);
```

## Authentication Middleware

Implement JWT authentication:

```javascript
// src/middleware/auth.js
const jwt = require('jsonwebtoken');

const authMiddleware = (req, res, next) => {
  const token = req.header('Authorization')?.replace('Bearer ', '');

  if (!token) {
    return res.status(401).json({ error: 'Authentication required' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

module.exports = authMiddleware;
```

## Error Handling

Centralized error handling:

```javascript
// src/middleware/errorHandler.js
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

const errorHandler = (err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';

  if (process.env.NODE_ENV === 'development') {
    res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack
    });
  } else {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.isOperational ? err.message : 'Something went wrong'
    });
  }
};

module.exports = { AppError, errorHandler };
```

## Rate Limiting

Protect your API from abuse:

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP, please try again later.'
});

app.use('/api/', limiter);
```

## API Versioning

Support multiple API versions:

```javascript
// v1
app.use('/api/v1/users', require('./routes/v1/userRoutes'));

// v2 (with breaking changes)
app.use('/api/v2/users', require('./routes/v2/userRoutes'));
```

## Best Practices

1. **Use Environment Variables**: Store sensitive data in `.env` files
2. **Implement Logging**: Use Winston or Morgan for comprehensive logging
3. **Add Pagination**: Always paginate list endpoints
4. **Use HTTP Status Codes Correctly**:
   - 200: OK
   - 201: Created
   - 204: No Content
   - 400: Bad Request
   - 401: Unauthorized
   - 404: Not Found
   - 500: Internal Server Error

5. **Implement Caching**: Use Redis for frequently accessed data
6. **Document Your API**: Use Swagger/OpenAPI for documentation
7. **Write Tests**: Implement unit and integration tests
8. **Monitor Performance**: Use tools like PM2, New Relic, or DataDog

## Testing

Example using Jest and Supertest:

```javascript
const request = require('supertest');
const app = require('../src/app');

describe('User API', () => {
  test('GET /api/users returns users list', async () => {
    const response = await request(app)
      .get('/api/users')
      .expect(200);

    expect(response.body).toHaveProperty('users');
    expect(Array.isArray(response.body.users)).toBe(true);
  });

  test('POST /api/users creates new user', async () => {
    const newUser = {
      name: 'Test User',
      email: 'test@example.com'
    };

    const response = await request(app)
      .post('/api/users')
      .send(newUser)
      .expect(201);

    expect(response.body).toHaveProperty('id');
    expect(response.body.name).toBe(newUser.name);
  });
});
```

## Conclusion

Building scalable REST APIs requires careful planning, proper structure, and adherence to best practices. By following these patterns, you'll create APIs that are maintainable, secure, and performant.

## Resources

- [Express.js Documentation](https://expressjs.com/)
- [REST API Design Best Practices](https://restfulapi.net/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

---

*Questions or suggestions? [Get in touch](../../contact.html)!*

[← Back to Blog](../../blog.html) | [Home](../../index.html)
