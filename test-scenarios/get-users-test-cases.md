# API Test Cases – GET Users Module

## Retrieve Users – GET /users

**Base URL:**
`https://api.example.com`

---

## Assumptions & Scope

- API follows RESTful principles.
- Authentication uses Bearer token.
- Responses are returned in JSON format.
- Users endpoint returns a list of user objects.

---

### TC_API_GET_001 – Retrieve list of users successfully

**Objective:**  
Verify that the API returns a list of users when a valid request is made.

**Method:** GET  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Expected Status Code:** 200 OK

**Expected Response Body (Example):**

```json
[
  {
    "id": "12345",
    "firstName": "Iqra",
    "lastName": "Ilyas",
    "email": "iqra.ilyas@test.com"
  }
]
```

**Response Validations:**

- Status code is **200**
- Response body is an **array of users**
- Each user object contains:
  - `id`
  - `firstName`
  - `lastName`
  - `email`
- No sensitive data like `password` is returned

**Performance Validation:**

- Response time < 2 seconds

---

### TC_API_GET_002 – Retrieve user by valid ID

**Objective:**  
Verify that the API returns correct user details when a valid user ID is provided.

**Method:** GET  
**Endpoint:** `/users/{id}`

**Path Parameter Example:**

```
/users/12345
```

**Request Headers:**

- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Expected Status Code:** 200 OK

**Expected Response Body (Example):**

```json
{
  "id": "12345",
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.ilyas@test.com"
}
```

**Response Validations:**

- Status code is **200**
- Response contains `id`
- Returned `id` matches the requested ID
- Response includes:
  - `firstName`
  - `lastName`
  - `email`
- Sensitive fields like `password` are not returned

**Performance Validation:**

- Response time < 2 seconds

**Database Validation (if accessible):**

- User record exists in database for the provided ID

---

### TC_API_GET_003 – Retrieve user with invalid ID (404 scenario)

**Objective:**  
Verify that the API returns a 404 error when a non-existent user ID is requested.

**Method:** GET  
**Endpoint:** `/users/{id}`

**Path Parameter Example:**

```
/users/99999
```

**Request Headers:**

- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Expected Status Code:** 404 Not Found

**Expected Response Body (Example):**

```json
{
  "error": "UserNotFound",
  "message": "User with the specified ID does not exist",
  "statusCode": 404
}
```

**Response Validations:**

- Status code is **404**
- Response body contains `error` field
- Error message clearly indicates user does not exist
- No user data fields (`firstName`, `email`, etc.) are returned

**Performance Validation:**

- Response time < 2 seconds

**Database Validation (if accessible):**

- No user record exists with the requested ID

---

### TC_API_GET_004 – Retrieve users when no users exist (empty list scenario)

**Objective:**  
Verify that the API returns an empty array when no users exist in the system.

**Method:** GET  
**Endpoint:** `/users`

**Request Headers:**

- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Expected Status Code:** 200 OK

**Expected Response Body (Example):**

```json
[]
```

**Response Validations:**

- Status code is **200**
- Response body is an **empty array**
- No error message is returned
- Response format remains valid JSON

**Performance Validation:**

- Response time < 2 seconds

**Database Validation (if accessible):**

- Users table contains **0 records**

---

### TC_API_GET_005 – Retrieve users with pagination parameters

**Objective:**  
Verify that the API correctly returns paginated results when `page` and `limit` parameters are provided.

**Method:** GET  
**Endpoint:** `/users`

**Query Parameters Example:**

```
/users?page=1&limit=10
```

**Request Headers:**

- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Expected Status Code:** 200 OK

**Expected Response Body (Example):**

```json
{
  "page": 1,
  "limit": 10,
  "totalUsers": 45,
  "data": [
    {
      "id": "12345",
      "firstName": "Iqra",
      "lastName": "Ilyas",
      "email": "iqra.ilyas@test.com"
    }
  ]
}
```

**Response Validations:**

- Status code is **200**
- Response contains pagination fields:
  - `page`
  - `limit`
  - `totalUsers`
  - `data`
- `data` field contains an **array of user objects**
- Number of users returned does not exceed the `limit` value
- Each user object contains:
  - `id`
  - `firstName`
  - `lastName`
  - `email`

**Performance Validation:**

- Response time < 2 seconds

**Database Validation (if accessible):**

- Returned users match the requested page range

