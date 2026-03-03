# API Test Cases – User Module

## Create User – POST /users

## Base URL

`https://api.example.com`

### TC_API_001 – Create user with valid mandatory fields

**Objective:**  
Verify that a user is successfully created when all mandatory fields contain valid data.

**Method:** POST  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Request Body:**

```json
{
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.ilyas@test.com",
  "password": "SecurePass123"
}
```

**Expected Status Code:** 201 Created

**Response Validations:**
- Response body contains `id`
- Email matches request
- Password is not returned
- Response time < 2 seconds

**Database Validation (if accessible):**
- New record created in `users` table
- Password stored in hashed format



This is a simple text
