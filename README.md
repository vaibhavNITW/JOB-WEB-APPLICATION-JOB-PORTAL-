# 💼 Job Portal

A full-stack Job Portal that bridges the gap between **recruiters** and **freshers**. It allows recruiters to create jobs and job seekers to find and apply for them easily.

[![GitHub Repo](https://img.shields.io/badge/GitHub-Balraj1802-blue?logo=github)](https://github.com/balraj1802/Job-Portal)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

---

## 📦 Backend Dependencies

### 1. `bcryptjs`
- **Version:** ^3.0.2  
- **Purpose:** For hashing passwords.
- **Usage:** Encrypts user passwords before storing in the database.
- **Example:**
```js
const bcrypt = require("bcryptjs");
const hash = await bcrypt.hash("password123", 10);
const isValid = await bcrypt.compare("password123", hash);
```
 ### 2 cookie-parser
- **Version:**: ^1.4.7

- **Purpose:**: To parse cookies attached to the client request.

- **Usage:**:
- **Example:**
```js
const cookieParser = require("cookie-parser");
app.use(cookieParser());

app.get("/profile", (req, res) => {
  const token = req.cookies.token;
  res.send(token);
});
```
### 3 🌍 cors
- **Version:**: ^2.8.5

- **Purpose:**: Enables cross-origin requests between frontend and backend.

- **Usage:**'
- **Example:**
```js
const cors = require("cors");
app.use(cors({
  origin: "http://localhost:5173",
  credentials: true,
}));
```
### 4 🛠 dotenv
- **Version:**: ^17.0.1

- **Purpose:**: Loads environment variables from a .env file.

- **Usage:**
- **Example:**
// .env file
PORT=5000
MONGO_URL=mongodb+srv://...
JWT_SECRET=your_jwt_secret
```js
require("dotenv").config();
const port = process.env.PORT;
```
### 5 🚀 express
- **Version:**: ^5.1.0

- **Purpose:**: Web framework to create routes, handle requests and build APIs.

- **Usage:**
- **Example:**
```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from TodoApp backend!");
});

app.listen(5000, () => console.log("Server is running on port 5000"));
```
### 6 🔐 jsonwebtoken
- **Version:**: ^9.0.2

- **Purpose:**: To generate and verify JWT tokens for authentication.

- **Usage:**
- **Example:**
```js
const jwt = require("jsonwebtoken");
const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, {
  expiresIn: "1h",
});
const decoded = jwt.verify(token, process.env.JWT_SECRET);
```
### 7 🧬 mongoose
- **Version:**: ^8.16.1

- **Purpose:**: MongoDB ODM (Object Data Modeling) tool to define schemas and models.
- **Usage:**
- **Example:**
```js
const mongoose = require("mongoose");

mongoose.connect(process.env.MONGO_URL, {
  useNewUrlParser: true,
});

const UserSchema = new mongoose.Schema({
  name: String,
  email: String,
});

const User = mongoose.model("User", UserSchema);
```
### 8 🔁 nodemon
- **Version:**: ^3.1.10

- **Purpose:**: Automatically restarts the Node server when files change.
- **Usage:**
- **Example:**
```js
// In package.json
"scripts": {
  "dev": "nodemon app.js"
}
```
## middleware -> isAuthenticated.js

# 📌 Example Usage Flow:
- You login → server gives you a JWT token.
- That token is saved in cookies.
- On any protected route:
- This middleware runs.
- It verifies the token.
- Extracts the user ID.
- Saves it to req.id.
- Then your controller can use req.id freely.

```js
import jwt from "jsonwebtoken";

const isAuthenticated = async (req, res, next) => {
    try {
        const token = req.cookies.token;
        if (!token) {
            return res.status(401).json({
                message: "User not authenticated",
                success: false,
            })
        }
        const decode = await jwt.verify(token, process.env.SECRET_KEY); // this will extract the userId
        if(!decode){
            return res.status(401).json({
                message:"Invalid token",
                success:false
            })
        };
        req.id = decode.userId;
        next();
    } catch (error) {
        console.log(error);
    }
}
export default isAuthenticated;
```
## Populate Funtion
 The .populate() function in Mongoose is used to automatically replace
 a referenced ObjectId with the actual document it refers to.
- Assume a Job document looks like this before populating:

    ```js
        {
        "title": "Frontend Developer",
        "company": "64a1f3bc1240a8f..."
        }
    ```
  - After populating:

    ```js
    {
        "title": "Frontend Developer",
        "company": {
        "_id": "64a1f3bc1240a8f...",
        "name": "Tech Solutions",
        "location": "Bangalore",
    ...
    }
    }
    ```
