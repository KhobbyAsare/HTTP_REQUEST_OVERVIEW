Making an HTTP request using standard JavaScript (without libraries like Axios or Fetch API) typically involves using the `XMLHttpRequest` object. Below are examples of how to perform different types of HTTP requests (GET, POST, PUT, DELETE) using `XMLHttpRequest`.

`XMLHttpRequest` is a built-in JavaScript object that allows you to interact with servers via HTTP. It's widely supported and has been used for many years to make asynchronous requests.

### 1. Making a GET Request

```javascript
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://api.example.com/data", true);

xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status < 300) {
        console.log("Response:", xhr.responseText);
    } else {
        console.error("Error:", xhr.statusText);
    }
};

xhr.onerror = function() {
    console.error("Request failed");
};

xhr.send();
```

### 2. Making a POST Request

```javascript
const xhr = new XMLHttpRequest();
xhr.open("POST", "https://api.example.com/data", true);
xhr.setRequestHeader("Content-Type", "application/json");

xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status < 300) {
        console.log("Response:", xhr.responseText);
    } else {
        console.error("Error:", xhr.statusText);
    }
};

xhr.onerror = function() {
    console.error("Request failed");
};

const data = JSON.stringify({ name: "John", age: 30 });
xhr.send(data);
```

### 3. Making a PUT Request

```javascript
const xhr = new XMLHttpRequest();
xhr.open("PUT", "https://api.example.com/data/1", true);
xhr.setRequestHeader("Content-Type", "application/json");

xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status < 300) {
        console.log("Response:", xhr.responseText);
    } else {
        console.error("Error:", xhr.statusText);
    }
};

xhr.onerror = function() {
    console.error("Request failed");
};

const updatedData = JSON.stringify({ name: "Jane", age: 25 });
xhr.send(updatedData);
```

### 4. Making a DELETE Request

```javascript
const xhr = new XMLHttpRequest();
xhr.open("DELETE", "https://api.example.com/data/1", true);

xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status < 300) {
        console.log("Resource deleted");
    } else {
        console.error("Error:", xhr.statusText);
    }
};

xhr.onerror = function() {
    console.error("Request failed");
};

xhr.send();
```

### Summary

- **GET** requests are used to retrieve data from a server.
- **POST** requests are used to send data to the server to create a new resource.
- **PUT** requests are used to update an existing resource or create one if it doesn't exist.
- **DELETE** requests are used to remove a resource.

Using `XMLHttpRequest` allows you to make HTTP requests in a straightforward way, but for more modern JavaScript development, consider using the Fetch API for a cleaner and more powerful approach. If you need examples using Fetch or have further questions, let me know!