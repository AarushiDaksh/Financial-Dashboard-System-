# Financial Dashboard API Documentation

## Base URL
```
Local: http://localhost:4000/api
Production: https://financial-dashboard-system-ltzw.onrender.com/api
```

## Authentication
```
Header: Authorization: Bearer <token>
```

---

## Health Check

**GET** `/health`

Response (200):
```json
{
  "success": true,
  "data": { "ok": true }
}
```

---

## Auth Endpoints

### Register
**POST** `/auth/register`
Request:
```json
{
  "email": "rahul.sharma@gmail.com",
  "name": "Rahul Sharma",
  "password": "SecurePass123"
}
```

Response (201):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "rahul.sharma@gmail.com",
    "name": "Rahul Sharma",
    "role": "VIEWER",
    "status": "ACTIVE",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

### Login
**POST** `/auth/login`
Request:
```json
{
  "email": "rahul.sharma@gmail.com",
  "password": "SecurePass123"
}
```

Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "rahul.sharma@gmail.com",
    "name": "Rahul Sharma",
    "role": "VIEWER",
    "status": "ACTIVE",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## User Endpoints

### Get Current User
**GET** `/users/me`
Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "rahul.sharma@gmail.com",
    "name": "Rahul Sharma",
    "role": "VIEWER",
    "status": "ACTIVE",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
```

---

### List Users (Admin)
**GET** `/users?page=1&limit=10&role=ADMIN`

Response (200):
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "email": "priya.singh@gmail.com",
        "name": "Priya Singh",
        "role": "ADMIN",
        "status": "ACTIVE",
        "createdAt": "2024-01-15T10:30:00Z"
      },
      {
        "id": "550e8400-e29b-41d4-a716-446655440001",
        "email": "amit.patel@gmail.com",
        "name": "Amit Patel",
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

### Update User Role (Admin)
**PATCH** `/users/:id/role`
Request:
```json
{
  "role": "ANALYST"
}
```

Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "rahul.sharma@gmail.com",
    "name": "Rahul Sharma",
    "role": "ANALYST",
    "status": "ACTIVE",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

### Update User Status (Admin)
**PATCH** `/users/:id/status`
Request:
```json
{
  "status": "INACTIVE"
}
```

Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "rahul.sharma@gmail.com",
    "name": "Rahul Sharma",
    "role": "VIEWER",
    "status": "INACTIVE",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

## Records Endpoints

### Create Record (Admin)
**POST** `/records`
Request:
```json
{
  "type": "INCOME",
  "category": "SALARY",
  "amount": 50000.00,
  "description": "Monthly salary from TCS",
  "date": "2024-01-15"
}
```

Response (201):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "userId": "550e8400-e29b-41d4-a716-446655440001",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 50000.00,
    "description": "Monthly salary from TCS",
    "date": "2024-01-15",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

Categories - Income: SALARY, FREELANCE, INVESTMENT, BONUS, OTHER_INCOME
Categories - Expense: FOOD, TRANSPORT, UTILITIES, ENTERTAINMENT, HEALTHCARE, EDUCATION, OTHER_EXPENSE

---

### List Records
**GET** `/records?page=1&limit=10&type=INCOME&sortBy=date`

Response (200):
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "550e8400-e29b",
        "type": "INCOME",
        "category": "SALARY",
        "amount": 50000.00,
        "description": "Monthly salary from TCS",
        "date": "2024-01-15",
        "createdAt": "2024-01-15T10:30:00Z"
      },
      {
        "id": "550e8400-e29c",
        "type": "EXPENSE",
        "category": "FOOD",
        "amount": 350.00,
        "description": "Lunch at Karachi Darbar",
        "date": "2024-01-15",
        "createdAt": "2024-01-15T12:45:00Z"
      }
    ],
    "total": 2,
    "page": 1,
    "limit": 10
  }
}
```

---

### Get Single Record
**GET** `/records/:id`
Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 50000.00,
    "description": "Monthly salary from TCS",
    "date": "2024-01-15",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
```

---

### Update Record (Admin)
**PATCH** `/records/:id`
Request:
```json
{
  "amount": 55000.00,
  "description": "Monthly salary + bonus"
}
```

Response (200):
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "type": "INCOME",
    "category": "SALARY",
    "amount": 55000.00,
    "description": "Monthly salary + bonus",
    "date": "2024-01-15",
    "updatedAt": "2024-01-15T11:30:00Z"
  }
}
```

---

### Delete Record (Admin)
**DELETE** `/records/:id`
Response (200):
```json
{
  "success": true,
  "data": {
    "message": "Record deleted successfully"
  }
}
```

---

## Dashboard Endpoints

### Summary
**GET** `/dashboard/summary?startDate=2024-01-01&endDate=2024-01-31`

Response (200):
```json
{
  "success": true,
  "data": {
    "totalIncome": 150000.00,
    "totalExpense": 35000.00,
    "recordCount": 25,
    "netBalance": 115000.00
  }
}
```

---

### Category Breakdown
**GET** `/dashboard/category-breakdown?startDate=2024-01-01&endDate=2024-01-31`

Response (200):
```json
{
  "success": true,
  "data": [
    {
      "category": "SALARY",
      "type": "INCOME",
      "totalAmount": 100000.00,
      "recordCount": 1
    },
    {
      "category": "FREELANCE",
      "type": "INCOME",
      "totalAmount": 50000.00,
      "recordCount": 5
    },
    {
      "category": "FOOD",
      "type": "EXPENSE",
      "totalAmount": 15000.00,
      "recordCount": 20
    },
    {
      "category": "TRANSPORT",
      "type": "EXPENSE",
      "totalAmount": 8000.00,
      "recordCount": 10
    },
    {
      "category": "ENTERTAINMENT",
      "type": "EXPENSE",
      "totalAmount": 12000.00,
      "recordCount": 5
    }
  ]
}
```

---

### Trends
**GET** `/dashboard/trends?startDate=2024-01-15&endDate=2024-01-20`

Response (200):
```json
{
  "success": true,
  "data": [
    {
      "date": "2024-01-15",
      "income": 50000.00,
      "expense": 1500.00,
      "net": 48500.00
    },
    {
      "date": "2024-01-16",
      "income": 2000.00,
      "expense": 2500.00,
      "net": -500.00
    },
    {
      "date": "2024-01-17",
      "income": 15000.00,
      "expense": 8000.00,
      "net": 7000.00
    },
    {
      "date": "2024-01-18",
      "income": 0.00,
      "expense": 12000.00,
      "net": -12000.00
    }
  ]
}
```

---

### Recent Activity
**GET** `/dashboard/recent?limit=5&startDate=2024-01-01&endDate=2024-01-31`

Response (200):
```json
{
  "success": true,
  "data": [
    {
      "id": "550e8400-e29b",
      "type": "INCOME",
      "category": "FREELANCE",
      "amount": 15000.00,
      "description": "Website design gig",
      "date": "2024-01-20",
      "createdAt": "2024-01-20T18:30:00Z"
    },
    {
      "id": "550e8400-e29c",
      "type": "EXPENSE",
      "category": "FOOD",
      "amount": 850.00,
      "description": "Dinner at Tandoor Palace",
      "date": "2024-01-20",
      "createdAt": "2024-01-20T14:10:00Z"
    },
    {
      "id": "550e8400-e29d",
      "type": "EXPENSE",
      "category": "ENTERTAINMENT",
      "amount": 4000.00,
      "description": "Concert tickets",
      "date": "2024-01-19",
      "createdAt": "2024-01-19T15:20:00Z"
    },
    {
      "id": "550e8400-e29e",
      "type": "INCOME",
      "category": "SALARY",
      "amount": 50000.00,
      "description": "Monthly salary",
      "date": "2024-01-15",
      "createdAt": "2024-01-15T09:00:00Z"
    }
  ]
}
```

---

## User Roles & Permissions

| Role | Access |
|------|--------|
| ADMIN | Full access - manage users, records, dashboard |
| ANALYST | Create/view records, view dashboard |
| VIEWER | View dashboard only |

---

## Error Responses

400 Bad Request:
```json
{
  "success": false,
  "error": "Validation error message"
}
```

401 Unauthorized:
```json
{
  "success": false,
  "error": "Authentication token required"
}
```

403 Forbidden:
```json
{
  "success": false,
  "error": "Insufficient permissions"
}
```

404 Not Found:
```json
{
  "success": false,
  "error": "Resource not found"
}
```

500 Server Error:
```json
{
  "success": false,
  "error": "Internal server error"
}
```

