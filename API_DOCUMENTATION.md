# Financial Dashboard System - API Documentation

Hey! 👋 This is a guide to using the Financial Dashboard API. Don't worry, it's simpler than it looks!

## Base URL
```
Local Development: http://localhost:4000/api
Production (Render): https://financial-dashboard-system-ltzw.onrender.com/api
```

## How Login Works 🔐
After you login, you'll get a special code (JWT token). Use this code for all future requests:
```
Header: Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## How Responses Look 📨
Success:
```json
{
  "success": true,
  "data": { /* your data here */ }
}
```

Oops! Something went wrong:
```json
{
  "success": false,
  "error": "What went wrong"
}
```

---

## 🏥 Health Check
Just checking if the server is awake...

**GET** `/health`

**Response:**
```json
{
  "success": true,
  "data": { "ok": true }
}
```

---

## 🔐 Auth - Let's Get You In (`/api/auth`)

### 1️⃣ Sign Up (Register)
**POST** `/auth/register`

Create your account!

**What to send:**
```json
{
  "email": "rahul.sharma@gmail.com",
  "name": "Rahul Sharma",
  "password": "MyPass@2024"
}
```

**Rules:**
- Email: Must be a real email
- Name: At least 2 letters, max 100
- Password: Minimum 8 letters, needs 1 CAPITAL letter + 1 number

**What you get back (201 - New account created!):**
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

**Copy this token!** You'll need it for everything else. 👆

---

### 2️⃣ Login
**POST** `/auth/login`

Welcome back! Get your token to access stuff.

**Send this:**
```json
{
  "email": "rahul.sharma@gmail.com",
  "password": "MyPass@2024"
}
```

**You get this (200 - All good!):**
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

## 👤 User Stuff (`/api/users`)

### 1️⃣ See Your Profile
**GET** `/users/me`

Check your own details.

**Need to add?** `Authorization: Bearer <your_token>`

**Response (200 - Here's you!):**
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

### 2️⃣ See All Users (Admin Only)
**GET** `/users`

See everyone who's registered (only admins can do this).

**Need?** Token + Admin access

**Try this URL:**
```
http://localhost:4000/api/users?page=1&limit=10&role=ADMIN
```

| What's This? | Type | Default | What It Does |
|---|---|---|---|
| page | number | 1 | Which page? |
| limit | number | 10 | How many per page? |
| role | text | - | Filter by: ADMIN, ANALYST, VIEWER |
| status | text | - | Filter by: ACTIVE, INACTIVE |

**Response (200 - Here they are!):**
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

### 3️⃣ Change Someone's Role (Admin Only)
**PATCH** `/users/:id/role`

Like promoting someone from viewer to analyst.

**Send this:**
```json
{
  "role": "ANALYST"
}
```

**Choices:** `ADMIN`, `ANALYST`, `VIEWER`

**Response (200 - Done!):**
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

### 4️⃣ Turn User On/Off (Admin Only)
**PATCH** `/users/:id/status`

Is the user active or inactive?

**Send:**
```json
{
  "status": "INACTIVE"
}
```

**Pick:** `ACTIVE` or `INACTIVE`

**Response (200 - Updated!):**
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

## 💰 Financial Records (`/api/records`)

### 1️⃣ Add Money In/Out
**POST** `/records`

Log your income or expense. Like "Got salary from TCS" or "Spent ₹150 on chai".

**Need?** Token + Admin access only

**Send:**
```json
{
  "type": "INCOME",
  "category": "SALARY",
  "amount": 50000.00,
  "description": "Monthly salary from TCS",
  "date": "2024-01-15"
}
```

**Fields:**

| Field | What? | Required | Notes |
|-------|-------|----------|-------|
| type | Income or Expense? | Yes | `INCOME` or `EXPENSE` |
| category | What type? | Yes | See below 👇 |
| amount | How much? | Yes | Number with max 2 decimals (₹ symbol not needed) |
| description | Why/what for? | No | Explanation (0-500 letters) |
| date | When? | Yes | Format: YYYY-MM-DD |

**Income Types:**
- `SALARY` - Regular job salary
- `FREELANCE` - Side gigs 
- `INVESTMENT` - Stock/crypto returns
- `BONUS` - Diwali bonus, performance bonus
- `OTHER_INCOME` - Random money that came

**Expense Types:**
- `FOOD` - Biryani, chai, lunch...
- `TRANSPORT` - Auto, cab, petrol for car
- `UTILITIES` - Electricity, water bill
- `ENTERTAINMENT` - Netflix, movies, games
- `HEALTHCARE` - Doctor, medicines
- `EDUCATION` - Courses, books
- `OTHER_EXPENSE` - Random spending

**You get (201 - Saved!):**
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

---

### 2️⃣ Get Your Records
**GET** `/records`

See all your income/expense logs. Can filter & sort!

**Need?** Token + Analyst/Admin access

**Example URL:**
```
http://localhost:4000/api/records?page=1&limit=10&type=INCOME&sortBy=date&sortOrder=desc
```

| Param | Type | Default | Meaning |
|-------|------|---------|---------|
| page | number | 1 | Page number |
| limit | number | 10 | Per page (1-100) |
| type | text | - | INCOME or EXPENSE |
| category | text | - | Filter by category |
| startDate | text | - | From date (YYYY-MM-DD) |
| endDate | text | - | To date (YYYY-MM-DD) |
| sortBy | text | date | Sort by: date, amount, or createdAt |
| sortOrder | text | desc | asc (old first) or desc (new first) |

**Response (200 - Here's your stuff!):**
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
        "description": "Lunch biryani at Karachi Darbar",
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

### 3️⃣ See One Record
**GET** `/records/:id`

See details of one specific transaction.

**Response (200 - Here's that one!):**
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

### 4️⃣ Edit a Record
**PATCH** `/records/:id`

Oops, wrong amount? Fix it! (Only admins can edit)

**Send (just what's different):**
```json
{
  "amount": 55000.00,
  "description": "Monthly salary + bonus"
}
```

**It sends back:**
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

### 5️⃣ Delete a Record
**DELETE** `/records/:id`

Remove a transaction (admin only).

**Response (200 - It's gone!):**
```json
{
  "success": true,
  "data": {
    "message": "Record deleted successfully"
  }
}
```

---

## 📊 Dashboard (`/api/dashboard`)

Check your money stats! These endpoints don't edit, just show analytics.

### 1️⃣ Summary - Quick Overview
**GET** `/dashboard/summary`

"How much did I earn/spend this month?"

**Query:**
```
?startDate=2024-01-01&endDate=2024-01-31
```

| Param | Type | Notes |
|-------|------|-------|
| startDate | text | From date (YYYY-MM-DD) - optional |
| endDate | text | To date (YYYY-MM-DD) - optional |

**Response (200):**
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

**Translation:** Earned ₹150k, spent ₹35k, saved ₹115k in January. ✨

---

### 2️⃣ Category Breakdown - Where's My Money?
**GET** `/dashboard/category-breakdown`

"How much on food? How much on transport?"

**Query:**
```
?startDate=2024-01-01&endDate=2024-01-31
```

**Response (200):**
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

**Translation:** 
- Food: ₹15k (20 purchases - ouch! 😅)
- Transport: ₹8k (10 trips)
- Entertainment: ₹12k (love biryani & Netflix!)

---

### 3️⃣ Trends - Money Over Time
**GET** `/dashboard/trends`

"Show me day-by-day how much I earned vs spent"

**Query:**
```
?startDate=2024-01-15&endDate=2024-01-20
```

**Response (200):**
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

**Translation:**
- Jan 15: Got ₹50k salary 💰
- Jan 16: Spent more than earned 😅
- Jan 17: Freelance gig paid off! 🎉
- Jan 18: Weekend shopping 🛍️ (oof)

---

### 4️⃣ Recent Activity - Latest Transactions
**GET** `/dashboard/recent`

"Show me my last 5 transactions"

**Query:**
```
?limit=5&startDate=2024-01-01&endDate=2024-01-31
```

| Param | Type | Default | What? |
|-------|------|---------|-------|
| limit | number | 5 | How many recent? (1-20) |
| startDate | text | - | From date - optional |
| endDate | text | - | To date - optional |

**Response (200):**
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
      "description": "Dinner Party @ Tandoor Palace",
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

