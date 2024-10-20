# Flask Authentication and Todo API Application

## Overview

This project is a **Flask-based API** that manages **user authentication** and a **todo list** for each authenticated user. The app features **JWT-based token authentication**, ensuring that only authorized users can perform CRUD (Create, Read, Update, Delete) operations on their own todo list items. The app also features admin-level privileges, allowing certain users to manage other users within the system.

### Key Features:
- User authentication with JWT tokens
- Admin privilege management
- CRUD operations for users and todos
- Password hashing for secure storage
- SQLAlchemy ORM for database management
- RESTful API endpoints

## Application Structure

### Models
There are two models defined using **SQLAlchemy**:

1. **User Model**
   - Represents the users in the system.
   - Attributes:
     - `id`: Unique identifier for each user (primary key).
     - `public_id`: A unique ID (UUID) to identify users.
     - `name`: The user's name (username).
     - `password`: Hashed password for the user.
     - `admin`: Boolean value indicating whether the user is an admin.
   
2. **Todo Model**
   - Represents todo list items for each user.
   - Attributes:
     - `id`: Unique identifier for each todo (primary key).
     - `text`: The description of the todo item.
     - `complete`: Boolean value indicating if the todo is completed.
     - `user_id`: A foreign key linking the todo to a specific user.

### Endpoints

#### 1. **User Endpoints**

- **`GET /users`**
  - Returns all users (admin-only).
  - Example:
    ```bash
    curl -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/users
    ```
  
- **`GET /user/<public_id>`**
  - Returns details of a single user based on their public ID (admin-only).
  - Example:
    ```bash
    curl -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/user/<public_id>
    ```

- **`POST /create-user`**
  - Creates a new user (admin-only).
  - Example:
    ```bash
    curl -X POST -H "x-access-token: <JWT_TOKEN>" -H "Content-Type: application/json" \
    -d '{"name": "newuser", "password": "newpassword"}' \
    http://localhost:5000/create-user
    ```

- **`PUT /update-user/<public_id>`**
  - Promotes a user to admin status (admin-only).
  - Example:
    ```bash
    curl -X PUT -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/update-user/<public_id>
    ```

- **`DELETE /delete-user/<public_id>`**
  - Deletes a user from the system (admin-only).
  - Example:
    ```bash
    curl -X DELETE -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/delete-user/<public_id>
    ```

#### 2. **Todo Endpoints**

- **`GET /todos`**
  - Returns all todos for the current user.
  - Example:
    ```bash
    curl -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/todos
    ```

- **`GET /todo/<todo_id>`**
  - Returns details of a single todo item by its ID (restricted to the owner).
  - Example:
    ```bash
    curl -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/todo/<todo_id>
    ```

- **`POST /create-todo`**
  - Creates a new todo item for the current user.
  - Example:
    ```bash
    curl -X POST -H "x-access-token: <JWT_TOKEN>" -H "Content-Type: application/json" \
    -d '{"text": "My new todo"}' \
    http://localhost:5000/create-todo
    ```

- **`PUT /update-todo/<todo_id>`**
  - Marks a todo item as complete.
  - Example:
    ```bash
    curl -X PUT -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/update-todo/<todo_id>
    ```

- **`DELETE /delete-todo/<todo_id>`**
  - Deletes a todo item (restricted to the owner).
  - Example:
    ```bash
    curl -X DELETE -H "x-access-token: <JWT_TOKEN>" http://localhost:5000/delete-todo/<todo_id>
    ```

#### 3. **Login Endpoint**

- **`GET /login`**
  - Authenticates a user and generates a JWT token if successful. The token is valid for 30 minutes.
  - Example:
    ```bash
    curl -u username:password http://localhost:5000/login
    ```
  - The server returns a token in the response body:
    ```json
    {
      "token": "<JWT_TOKEN>"
    }
    ```

### Key Learnings and Skills

1. **JWT Authentication**
   - You will learn how to use **JSON Web Tokens (JWT)** for securing API endpoints and ensuring only authenticated users can access resources. JWT is a popular method for stateless authentication in modern applications.
   - **Key function**: 
     - `jwt.encode()` generates the token.
     - `jwt.decode()` validates the token and retrieves user information.

2. **User Privileges with Decorators**
   - This project uses **Python decorators** (`@wraps`) to enforce token authentication across routes.
   - **Example**: The `@token_required` decorator is applied to every route that requires authentication.

3. **Password Hashing**
   - Instead of storing raw passwords, the app securely hashes passwords using **Werkzeug’s `generate_password_hash()`** and verifies passwords using `check_password_hash()`.
   - Storing passwords securely is a fundamental skill for any developer working with user authentication.

4. **SQLAlchemy ORM**
   - The project uses **SQLAlchemy**, a powerful ORM (Object Relational Mapper), for database interactions. SQLAlchemy allows you to work with databases like SQLite, PostgreSQL, MySQL, and more, using Python objects instead of raw SQL queries.
   - **Key ORM features**:
     - Define models (e.g., `User`, `Todo`) and let SQLAlchemy handle the database schema.
     - Use `db.session.add()`, `db.session.commit()`, `db.session.delete()` to manage database operations.

5. **CRUD Operations**
   - The app implements all basic CRUD operations (Create, Read, Update, Delete) for both users and todo items.
   - This is critical for full-stack developers building REST APIs, especially in projects involving resource management like todo lists, blog posts, or e-commerce products.

6. **Admin Privileges**
   - The app demonstrates how to enforce user roles (e.g., admin privileges) to restrict access to certain routes. Only admins can manage other users, providing a clear example of **role-based access control (RBAC)**.

7. **Error Handling**
   - The app includes basic error handling for cases like missing authentication tokens, unauthorized access, or non-existent records, returning appropriate HTTP status codes (`401 Unauthorized`, `404 Not Found`).

### How to Run the Application

1. **Install Dependencies**:  
   Ensure you have Flask and other dependencies installed:
   ```bash
   pip install Flask Flask-SQLAlchemy Werkzeug PyJWT
   ```

2. **Run the Application**:
   You can run the Flask development server by executing:
   ```bash
   python <filename>.py
   ```

3. **Create the Database**:
   The app automatically creates the SQLite database (`site.db`) on the first run if it doesn't exist.

4. **Login and Obtain Token**:
   Use the `/login` endpoint to authenticate and get a JWT token, which you'll use in the `x-access-token` header for all other API requests.

### Conclusion

This Flask project covers key skills needed to build robust and secure web applications. You will gain experience with authentication, role management, database handling, and building RESTful APIs. These are essential for developing full-stack applications that are scalable and secure.

