# Swagger Analysis – User Module

## Endpoint Reviewed

**Method:** POST  
**Endpoint:** `/users`  
**Authentication:** Bearer Token Required  

---

## Request Schema Observations

- `firstName` – Required, string
- `lastName` – Required, string
- `email` – Required, string (must be unique)
- `password` – Required, string (minimum length validation applied)

---

## Response Schema Observations

### Success Response (201 Created)
- `id` – Auto-generated unique identifier
- `firstName` – Returned as provided
- `lastName` – Returned as provided
- `email` – Returned as provided
- `createdAt` – Timestamp (ISO 8601 format)

### Error Responses
- 400 – Validation errors
- 401 – Unauthorized access
- 409 – Conflict (duplicate email)

Error responses follow structured JSON format:
```json
{
  "error": "ErrorType",
  "message": "Description",
  "statusCode": 400
}
```

---

## Design Decisions Based on Swagger

- Positive and negative test cases were designed based on required fields.
- Boundary testing included for field length validation.
- Authentication validation included due to protected endpoint.
- Conflict testing included due to unique email constraint.
- Error structure validation included to ensure API contract consistency.

---
