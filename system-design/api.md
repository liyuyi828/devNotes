# API Design

## HTTP Response Codes

### 1xx - Informational
- **100 Continue**: Server has received the request headers and client should proceed to send the request body
- **101 Switching Protocols**: Server is switching protocols as requested by the client
- **102 Processing**: Server has received and is processing the request (WebDAV)

### 2xx - Success
- **200 OK**: Standard response for successful HTTP requests
- **201 Created**: Request has been fulfilled and resulted in a new resource being created
- **202 Accepted**: Request has been accepted for processing, but processing is not complete
- **204 No Content**: Server successfully processed the request but is not returning any content
- **206 Partial Content**: Server is delivering only part of the resource due to a range header

### 3xx - Redirection
- **301 Moved Permanently**: Resource has been moved permanently to a new URL
- **302 Found**: Resource temporarily resides under a different URL
- **304 Not Modified**: Resource has not been modified since last request
- **307 Temporary Redirect**: Request should be repeated with another URL, but future requests should still use the original URL
- **308 Permanent Redirect**: Request and all future requests should be repeated using another URL

### 4xx - Client Error
- **400 Bad Request**: Server cannot process the request due to client error (malformed syntax, invalid request)
- **401 Unauthorized**: Authentication is required and has failed or not been provided
- **403 Forbidden**: Server understood the request but refuses to authorize it
- **404 Not Found**: Requested resource could not be found
- **405 Method Not Allowed**: Request method is not supported for the requested resource
- **409 Conflict**: Request conflicts with the current state of the server
- **410 Gone**: Resource is no longer available and will not be available again
- **422 Unprocessable Entity**: Request was well-formed but contains semantic errors
- **429 Too Many Requests**: User has sent too many requests in a given amount of time (rate limiting)

### 5xx - Server Error
- **500 Internal Server Error**: Generic error message when server encounters an unexpected condition
- **501 Not Implemented**: Server does not support the functionality required to fulfill the request
- **502 Bad Gateway**: Server received an invalid response from an upstream server
- **503 Service Unavailable**: Server is currently unavailable (overloaded or down for maintenance)
- **504 Gateway Timeout**: Server did not receive a timely response from an upstream server
- **507 Insufficient Storage**: Server is unable to store the representation needed to complete the request

## HTTP Methods

### GET
**Purpose**: Retrieve data from a server
- **Idempotent**: Yes (multiple identical requests have the same effect)
- **Safe**: Yes (does not modify server state)
- **Cacheable**: Yes
- **Request Body**: No
- **Use Cases**: Fetching user profiles, listing products, reading articles
- **Example**: `GET /api/users/123`

### POST
**Purpose**: Create new resources or submit data to be processed
- **Idempotent**: No (multiple requests may create multiple resources)
- **Safe**: No (modifies server state)
- **Cacheable**: Generally no
- **Request Body**: Yes
- **Use Cases**: Creating new users, submitting forms, uploading files
- **Example**: `POST /api/users` with user data in body

### PUT
**Purpose**: Update/replace an entire resource or create if it doesn't exist
- **Idempotent**: Yes (multiple identical requests have the same effect)
- **Safe**: No (modifies server state)
- **Cacheable**: No
- **Request Body**: Yes
- **Use Cases**: Completely updating user profiles, replacing documents
- **Example**: `PUT /api/users/123` with complete user data

### PATCH
**Purpose**: Partially update an existing resource
- **Idempotent**: Depends on implementation (should be designed to be idempotent)
- **Safe**: No (modifies server state)
- **Cacheable**: No
- **Request Body**: Yes
- **Use Cases**: Updating specific fields, changing user email, modifying settings
- **Example**: `PATCH /api/users/123` with only changed fields

### DELETE
**Purpose**: Remove a resource from the server
- **Idempotent**: Yes (deleting same resource multiple times has same effect)
- **Safe**: No (modifies server state)
- **Cacheable**: No
- **Request Body**: Usually no
- **Use Cases**: Deleting user accounts, removing posts, clearing cache
- **Example**: `DELETE /api/users/123`

### Additional Methods

#### HEAD
**Purpose**: Same as GET but returns only headers (no body)
- **Use Cases**: Checking if resource exists, getting metadata, cache validation

#### OPTIONS
**Purpose**: Retrieve allowed methods and capabilities for a resource
- **Use Cases**: CORS preflight requests, API discovery