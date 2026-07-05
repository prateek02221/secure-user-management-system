<div align="center">

# 🔐 Secure User Management System

### A production-style authentication & authorization system built with Spring Boot, Spring Security, JWT & MySQL

[![Java](https://img.shields.io/badge/Java-17-orange?style=for-the-badge&logo=openjdk&logoColor=white)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white)](https://spring.io/projects/spring-security)
[![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)](https://jwt.io/)
[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/prateek02221/Secure-User-Management-System?style=flat-square)](../../stargazers)

**Register → Login → Get a JWT → Access role-protected dashboards. That's the whole ride.**

[Features](#-features) • [Tech Stack](#-tech-stack) • [Getting Started](#️-getting-started) • [API Reference](#-api-reference) • [Screenshots](#-screenshots) • [Roadmap](#-roadmap)

</div>

---

## ✨ Why this project?

Most "user management" demos stop at login. This one goes further — real password hashing, stateless JWT auth, and actual role separation between `USER` and `ADMIN`, wired end-to-end from a MySQL table to a plain HTML/CSS/JS frontend with zero frameworks. If you're learning Spring Security or want a clean starter for your next SaaS side project, this is built to be forked.

## 🚀 Features

| | Feature | Description |
|---|---|---|
| 📝 | **User Registration & Login** | Clean, validated auth flows out of the box |
| 🔑 | **Password Encryption** | BCrypt hashing — passwords are never stored in plain text |
| 🪪 | **JWT Authentication** | Stateless, scalable token-based sessions |
| 🛡️ | **Role-Based Access Control** | Separate `USER` and `ADMIN` permissions |
| 🔒 | **Protected REST APIs** | Endpoints locked down via Spring Security filters |
| 📊 | **User Dashboard** | A logged-in space for regular users |
| 👑 | **Admin Dashboard** | Elevated view for admin-only actions |
| 🚪 | **Secure Logout** | Proper token invalidation on the client |
| 🌐 | **CORS Enabled** | Frontend and backend talk to each other cleanly |

<details>
<summary><b>💡 Curious how the JWT flow works under the hood?</b></summary>
<br>

```
1. User submits credentials → POST /api/auth/login
2. Backend verifies password (BCrypt) against MySQL record
3. On success → Spring Security generates a signed JWT
4. Token returned to frontend, stored client-side
5. Every protected request → Authorization: Bearer <token>
6. Spring Security filter validates signature + expiry + role
7. Access granted (or 401/403 if invalid/insufficient role)
```

</details>

---

## 🧭 Architecture: Auth Flow

```mermaid
sequenceDiagram
    actor U as User
    participant F as Frontend (HTML/JS)
    participant B as Spring Boot API
    participant S as Spring Security
    participant D as MySQL

    U->>F: Enter credentials
    F->>B: POST /api/auth/login
    B->>D: Fetch user by username
    D-->>B: Hashed password + role
    B->>S: Verify password (BCrypt)
    S-->>B: Match confirmed
    B->>S: Generate signed JWT (with role claim)
    S-->>B: JWT token
    B-->>F: 200 OK + JWT
    F->>F: Store token client-side

    Note over U,D: Later — accessing a protected route

    U->>F: Visit dashboard
    F->>B: GET /api/user/dashboard<br/>Authorization: Bearer <JWT>
    B->>S: Validate token signature + expiry + role
    alt Valid & authorized
        S-->>B: Access granted
        B-->>F: 200 OK + dashboard data
    else Invalid / wrong role
        S-->>B: Access denied
        B-->>F: 401 Unauthorized / 403 Forbidden
    end
```

> Renders natively on GitHub — no image export needed. If it doesn't render in your viewer, any [Mermaid Live Editor](https://mermaid.live) will preview it instantly.

---

## 🛠 Tech Stack

<table>
<tr>
<td valign="top" width="50%">

### Backend
- ☕ **Java 17**
- 🍃 **Spring Boot**
- 🔐 **Spring Security**
- 🪪 **JWT (JSON Web Tokens)**
- 🐬 **MySQL**
- 🗃️ **JPA / Hibernate**

</td>
<td valign="top" width="50%">

### Frontend
- 🧱 **HTML5**
- 🎨 **CSS3**
- ⚡ **JavaScript (Fetch API)**
- 🚫 No frameworks — deliberately lightweight

</td>
</tr>
</table>

---

## 📂 Project Structure

```
Secure-User-Management-System
├── backend
│   └── usermanagement
│       ├── src/main/java/...          # Controllers, Services, Security config
│       └── src/main/resources/
│           └── application.properties  # DB + JWT config
├── frontend
│   ├── login.html
│   ├── register.html
│   ├── user.html
│   └── admin.html
└── README.md
```

---

## ▶️ Getting Started

### ✅ Prerequisites
- Java 17+
- Maven
- MySQL running locally (or update the connection string)

### 1️⃣ Clone the repo
```bash
git clone https://github.com/prateek02221/Secure-User-Management-System.git
cd Secure-User-Management-System
```

### 2️⃣ Configure the database
Update `backend/usermanagement/src/main/resources/application.properties`:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/user_management
spring.datasource.username=root
spring.datasource.password=your_password
jwt.secret=your_jwt_secret_key
```

### 3️⃣ Run the backend
```bash
cd backend/usermanagement
mvn spring-boot:run
```
The API will be live at `http://localhost:8080` 🎉

### 4️⃣ Launch the frontend
Simply open `frontend/login.html` in your browser (or serve the `frontend` folder with any static server / Live Server extension).

---

## 📡 API Reference

| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| `POST` | `/api/auth/register` | Public | Register a new user |
| `POST` | `/api/auth/login` | Public | Authenticate & receive JWT |
| `GET`  | `/api/user/dashboard` | 🔒 USER | User-only protected data |
| `GET`  | `/api/admin/dashboard` | 🔒 ADMIN | Admin-only protected data |
| `POST` | `/api/auth/logout` | 🔒 Authenticated | Invalidate current session |

> All protected routes require a header: `Authorization: Bearer <your_jwt_token>`

---

---

## 🗺 Roadmap

- [ ] Refresh token support
- [ ] Email verification on registration
- [ ] Forgot/reset password flow
- [ ] Rate limiting on auth endpoints
- [ ] Dockerize backend + MySQL
- [ ] Swagger/OpenAPI docs

Have an idea? Open an issue or PR — contributions welcome!

---

## 🤝 Contributing

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/amazing-thing`)
3. Commit your changes (`git commit -m 'Add amazing thing'`)
4. Push to the branch (`git push origin feature/amazing-thing`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the **MIT License** — feel free to use it for learning, portfolios, or your own builds.

---

<div align="center">

**Built with ☕, 🔐, and a healthy respect for BCrypt**

If this helped you, consider giving it a ⭐ — it really does help!

</div>
