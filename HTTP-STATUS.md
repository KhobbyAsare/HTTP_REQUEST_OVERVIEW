# HTTP Status Codes Overview

HTTP status codes indicate the result of an HTTP request and are divided into five categories:

## 1. 1xx: Informational
These codes indicate that the request has been received and is being processed.

- **100 Continue**: The server has received the request headers and the client should proceed with the request body.
- **101 Switching Protocols**: The server is switching protocols as requested by the client.

## 2. 2xx: Successful
These codes indicate that the request was successfully received, understood, and accepted.

- **200 OK**: The request was successful, and the server returned the requested data.
- **201 Created**: The request was successful, and a new resource was created as a result.
- **204 No Content**: The request was successful, but there is no content to return (commonly used for DELETE requests).

## 3. 3xx: Redirection
These codes indicate that further action is needed to complete the request, typically involving redirection to another URL.

- **301 Moved Permanently**: The resource has been permanently moved to a new URL.
- **302 Found**: The resource is temporarily located at a different URL (used for temporary redirection).
- **304 Not Modified**: The resource has not been modified since the last request, and the cached version can be used.

## 4. 4xx: Client Errors
These codes indicate that there was an error with the client's request.

- **400 Bad Request**: The server could not understand the request due to invalid syntax.
- **401 Unauthorized**: The request requires user authentication. The client must authenticate themselves to get the requested response.
- **403 Forbidden**: The server understood the request but refuses to authorize it.
- **404 Not Found**: The server cannot find the requested resource.

## 5. 5xx: Server Errors
These codes indicate that the server failed to fulfill a valid request.

- **500 Internal Server Error**: The server encountered an unexpected condition that prevented it from fulfilling the request.
- **502 Bad Gateway**: The server was acting as a gateway or proxy and received an invalid response from the upstream server.
- **503 Service Unavailable**: The server is currently unable to handle the request due to temporary overload or maintenance.

## Summary
Understanding these HTTP status codes is crucial for debugging web applications and APIs, as they provide insight into the success or failure of requests. Using the correct status codes helps improve the clarity and usability of your web services.
