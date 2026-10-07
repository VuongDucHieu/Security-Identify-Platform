# Secure Identity Platform — Product Requirements

## 1. Project Overview

Secure Identity Platform là hệ thống Identity & Authentication được xây dựng
nhằm cung cấp cơ chế xác thực an toàn hơn so với hệ thống chỉ sử dụng
email/password.

Hệ thống hỗ trợ nhiều phương thức authentication, quản lý session và thiết bị,
đồng thời cung cấp các cơ chế hạn chế và ghi nhận những hành vi đăng nhập
đáng ngờ.

---

## 2. Problem Statement

Các hệ thống chỉ sử dụng email/password có thể gặp nhiều vấn đề:

- Password reuse
- Weak passwords
- Credential stuffing
- Brute-force attacks
- Password leakage
- Phishing
- Khó quản lý các session đang hoạt động
- Khó kiểm soát thiết bị đã đăng nhập
- Thiếu khả năng theo dõi các authentication event đáng ngờ

Project hướng tới xây dựng một authentication platform có nhiều lớp bảo vệ
thay vì phụ thuộc vào một authentication method duy nhất.

---

## 3. Product Goals

Hệ thống cần:

1. Cho phép người dùng đăng ký và đăng nhập bằng email/password.
2. Hỗ trợ Passkey/WebAuthn.
3. Hỗ trợ Face Verification.
4. Cho phép quản lý session.
5. Cho phép quản lý thiết bị đăng nhập.
6. Cho phép revoke session.
7. Hỗ trợ logout khỏi tất cả thiết bị.
8. Hạn chế brute-force login.
9. Theo dõi login attempts.
10. Ghi nhận security events và audit logs.

---

## 4. Authentication Methods

### 4.1 Password Authentication

User có thể:

- Register bằng email/password
- Login bằng email/password
- Logout
- Refresh authentication
- Logout khỏi tất cả thiết bị

Password tuyệt đối không được lưu dưới dạng plaintext.

---

### 4.2 Passkey Authentication

User có thể:

- Register Passkey
- View Passkeys
- Remove Passkey
- Login bằng Passkey

Passkey sử dụng WebAuthn và public-key cryptography.

High-level flow:

Client
→ Request challenge
→ Server generates challenge
→ Browser/WebAuthn
→ User verifies through platform authenticator
→ Authenticator signs challenge
→ Server verifies signature
→ Authentication success

Private key không được gửi tới Backend.

---

### 4.3 Face Verification

User có thể đăng ký face information và sử dụng nó trong authentication flow.

High-level flow:

Face
→ Face detection
→ Embedding model
→ Face embedding
→ Store embedding

Khi verification:

New face
→ Generate embedding
→ Compare with stored embedding
→ Calculate similarity/distance
→ Apply threshold
→ Match / Not Match

Face Verification trong project này không được coi là tương đương với
platform biometric authentication được sử dụng bởi Passkey.

---

## 5. Session & Device Management

Hệ thống cần theo dõi các authentication session của user.

User có thể:

- Xem các session đang hoạt động
- Xem thiết bị liên quan đến session
- Revoke một session
- Logout khỏi tất cả thiết bị

Ví dụ:

User
├── Chrome / Windows
├── iPhone
└── MacBook

Mỗi thiết bị có thể có session riêng.

---

## 6. Security Requirements

Hệ thống cần có các cơ chế bảo vệ authentication flow.

Bao gồm:

- Rate limiting
- Login attempt tracking
- Temporary account locking
- Session revocation
- Security event tracking
- Audit logging

Ví dụ:

Repeated failed login
→ Track login attempt
→ Threshold reached
→ Temporary lock
→ Record security event

---

## 7. Main Product Modules

### Identity

Quản lý danh tính và account của user.

### Authentication

Xác minh user thông qua:

- Password
- Passkey
- Face Verification

### Session

Quản lý authentication sessions.

### Device

Theo dõi các thiết bị liên quan đến authentication.

### Security

Theo dõi và hạn chế các hành vi authentication đáng ngờ.

### Audit

Lưu lại các security-sensitive actions để phục vụ investigation và auditing.

---

## 8. Initial Scope

V1 tập trung vào:

- User registration
- Email verification
- Password login
- Logout
- Refresh authentication
- Session management
- Device management
- Passkey
- Face Verification
- Rate limiting
- Login attempt protection
- Account locking
- Security events
- Audit logs

---

## 9. Out of Scope for Initial Version

V1 chưa tập trung vào:

- Social login
- OAuth provider
- Enterprise SSO
- SAML
- Admin dashboard
- Billing
- Organization/multi-tenant
- Production-scale infrastructure

Các feature này có thể được bổ sung sau khi authentication core ổn định.

---

## 10. Engineering Goals

Project không chỉ nhằm tạo ra API register/login.

Engineering goals bao gồm:

- Authentication architecture
- Secure credential handling
- Session lifecycle
- Database integrity
- Concurrent request handling
- Race condition awareness
- WebAuthn
- Public/private key authentication
- Face embeddings
- Similarity comparison
- Rate limiting
- Security event tracking
- Auditability
- Automated testing

---

## 11. Learning Goals

Sau project, Backend Engineer cần có khả năng giải thích:

- Authentication khác Authorization như thế nào?
- Password nên được lưu như thế nào?
- Session hoạt động như thế nào?
- Access token và refresh token giải quyết vấn đề gì?
- Passkey hoạt động như thế nào?
- Challenge-response authentication là gì?
- Public/private key được sử dụng ra sao?
- Face embedding là gì?
- Similarity/distance được sử dụng như thế nào?
- Race condition có thể xảy ra ở authentication system như thế nào?
- Rate limiting giải quyết vấn đề gì?
- Session revocation hoạt động như thế nào?
- Audit log và security event khác nhau như thế nào?

---

## 12. Success Criteria

Project được coi là hoàn thành khi:

- Core authentication flows hoạt động đúng.
- Business rules được document rõ ràng.
- Database bảo vệ được các important invariants.
- Authentication flows có automated tests.
- Security-sensitive actions được audit.
- Các failure cases quan trọng được xử lý.
- Developer có thể giải thích các architectural decisions.
- AI coding agent có đủ documentation để hiểu requirement trước khi
  implement hoặc modify feature.

---

## 13. Project Principle

Business requirements phải được xác định trước implementation.

Workflow:

PO Requirements
→ Business Analysis
→ Business Rules
→ System Design
→ Database Design
→ Technical Design
→ Ticket
→ Implementation
→ Testing
→ Security Review

Không lựa chọn implementation chỉ vì framework/library hỗ trợ sẵn.

Mỗi technical decision quan trọng cần trả lời:

1. Problem là gì?
2. Tại sao cần giải quyết?
3. Giải pháp được chọn là gì?
4. Alternative là gì?
5. Trade-off là gì?
6. Khi nào không nên sử dụng giải pháp này?