# 📋 Changelog — Bank Transaction System

All notable changes to this project will be documented here.

---

## [1.0.0] - 2024

### Added
- User registration, login, logout with JWT authentication
- Token blacklisting for secure logout
- Account creation and management
- Double-entry ledger system (DEBIT/CREDIT entries)
- 10-step atomic transaction flow using MongoDB sessions
- Idempotency key support to prevent duplicate transactions
- System user support for initial fund disbursement
- Email notifications on registration and transaction completion
- Cookie + Bearer token flexible authentication
- Password hashing with bcryptjs

---

## Upcoming

- [ ] Pagination for transaction history
- [ ] GET balance endpoint
- [ ] Transaction reversal support
- [ ] Rate limiting
- [ ] Swagger/OpenAPI documentation
- [ ] Unit and integration tests
