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
