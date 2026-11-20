# PolyglotMesh – Backend

A Spring Boot-based backend application for a multi-language code execution platform. PolyglotMesh is designed to provide REST APIs for submitting code execution requests, validating programming languages, and maintaining execution history.

> **Current Status:** Backend API structure and execution history management are implemented. Actual Python, JavaScript, and Ruby execution engine integration is pending.

---

## Project Overview

PolyglotMesh aims to provide a unified platform where users can submit source code in different programming languages through a common interface.

The backend is responsible for:

- Receiving code execution requests through REST APIs.
- Validating incoming request data.
- Checking supported programming languages.
- Communicating with an execution engine through a Java interface.
- Storing execution history in MongoDB.
- Managing execution records.
- Handling application exceptions and validation errors.

## Tech Stack

| Technology | Purpose |
|---|---|
| Java 24 | Backend programming language |
| Spring Boot 3.5.16 | Backend framework |
| Spring WebFlux | REST API development |
| Spring Data MongoDB | Database integration |
| MongoDB | Execution history storage |
| Maven | Dependency management and build |
| Postman | API testing |
| Eclipse / Spring Tools | Development environment |

## Features Implemented

### 1. REST API Development
- Backend health-check endpoint.
- Execution service health-check endpoint.
- Code execution request endpoint.
- Execution history retrieval.
- Execution history retrieval by ID.
- Execution history deletion.

### 2. Request Validation
- Validates incoming execution requests.
- Prevents empty language values.
- Prevents empty source code.
- Checks supported programming languages.

### 3. Supported Languages

The backend currently recognizes:

- Python
- JavaScript
- Ruby

### 4. Execution Engine Integration Structure

An `ExecutionEngine` interface has been created to define the contract for code execution.

The current implementation uses `PendingExecutionEngine` as a temporary placeholder.

The actual execution engine integration will be completed during the team's final integration phase.

### 5. Execution History Management

MongoDB is used to store execution records, including:

- Execution ID
- Programming language
- Submitted source code
- Execution output
- Execution status
- Error message
- Execution time
- Creation timestamp

### 6. Exception Handling

A centralized `GlobalExceptionHandler` handles:

- Resource not found errors – HTTP 404
- Request validation errors – HTTP 400
- Unexpected application errors – HTTP 500

---

## Project Structure

```text
PolyglotMesh/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── polyglotmesh/
│   │   │           │
│   │   │           ├── PolyglotMeshApplication.java
│   │   │           │
│   │   │           ├── controller/
│   │   │           │   └── ExecutionController.java
│   │   │           │
│   │   │           ├── dto/
│   │   │           │   ├── ExecutionRequest.java
│   │   │           │   └── ExecutionResponse.java
│   │   │           │
│   │   │           ├── entity/
│   │   │           │   └── ExecutionHistory.java
│   │   │           │
│   │   │           ├── exception/
│   │   │           │   ├── GlobalExceptionHandler.java
│   │   │           │   └── ResourceNotFoundException.java
│   │   │           │
│   │   │           ├── repository/
│   │   │           │   └── ExecutionHistoryRepository.java
│   │   │           │
│   │   │           └── service/
│   │   │               ├── ExecutionEngine.java
│   │   │               ├── ExecutionService.java
│   │   │               └── PendingExecutionEngine.java
│   │   │
│   │   └── resources/
│   │       └── application.properties
│   │
│   └── test/
│       └── java/
│
├── .mvn/
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

## REST API Documentation

Base URL:

```text
http://localhost:8080
```

| HTTP Method | Endpoint | Description |
|---|---|---|
| GET | `/api/test` | Check backend status |
| GET | `/api/service-test` | Check execution service |
| POST | `/api/execute` | Submit code execution request |
| GET | `/api/history` | Retrieve all execution history |
| GET | `/api/history/{id}` | Retrieve history by ID |
| DELETE | `/api/history/{id}` | Delete history by ID |

### 1. Backend Health Check

**Request**

```http
GET /api/test
```

**Response**

```text
PolyglotMesh Backend is Running!
```

### 2. Execution Service Health Check

**Request**

```http
GET /api/service-test
```

**Response**

```text
Execution Service is Working!
```

### 3. Code Execution API

**Request**

```http
POST /api/execute
Content-Type: application/json
```

**Example – Python**

```json
{
  "language": "python",
  "code": "print('Hello from Python')"
}
```

**Example – JavaScript**

```json
{
  "language": "javascript",
  "code": "console.log('Hello from JavaScript')"
}
```

**Example – Ruby**

```json
{
  "language": "ruby",
  "code": "puts 'Hello from Ruby'"
}
```

**Current Response (Temporary Engine)**

```json
{
  "output": null,
  "status": "PENDING",
  "error": "Execution engine is not connected yet."
}
```

The current placeholder implementation does not execute source code. It returns a pending response until the actual execution engine is integrated.

### 4. Get All Execution History

**Request**

```http
GET /api/history
```

Returns all saved execution records from MongoDB.

### 5. Get Execution History by ID

**Request**

```http
GET /api/history/{id}
```

Returns a specific execution record.

If the requested ID does not exist, the API returns HTTP 404.

### 6. Delete Execution History

**Request**

```http
DELETE /api/history/{id}
```

Deletes the specified execution history record.

---

## Getting Started

Follow these steps to run the backend locally.

### Prerequisites

Make sure you have installed:

- Java JDK 24
- Maven (optional when using the included Maven Wrapper)
- MongoDB
- Eclipse / Spring Tools or IntelliJ IDEA
- Postman

### Step 1: Clone the Repository

```bash
git clone https://github.com/Ankit-mudi/PolyglotMesh-Backend.git
```

### Step 2: Navigate to the Project

```bash
cd PolyglotMesh-Backend
```

### Step 3: Start MongoDB

Make sure your MongoDB server is running locally on its configured port.

The current development setup uses the default local MongoDB connection.

### Step 4: Run the Application

On Windows:

```bash
mvnw.cmd spring-boot:run
```

Alternatively, if Maven is installed:

```bash
mvn spring-boot:run
```

### Step 5: Verify the Backend

Open:

```text
http://localhost:8080/api/test
```

If the application is running successfully, you should receive:

```text
PolyglotMesh Backend is Running!
```

---

## Database

PolyglotMesh uses MongoDB for storing execution history.

**Collection:**

```text
execution_history
```

Each execution record stores the submitted language, source code, output, status, error information, execution time, and creation timestamp.

---

## Error Handling

The backend provides centralized exception handling using `@RestControllerAdvice`.

| HTTP Status | Description |
|---|---|
| 200 | Request processed successfully |
| 400 | Invalid request or validation failure |
| 404 | Requested execution history not found |
| 500 | Unexpected internal server error |

---

## Current Development Status

| Component | Status |
|---|---|
| Spring Boot project setup | Completed |
| REST API structure | Completed |
| Code execution request API | Completed |
| Request validation | Completed |
| MongoDB integration | Completed |
| Execution history management | Completed |
| Centralized exception handling | Completed |
| Execution engine interface | Completed |
| Temporary execution engine implementation | Completed |
| Actual multi-language code execution | Pending |
| Final team integration | Pending |

---

## Developer Responsibilities

**Developer 1 – Backend Development**

Responsible for:

- Spring Boot project setup.
- REST API development.
- Request validation.
- MongoDB integration.
- Execution history management.
- Exception handling.
- Backend-side execution engine integration structure.
- Backend testing and bug fixes.

---

## Future Enhancements

- Integration with the actual multi-language execution engine.
- Real-time execution results.
- Improved execution error reporting.
- Execution timeout and resource management.
- Enhanced API security.
- Additional supported programming languages.

---

## Author

**Ankit Mudi**

GitHub: [@Ankit-mudi](https://github.com/Ankit-mudi)

Project Repository: [PolyglotMesh-Backend](https://github.com/Ankit-mudi/PolyglotMesh-Backend)

---

*PolyglotMesh – Building a unified backend for multi-language code execution.*
