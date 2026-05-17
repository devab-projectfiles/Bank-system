<h1 align="center">🏦 Enterprise Banking API</h1>

<div align="center">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white" />
  <img src="https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" />
</div>

<br />

A production-ready, highly secure RESTful banking API built to demonstrate advanced backend patterns including **Atomic Database Transactions**, **Role-Based Access Control (RBAC)**, and **In-Memory Caching** for robust security.

## 🚀 Live Demo & Documentation

Test the live API instantly using the Postman collection below:

[![Run in Postman](https://run.pstmn.io/button.svg)](https://raw.githubusercontent.com/devab-projectfiles/Bank-system/main/Bank%20System.postman_collection.json)

---

## 🏗️ Architecture & Engineering Highlights

This project was built focusing on scalability, security, and data integrity:

- **Atomic Transactions**: Money transfers utilize MongoDB sessions. If a transfer fails mid-process, the database rolls back completely to prevent data corruption or "lost money".
- **Advanced Authentication**: Dual-token strategy utilizing short-lived JWT Access tokens and long-lived Refresh tokens.
- **Token Revocation**: Integrates Redis to instantly blacklist revoked tokens upon logout, ensuring absolute session security across all devices.
- **Repository Pattern**: Clean separation of concerns. Database logic is abstracted away from business logic, making the codebase highly maintainable.
- **Strict Validation**: All incoming requests are aggressively validated using **Zod** before hitting the controllers.

---

## 🌟 Core Features

- **User Management**: Secure registration, login, profile updates, and Google OAuth integration.
- **Account & Card Systems**: Users can open multiple bank accounts, link external credit/debit cards, and configure default payment methods.
- **Financial Operations**: Process deposits, withdrawals, and inter-account transfers.
- **Beneficiaries**: Save trusted contacts (CBE accounts, etc.) for rapid and secure transfers.
- **Admin Dashboard**: System-wide statistics, user blocking/unblocking, and transaction monitoring (strictly restricted to the `admin` role).

---

## 🔒 Security Measures

- **Password Cryptography**: Bcrypt hashing with strict salting.
- **Data Sanitization**: `express-mongo-sanitize` prevents NoSQL injection attacks.
- **Brute Force Protection**: `express-rate-limit` enforces strict API limits.
- **Header Security**: `Helmet.js` configures secure HTTP headers out of the box.

---

## 💻 Local Installation & Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/devab-projectfiles/Bank-system.git
   cd Bank-system
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure Environment Variables**
   Create a `.env.development` file in the root directory based on the following template:
   ```env
   PORT=3000
   NODE_ENV=development
   LOCAL_URI_DB=mongodb://localhost:27017/bank-system
   DB_URI_ONLINE=your_mongodb_atlas_connection_string
   SALT_ROUNDS=10
   PREFIX=Bearer
   ACCESS_TOKEN_KEY=your_access_secret
   REFRESH_TOKEN_KEY=your_refresh_secret
   ACCESS_TOKEN_EXPIRY=15m
   REFRESH_TOKEN_EXPIRY=7d
   REDIS_URL=your_upstash_redis_url
   CLIENT_ID=your_google_client_id
   EMAIL=your_email@gmail.com
   PASSWORD=your_email_app_password
   ```

4. **Run the server**
   ```bash
   # Starts compilation in watch mode and runs Nodemon
   npm run start:dev
   ```

---

## 📡 Primary API Routes

Here is a high-level overview of the available endpoints. Full details and request payloads are available inside the Postman collection.

| Module | Endpoints | Description | Auth Required |
| --- | --- | --- | --- |
| **Auth** | `POST /auth/register`<br>`POST /auth/login`<br>`POST /auth/logout` | User authentication & session management. | ❌ / ✅ |
| **User** | `GET /user/me`<br>`PATCH /user/update-info` | Manage personal profile and credentials. | ✅ |
| **Account** | `POST /account/create`<br>`GET /account/status` | Open accounts and generate filtered statements. | ✅ |
| **Card** | `POST /card/AddCard`<br>`GET /card/getAllCards` | Link external cards to internal accounts. | ✅ |
| **Transfer** | `PATCH /transaction/deposit`<br>`POST /transaction/transfer` | Move funds securely between accounts. | ✅ |
| **Admin** | `GET /admin/dashBoard`<br>`PATCH /admin/user/:id/block` | Manage the entire banking ecosystem. | 🔒 (Admin) |
