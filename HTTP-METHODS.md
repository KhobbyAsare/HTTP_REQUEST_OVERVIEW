# HTTP Methods Overview

HTTP methods (also known as HTTP verbs) define the action to be performed on a given resource. Here are the most commonly used HTTP methods:

## 1. GET
- **Description**: Retrieves data from a server at the specified resource.
- **Idempotent**: Yes (multiple identical requests produce the same result).
- **Safe**: Yes (does not modify the resource).

## 2. POST
- **Description**: Sends data to the server to create a new resource.
- **Idempotent**: No (multiple identical requests may result in different outcomes).
- **Safe**: No (may modify the resource or create new resources).

## 3. PUT
- **Description**: Updates an existing resource or creates a new resource if it does not exist.
- **Idempotent**: Yes (multiple identical requests produce the same result).
- **Safe**: No (modifies the resource).

## 4. PATCH
- **Description**: Applies partial modifications to a resource.
- **Idempotent**: Yes (multiple identical requests produce the same result).
- **Safe**: No (modifies the resource).

## 5. DELETE
- **Description**: Removes a specified resource from the server.
- **Idempotent**: Yes (deleting a resource multiple times has the same effect).
- **Safe**: No (modifies the resource by removing it).

## 6. HEAD
- **Description**: Similar to GET, but retrieves only the headers of a resource, without the body.
- **Idempotent**: Yes.
- **Safe**: Yes.

## 7. OPTIONS
- **Description**: Describes the communication options for the target resource. It can be used to check the supported methods.
- **Idempotent**: Yes.
- **Safe**: Yes.

## 8. TRACE
- **Description**: Echoes back the received request to the client, primarily for diagnostic purposes.
- **Idempotent**: Yes.
- **Safe**: Yes.

## Summary
Understanding these HTTP methods is essential for designing and implementing RESTful APIs. Each method serves a specific purpose and defines how clients and servers interact with resources.
