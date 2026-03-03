# API Test Cases – User Module

## Create User – POST /users

## Base URL

`https://api.example.com`

---

## Assumptions & Scope

- Testing assumed against staging environment.
- API follows RESTful principles.
- Email field must be unique.
- Password policy requires minimum 8 characters with alphanumeric combination.
- Authentication is handled using Bearer token.
- All responses are expected in JSON format.
- Swagger documentation was reviewed before designing test cases.


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

**Expected Response Body (Example):**

```json
{
  "id": "12345",
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.ilyas@test.com",
  "createdAt": "2026-03-03T10:15:30Z"
}
```

**Response Validations:**
- `id` is generated and non-null
- Email matches request
- `createdAt` follows ISO 8601 format
- Password field is NOT returned

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- New record created in `users` table
- Password stored in hashed format

---

### TC_API_002 – Create user with missing mandatory field (email)

**Objective:**  
Verify that the API returns a validation error when the required field `email` is missing.

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
  "password": "SecurePass123"
}
```
**Expected Status Code:** 400 Bad Request

**Expected Response Body (Example):**

```json
{
  "error": "ValidationError",
  "message": "Email is required",
  "statusCode": 400
}

```

### TC_API_003 – Create user with invalid email format

**Objective:**  
Verify that the API returns a validation error when an improperly formatted email address is provided.

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
  "email": "iqra.invalid-email",
  "password": "SecurePass123"
}
```
**Expected Status Code:** 400 Bad Request

**Expected Response Body (Example):**

```json

{
  "error": "ValidationError",
  "message": "Invalid email format",
  "statusCode": 400
}
```
**Response Validations:**

- Status code is 400
- error field is present
- Error message clearly references email
- No id field is returned

**Performance Validation:**

- Response time < 2 seconds under normal load

**Database Validation (if accessible):**

- No new record created in users table
- No invalid email stored in database
  

### TC_API_004 – Create user with weak password (boundary validation)

**Objective:**  
Verify that the API enforces password strength rules and rejects weak or invalid passwords.

**Method:** POST  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Request Body (Weak Password Example):**

```json
{
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.weak@test.com",
  "password": "123"
}
```

**Expected Status Code:** 400 Bad Request

**Expected Response Body (Example):**

```json
{
  "error": "ValidationError",
  "message": "Password must meet minimum strength requirements",
  "statusCode": 400
}
```

**Response Validations:**
- Status code is 400
- `error` field is present
- Error message clearly references password strength policy
- No `id` field is returned

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- No new record created in `users` table
- Weak password is not stored in database

### TC_API_005 – Create user with duplicate email

**Objective:**  
Verify that the API prevents creation of a user when the email already exists in the system.

**Method:** POST  
**Endpoint:** `/users`

**Precondition:**  
A user with email `iqra.ilyas@test.com` already exists in the system.

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

**Expected Status Code:** 409 Conflict

**Expected Response Body (Example):**

```json
{
  "error": "ConflictError",
  "message": "Email already exists",
  "statusCode": 409
}
```

**Response Validations:**
- Status code is 409
- `error` field is present
- Error message clearly indicates duplicate email
- No new `id` is generated

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- No duplicate record created in `users` table
- Total user count remains unchanged

### TC_API_006 – Create user with unexpected additional field

**Objective:**  
Verify that the API properly handles unexpected or unsupported fields in the request body.

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
  "email": "iqra.extra@test.com",
  "password": "SecurePass123",
  "role": "admin"
}
```

**Expected Status Code:** 400 Bad Request  
*(or 201 Created if API is designed to ignore unknown fields — confirm based on API specification)*

**Expected Response Body (Example - Strict Validation):**

```json
{
  "error": "ValidationError",
  "message": "Unexpected field: role",
  "statusCode": 400
}
```

**Response Validations:**
- Status code matches API specification
- If strict validation → error references `role`
- If tolerant validation → user is created but `role` is ignored
- No unauthorized role assignment occurs

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- No unintended `role` field stored in database
- No privilege escalation occurs

### TC_API_007 – Create user without Authorization token

**Objective:**  
Verify that the API rejects the request when the Authorization header is missing.

**Method:** POST  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization header is NOT included

**Request Body:**

```json
{
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.noauth@test.com",
  "password": "SecurePass123"
}
```

**Expected Status Code:** 401 Unauthorized

**Expected Response Body (Example):**

```json
{
  "error": "Unauthorized",
  "message": "Authentication token is missing or invalid",
  "statusCode": 401
}
```

**Response Validations:**
- Status code is 401
- `error` field is present
- Error message indicates missing authentication
- No `id` is generated

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- No new record created in `users` table
- No partial data stored

### TC_API_008 – Create user with expired or invalid Authorization token

**Objective:**  
Verify that the API rejects the request when an expired or invalid authentication token is provided.

**Method:** POST  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization: Bearer <expired_or_invalid_token>

**Request Body:**

```json
{
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.invalidtoken@test.com",
  "password": "SecurePass123"
}
```

**Expected Status Code:** 401 Unauthorized

**Expected Response Body (Example):**

```json
{
  "error": "Unauthorized",
  "message": "Authentication token is expired or invalid",
  "statusCode": 401
}
```

**Response Validations:**
- Status code is 401
- `error` field is present
- Error message clearly indicates invalid or expired token
- No `id` is generated

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- No new record created in `users` table
- No unauthorized user creation occurs

### TC_API_009 – Create user with maximum allowed firstName length

**Objective:**  
Verify that the API correctly accepts the maximum allowed length for the `firstName` field and rejects values exceeding the defined limit.

**Method:** POST  
**Endpoint:** `/users`

**Request Headers:**
- Content-Type: application/json
- Authorization: Bearer <valid_token>

**Request Body (Maximum Allowed Length Example – 50 characters):**

```json
{
  "firstName": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA",
  "lastName": "Ilyas",
  "email": "iqra.maxlength@test.com",
  "password": "SecurePass123"
}
```

**Expected Status Code (Valid Boundary):** 201 Created

**Expected Response Body (Example - Valid Case):**

```json
{
  "id": "67890",
  "firstName": "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA",
  "lastName": "Ilyas",
  "email": "iqra.maxlength@test.com",
  "createdAt": "2026-03-03T10:20:00Z"
}
```

**Response Validations (Valid Boundary):**
- Status code is 201
- User is successfully created
- `firstName` is stored exactly as provided
- No truncation occurs

**Negative Scenario (Exceeding Maximum Length):**
If `firstName` exceeds allowed length (e.g., 51+ characters), API should return validation error.

**Expected Status Code (Invalid Boundary):** 400 Bad Request

**Performance Validation:**
- Response time < 2 seconds under normal load

**Database Validation (if accessible):**
- Valid boundary → record stored correctly
- Invalid boundary → no record created

### TC_API_010 – Create user under repeated request load (basic performance & stability validation)

**Objective:**  
Verify that the API remains stable and responsive under repeated and concurrent user creation requests.

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
  "email": "iqra.loadtest@test.com",
  "password": "SecurePass123"
}
```

**Execution Strategy:**
- Execute 20–50 sequential requests
- Execute 10 concurrent requests (if tool supports concurrency)

**Expected Status Code:** 201 Created (for valid unique requests)

**Expected Response Body (Example):**

```json
{
  "id": "dynamic_id_value",
  "firstName": "Iqra",
  "lastName": "Ilyas",
  "email": "iqra.loadtest@test.com",
  "createdAt": "2026-03-03T10:25:00Z"
}
```

**Response Validations:**
- Status code is 201 for each valid request
- No 5xx server errors occur
- Each request generates a unique `id`
- No duplicate records created unintentionally

**Performance Validation:**
- Response time remains < 2 seconds under repeated load
- No significant degradation in response time across executions

**Database Validation (if accessible):**
- All successfully processed requests create valid records
- No partial or corrupted records stored
- Database integrity remains consistent



  
