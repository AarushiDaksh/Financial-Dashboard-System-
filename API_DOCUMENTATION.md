# Financial Dashboard System - API Documentation

## Base URL
```
http://localhost:4000/api
OR
https://your-production-url/api
```

## Authentication
Most endpoints require a JWT token. Include the token in the `Authorization` header:
```
Authorization: Bearer <your_jwt_token>
```

## Response Format
All responses follow this format:
```json
{
  "success": true,
  "data": { /* endpoint-specific data */ }
}
```

Error responses:
```json
{
  "success": false,
  "error": "Error message"
}
```

---

## Health Check
### GET /health
Check if the server is running (no authentication required).

**Response:**
```json
{
  "success": true,
  "data": {
    "ok": true
  }
}
```

---

## 🔐 Authentication Module (`/api/auth`)

### 1. Register User
**POST** `/auth/register`

Register a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "name": "John Doe",
  "password": "SecurePass123"
}
```

**Requirements:**
- Email: Valid email format
- Name: 2-100 characters
- Password: Minimum 8 characters, at least 1 uppercase letter, at least 1 number

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "VIEWER",
    "status": "ACTIVE",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

### 2. Login User
**POST** `/auth/login`

Authenticate user and receive JWT token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "VIEWER",
    "status": "ACTIVE",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## 👤 Users Module (`/api/users`)

### 1. Get Current User Profile
**GET** `/users/me`

Retrieve the logged-in user's profile information.

**Authentication:** Required ✅

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "VIEWER",
    "status": "ACTIVE",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
```

---

### 2. List All Users (Admin Only)
**GET** `/users`

Retrieve a paginated list of all users (admin access required).

**Authentication:** Required ✅
**Authorization:** Admin only

**Query Parameters:**
```
?page=1&limit=10&role=ADMIN&status=ACTIVE
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| page | number | 1 | Page number (minimum 1) |
| limit | number | 10 | Items per page (1-100) |
| role | string | - | Filter by role (ADMIN, ANALYST, VIEWER) |
| status | string | - | Filter by status (ACTIVE, INACTIVE) |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "email": "admin@example.com",
        "name": "Admin User",
        "role": "ADMIN",
        "status": "ACTIVE",
        "createdAt": "2024-01-15T10:30:00Z"
      },
      {
        "id": "550e8400-e29b-41d4-a716-446655440001",
        "email": "analyst@example.com",
        "name": "Analyst User",
        "role": "ANALYST",
        "status": "ACTIVE",
        "createdAt": "2024-01-16T10:30:00Z"
      }
    ],
    "Total": 2,
    "page": 1,
    "limit": 10
  }
}
```

---

### 3. Update User Role (Admin Only)
**PATCH** `/users/:id/role`

Change a user's role.

**Authentication:** Required ✅
**Authorization:** Admin only

**Request Body:**
```json
{
  "role": "ANALYST"
}
```

**Valid Roles:** `ADMIN`, `ANALYST`, `VIEWER`

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "ANALYST",
    "status": "ACTIVE",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

### 4. Update User Status (Admin Only)
**PATCH** `/users/:id/status`

Change a user's status (activate/deactivate).

**Authentication:** Required ✅
**Authorization:** Admin only

**Request Body:**
```json
{
  "status": "INACTIVE"
}
```

**Valid Statuses:** `ACTIVE`, `INACTIVE`

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "VIEWER",
    "status": "INACTIVE",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

## 💰 Financial Records Module (`/api/records`)

### 1. Create Financial Record
**POST** `/records`

Create a new income or expense record.

**Authentication:** Required ✅
**Authorization:** Admin only

**Request Body:**
```json
{
  "type": "INCOME",
  "category": "SALARY",
  "amount": 5000.00,
  "description": "Monthly salary for January",
  "date": "2024-01-15"
}
```

**Fields:**
| Field | Type | Required | Notes |
|-------|------|----------|-------|
| type | string | Yes | `INCOME` or `EXPENSE` |
| category | string | Yes | Category based on type |
| amount | number | Yes | Positive number, max 2 decimals |
| description | string | No | 0-500 characters |
| date | string | Yes | Format: YYYY-MM-DD |

**Income Categories:** `SALARY`, `FREELANCE`, `INVESTMENT`, `BONUS`, `OTHER_INCOME`
**Expense Categories:** `FOOD`, `TRANSPORT`, `UTILITIES`, `ENTERTAINMENT`, `HEALTHCARE`, `EDUCATION`, `OTHER_EXPENSE`

**Response (201 Created):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "userId": "550e8400-e29b-41d4-a716-446655440001",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 5000.00,
    "description": "Monthly salary for January",
    "date": "2024-01-15",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
```

---

### 2. List Financial Records
**GET** `/records`

Retrieve paginated list of financial records (Analyst/Admin only).

**Authentication:** Required ✅
**Authorization:** Analyst or Admin

**Query Parameters:**
```
?page=1&limit=10&type=INCOME&category=SALARY&startDate=2024-01-01&endDate=2024-01-31&sortBy=date&sortOrder=desc
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| page | number | 1 | Page number |
| limit | number | 10 | Items per page (1-100) |
| type | string | - | `INCOME` or `EXPENSE` |
| category | string | - | Filter by category |
| startDate | string | - | Start date (YYYY-MM-DD) |
| endDate | string | - | End date (YYYY-MM-DD) |
| sortBy | string | date | Sort by: `date`, `amount`, `createdAt` |
| sortOrder | string | desc | `asc` or `desc` |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "userId": "550e8400-e29b-41d4-a716-446655440001",
        "type": "INCOME",
        "category": "SALARY",
        "amount": 5000.00,
        "description": "Monthly salary for January",
        "date": "2024-01-15",
        "createdAt": "2024-01-15T10:30:00Z"
      }
    ],
    "total": 1,
    "page": 1,
    "limit": 10
  }
}
```

---

### 3. Get Single Record
**GET** `/records/:id`

Retrieve a specific financial record by ID.

**Authentication:** Required ✅
**Authorization:** Analyst or Admin

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "userId": "550e8400-e29b-41d4-a716-446655440001",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 5000.00,
    "description": "Monthly salary for January",
    "date": "2024-01-15",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
```

---

### 4. Update Financial Record
**PATCH** `/records/:id`

Update an existing financial record (at least one field required).

**Authentication:** Required ✅
**Authorization:** Admin only

**Request Body (partial update):**
```json
{
  "amount": 5500.00,
  "description": "Updated salary"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "userId": "550e8400-e29b-41d4-a716-446655440001",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 5500.00,
    "description": "Updated salary",
    "date": "2024-01-15",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

### 5. Delete Financial Record
**DELETE** `/records/:id`

Remove a financial record.

**Authentication:** Required ✅
**Authorization:** Admin only

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "message": "Record deleted successfully"
  }
}
```

---

## 📊 Dashboard Module (`/api/dashboard`)

### 1. Get Dashboard Summary
**GET** `/dashboard/summary`

Get summary of income, expenses, and record count for selected period.

**Authentication:** Required ✅
**Authorization:** Viewer, Analyst, or Admin

**Query Parameters:**
```
?startDate=2024-01-01&endDate=2024-01-31
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| startDate | string | - | Start date (YYYY-MM-DD) |
| endDate | string | - | End date (YYYY-MM-DD) |

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "totalIncome": 10000.00,
    "totalExpense": 3500.00,
    "recordCount": 15,
    "netBalance": 6500.00
  }
}
```

---

### 2. Get Category Breakdown
**GET** `/dashboard/category-breakdown`

Get total amount and count grouped by category and type.

**Authentication:** Required ✅
**Authorization:** Viewer, Analyst, or Admin

**Query Parameters:**
```
?startDate=2024-01-01&endDate=2024-01-31
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "category": "SALARY",
      "type": "INCOME",
      "totalAmount": 8000.00,
      "recordCount": 2
    },
    {
      "category": "FREELANCE",
      "type": "INCOME",
      "totalAmount": 2000.00,
      "recordCount": 1
    },
    {
      "category": "FOOD",
      "type": "EXPENSE",
      "totalAmount": 1500.00,
      "recordCount": 10
    },
    {
      "category": "UTILITIES",
      "type": "EXPENSE",
      "totalAmount": 800.00,
      "recordCount": 2
    }
  ]
}
```

---

### 3. Get Trends
**GET** `/dashboard/trends`

Get income and expense trends over time (grouped by day/week/month).

**Authentication:** Required ✅
**Authorization:** Viewer, Analyst, or Admin

**Query Parameters:**
```
?startDate=2024-01-01&endDate=2024-01-31
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "date": "2024-01-15",
      "income": 5000.00,
      "expense": 300.00,
      "net": 4700.00
    },
    {
      "date": "2024-01-16",
      "income": 2000.00,
      "expense": 500.00,
      "net": 1500.00
    },
    {
      "date": "2024-01-17",
      "income": 3000.00,
      "expense": 2700.00,
      "net": 300.00
    }
  ]
}
```

---

### 4. Get Recent Activity
**GET** `/dashboard/recent`

Get recently created financial records.

**Authentication:** Required ✅
**Authorization:** Viewer, Analyst, or Admin

**Query Parameters:**
```
?limit=5&startDate=2024-01-01&endDate=2024-01-31
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| limit | number | 5 | Number of recent records (1-20) |
| startDate | string | - | Start date (YYYY-MM-DD) |
| endDate | string | - | End date (YYYY-MM-DD) |

**Response (200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "type": "INCOME",
      "category": "SALARY",
      "amount": 5000.00,
      "description": "Monthly salary",
      "date": "2024-01-17",
      "createdAt": "2024-01-17T15:20:00Z"
    },
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "type": "EXPENSE",
      "category": "FOOD",
      "amount": 45.50,
      "description": "Restaurant",
      "date": "2024-01-17",
      "createdAt": "2024-01-17T14:10:00Z"
    }
  ]
}
```

---

## User Roles & Permissions

| Role | Permissions |
|------|-------------|
| **ADMIN** | Full access to all endpoints. Can manage users, records, and view dashboard. |
| **ANALYST** | Can view and create financial records. Can view dashboard analytics. Cannot manage users. |
| **VIEWER** | Read-only access to dashboard analytics. Cannot create or modify records. Cannot manage users. |

---

## Error Responses

### 400 Bad Request
```json
{
  "success": false,
  "error": "Validation error message"
}
```

### 401 Unauthorized
```json
{
  "success": false,
  "error": "Authentication token is missing or invalid"
}
```

### 403 Forbidden
```json
{
  "success": false,
  "error": "You do not have permission to access this resource"
}
```

### 404 Not Found
```json
{
  "success": false,
  "error": "Resource not found"
}
```

### 500 Internal Server Error
```json
{
  "success": false,
  "error": "Internal server error"
}
```

---

## Testing with Thunder Client / Postman

### Example: Login and Get Token
1. **POST** `http://localhost:4000/api/auth/login`
   ```json
   {
     "email": "admin@example.com",
     "password": "Admin@1234"
   }
   ```
2. Copy the `token` from response
3. Use it in subsequent requests: `Authorization: Bearer <token>`

### Example: Create a Record
1. **POST** `http://localhost:4000/api/records`
2. Headers:
   ```
   Authorization: Bearer <your_token>
   Content-Type: application/json
   ```
3. Body:
   ```json
   {
     "type": "EXPENSE",
     "category": "FOOD",
     "amount": 45.50,
     "description": "Lunch",
     "date": "2024-01-15"
   }
   ```

---

## Summary

**Total Endpoints:** 14
- Authentication: 2 endpoints
- Users: 4 endpoints
- Records: 5 endpoints
- Dashboard: 4 endpoints

All endpoints (except health check and auth register/login) require JWT authentication.

