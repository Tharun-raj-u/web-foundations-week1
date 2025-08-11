# HTTP & REST API Basics

## Step 1 – What is JSON and Why Is It Used?

**What is JSON (JavaScript Object Notation)?**  
JSON is a lightweight, text-based format for storing and exchanging data. It represents data as key–value pairs, arrays, and nested structures, making it easy for both humans to read and machines to parse.

**How does it differ from regular JavaScript objects?**  
While JSON looks similar to JavaScript objects, it is purely a string format. JSON keys and string values must be wrapped in double quotes, and it cannot contain functions or undefined values. JavaScript objects can have methods, variables without quotes, and more flexible syntax.

**Why is JSON used so widely in APIs instead of formats like XML or plain text?**  
- **Lightweight**: Smaller payloads mean faster network transfer.  
- **Universal**: Supported by almost all programming languages.  
- **Easy to parse**: Built-in parsers exist in most languages.  
- **Cleaner syntax**: Easier for humans to read than XML, and more structured than plain text.

---

## Step 2 – Exploring a Public REST API

Using **JSONPlaceholder** (`https://jsonplaceholder.typicode.com`).

### 1. GET Request
**Purpose:** Fetch a list of posts.

- **HTTP Method:** GET  
- **URL:** `https://jsonplaceholder.typicode.com/posts`  
- **Status Code:** `200 OK`  
- **Request Headers Sent:**  
  - `Accept: application/json`  
- **Response Headers Received:**  
  - `Content-Type: application/json; charset=utf-8`  
- **Response Body (first 1 item shown for brevity):**
```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit..."
  }
]
```

---

### 2. POST Request
**Purpose:** Create a new post.

- **HTTP Method:** POST  
- **URL:** `https://jsonplaceholder.typicode.com/posts`  
- **Request Headers Sent:**  
  - `Content-Type: application/json`  
- **Request Body:**
```json
{
  "title": "My New Post",
  "body": "This is a test post",
  "userId": 1
}
```
- **Status Code:** `201 Created`  
- **Response Body:**
```json
{
  "title": "My New Post",
  "body": "This is a test post",
  "userId": 1,
  "id": 101
}
```
- **Response Headers Received:**  
  - `Content-Type: application/json; charset=utf-8`

---

## Step 3 – Reflection

**Difference Between GET, POST, PUT, DELETE:**
- **GET** – Retrieve data from the server. Example: Viewing all blog posts.  
- **POST** – Send new data to the server. Example: Creating a new user account.  
- **PUT** – Update existing data completely. Example: Editing a user profile.  
- **DELETE** – Remove data from the server. Example: Deleting a comment.

**Major HTTP Status Code Categories:**
- **2xx – Success** (e.g., `200 OK`, `201 Created`) → Request completed successfully.  
- **4xx – Client Error** (e.g., `404 Not Found`, `400 Bad Request`) → Problem with the request.  
- **5xx – Server Error** (e.g., `500 Internal Server Error`) → Problem on the server side.

**What are Headers in HTTP?**
Headers are key–value pairs sent with requests and responses to pass additional information (e.g., format, authentication, caching).  
- Example observed: **`Content-Type: application/json`** – tells the server/client that the body content is JSON.

---

## Key Takeaways
- APIs allow applications to talk to servers through structured HTTP requests and responses.  
- HTTP requests have critical parts: method, URL, headers, and optional body.  
- Understanding HTTP helps you debug issues, design better APIs, and integrate third-party services efficiently.
