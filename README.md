# 🏦 Bank Transaction System

A production-ready **Bank Transaction System** built with Node.js, Express, and MongoDB. It supports user authentication, bank account management, and secure money transfers using a **double-entry bookkeeping ledger** system with full transaction integrity via MongoDB atomic sessions.

---

## ✨ Features

- 🔐 **JWT Authentication** — Register, Login, Logout with token blacklisting
- 🏦 **Bank Account Management** — Create and manage user bank accounts
- 💸 **Secure Fund Transfers** — Double-entry ledger with idempotency key support
- 📧 **Email Notifications** — Sends emails on registration and transaction completion
- 🔒 **System User Support** — Special privileged user for initial fund disbursement
- 🧾 **10-Step Transfer Flow** — Validated, atomic, and fault-tolerant money transfers
- 🍪 **Cookie + Bearer Token** — Flexible auth via cookies or Authorization header
- 🛡️ **Duplicate Prevention** — Idempotency keys prevent double-spending

---

## 🏗️ Project Structure

```
bank-transaction-system/
│
├── server.js                          # Entry point
├── package.json                       # Dependencies & scripts
├── .env                               # Environment variables (not committed)
├── .env.example                       # Environment variable template
├── .gitignore
│
└── src/
    ├── app.js                         # Express app setup & routes
    │
    ├── config/
    │   └── db.js                      # MongoDB connection
    │
    ├── controllers/
    │   ├── auth.controller.js         # Register, Login, Logout
    │   ├── account.controller.js      # Bank account operations
    │   └── transaction.controller.js  # Fund transfer & initial funds
    │
    ├── middleware/
    │   └── auth.middleware.js         # JWT auth & system user guard
    │
    ├── models/
    │   ├── user.model.js              # User schema (bcrypt hashed password)
    │   ├── account.model.js           # Bank account schema with balance
    │   ├── ledger.model.js            # Ledger entry schema (DEBIT/CREDIT)
    │   ├── transaction.model.js       # Transaction schema with status
    │   └── blackList.model.js         # Blacklisted JWT tokens
    │
    ├── routes/
    │   ├── auth.routes.js             # /api/auth/*
    │   ├── account.routes.js          # /api/accounts/*
    │   └── transaction.routes.js      # /api/transactions/*
    │
    └── services/
        └── email.service.js           # Nodemailer email notifications
```

---

## ⚙️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Dishant2436/bank-transaction-system.git
cd bank-transaction-system
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Create `.env` File

Copy the example file and fill in your values:

```bash
cp .env.example .env
```

```env
MONGO_URI=mongodb+srv://your-username:your-password@cluster.mongodb.net/bank
JWT_SECRET=your_super_secret_jwt_key
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-gmail-app-password
PORT=3000
```

### 4. Start the Server

```bash
# Development (with auto-reload)
npm run dev

# Production
npm start
```

Server runs at: `http://localhost:3000`

---

## 🚀 API Endpoints

### 🔐 Auth Routes

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|--------------|
| POST | `/api/auth/register` | Register a new user | ❌ |
| POST | `/api/auth/login` | Login and receive token | ❌ |
| POST | `/api/auth/logout` | Logout and blacklist token | ✅ |

### 🏦 Account Routes

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|--------------|
| POST | `/api/accounts` | Create a new bank account | ✅ |
| GET | `/api/accounts` | Get all user accounts | ✅ |

### 💸 Transaction Routes

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|--------------|
| POST | `/api/transactions` | Transfer funds between accounts | ✅ User |
| POST | `/api/transactions/system/initial-funds` | Deposit initial funds to account | ✅ System |

---

## 📋 Request Examples

### Register a New User
```json
POST /api/auth/register
{
  "name": "Dishant Kumar",
  "email": "dishantsharma126@gmail.com",
  "password": "password123"
}
```

### Transfer Funds
```json
POST /api/transactions
Authorization: Bearer <token>

{
  "fromAccount": "account_id_sender",
  "toAccount": "account_id_receiver",
  "amount": 500,
  "idempotencyKey": "unique-key-12345"
}
```

---

## 🔄 10-Step Transaction Flow

```
1.  Validate request fields
2.  Check idempotency key — prevent duplicate transactions
3.  Verify both accounts exist and are ACTIVE
4.  Derive sender balance from ledger
5.  Check sender has sufficient balance
6.  Create transaction record (PENDING)
7.  Create DEBIT ledger entry for sender
8.  Create CREDIT ledger entry for receiver
9.  Mark transaction COMPLETED + commit MongoDB session (atomic)
10. Send email notification to sender
```

---

## 🔒 Security Features

- Passwords hashed with **bcryptjs** (10 salt rounds)
- JWT tokens expire in **3 days**
- Logged-out tokens stored in a **blacklist** to prevent reuse
- MongoDB **transactions + sessions** ensure atomic fund transfers
- **Idempotency keys** prevent duplicate transactions
- All sensitive credentials stored in environment variables

---

## 🛠️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| Node.js | Runtime environment |
| Express.js v5 | Web framework |
| MongoDB + Mongoose | Database & ODM |
| JWT | Authentication tokens |
| bcryptjs | Password hashing |
| Nodemailer | Email notifications |
| dotenv | Environment variables |
| cookie-parser | Cookie handling |
| nodemon | Dev auto-reload |

---

## 📌 Environment Variables

| Variable | Description |
|----------|-------------|
| `MONGO_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret key for signing JWT tokens |
| `EMAIL_USER` | Gmail address for sending emails |
| `EMAIL_PASS` | Gmail app password |
| `PORT` | Server port (default: 3000) |

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Dishant Kumar**
- 📧 Email: [dishantsharma126@gmail.com](mailto:dishantsharma126@gmail.com)
- 🐙 GitHub: [@Dishant2436](https://github.com/Dishant2436)
