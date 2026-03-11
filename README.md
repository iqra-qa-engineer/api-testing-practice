# API Testing Practice – User Module

This repository demonstrates structured API testing for a User Creation endpoint using manual test design and Postman execution.

---

## Project Overview

This project simulates real-world API testing workflow:

1. Reviewing Swagger documentation
2. Identifying required and optional fields
3. Designing positive, negative, and boundary test cases
4. Validating response schema and status codes
5. Testing authentication and conflict scenarios
6. Performing basic performance validation

---

## Repository Structure

```
api-testing-practice
│
├── postman-collections
│   └── Exported Postman collections
│
├── swagger-notes
│   └── API schema analysis & observations
│
├── test-scenarios
│   └── Structured API test cases (TC_API_001–TC_API_010)
│
└── README.md
```

---

## Test Coverage Includes

- Positive scenario validation
- Required field validation
- Invalid input handling
- Duplicate data conflict handling
- Authentication testing
- Boundary value testing
- Performance & stability checks
- Response schema validation

---

## Tools Used

- Postman
- Swagger UI
- Manual Test Design Techniques

---

## Objective of This Repository

To demonstrate practical API testing skills including:
- Requirement analysis
- Structured test case design
- API validation logic
- Security validation awareness
- Basic performance consideration

  ## How to Run the Postman Collection

Follow these steps to execute the API tests locally.

### 1. Import the Collection

1. Open **Postman**
2. Click **Import**
3. Select the file: 
   
postman-collections/user-api-testing.postman_collection.json


### 2. Configure Headers

Ensure the following headers are present in requests:

| Key | Value |
|----|----|
| Content-Type | application/json |
| Authorization | Bearer testtoken123 |

### 3. Run the Collection

1. Open the imported collection
2. Click **Run Collection**
3. Execute all test cases

The collection includes automated validations for:

- Status code verification
- Response body validation
- Authentication scenarios
- Error handling
- Boundary conditions
- Basic performance checks
