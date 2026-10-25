# SmartSupport – Backend (Spring Boot)

SmartSupport is a robust, production-grade Customer Support / Helpdesk backend service built using **Java 17** and **Spring Boot 3**. It provides secure, role-based RESTful APIs for managing user accounts, support tickets, staff assignments, message conversations, and role authorization.

This repository contains the backend service. The frontend is built separately using React & Vite.

---

## ?? Related Links

- **Frontend Repository:** [https://github.com/vaishnavisonawane893/smartsupport-frontend](https://github.com/vaishnavisonawane893/smartsupport-frontend)

---

## ?? Tech Stack

- **Language:** Java 17
- **Framework:** Spring Boot 3.3.4
- **Security:** Spring Security with stateless JWT Authentication
- **Data Access:** Spring Data JPA / Hibernate
- **Database:** MySQL 8+ (or compatible cloud MySQL like AWS RDS, Aiven, PlanetScale)
- **Build Tool:** Maven (wrapper included)
- **Containerization:** Docker

---

## ?? Key Features

- **Role-Based Access Control (RBAC):**
  - `USER`: Register, raise tickets, view personal tickets, reply to open tickets.
  - `STAFF`: View assigned tickets, update ticket progress/status, post internal/external replies.
  - `ADMIN`: Full platform oversight, staff management (create/update/delete staff), reassign tickets, analytics.
- **JWT Authentication:** Secure token generation and filter chain verification with token expiry.
- **RESTful Architecture:** Clear endpoint structuring with standard HTTP response codes and DTO validations.
- **Dynamic CORS Support:** Configurable frontend origin via environment variables.
- **Clean Architecture:** Service-repository pattern with decoupled DTOs and entity models.

---

## ?? Project Structure

```
smartsupport-backend/
+-- src/
¦   +-- main/
¦   ¦   +-- java/com/smartsupport/
¦   ¦   ¦   +-- controller/          # REST API endpoints (Auth, Tickets, Admin, Staff)
¦   ¦   ¦   +-- dto/                 # Request & Response Data Transfer Objects
¦   ¦   ¦   +-- entity/              # JPA Database Entities (User, Ticket, TicketReply)
¦   ¦   ¦   +-- repository/          # Spring Data JPA Repositories
¦   ¦   ¦   +-- security/            # JWT Filter, Token Util, Security Configuration
¦   ¦   ¦   +-- service/             # Business Logic & Service Implementations
¦   ¦   ¦   +-- SmartSupportApplication.java
¦   ¦   +-- resources/
¦   ¦       +-- application.properties # Main application configuration
¦   +-- test/                        # Unit and integration tests
+-- .env.example                     # Sample environment variable template
+-- Dockerfile                       # Container deployment definition
+-- mvnw / mvnw.cmd                  # Maven wrappers for Linux/macOS and Windows
+-- pom.xml                          # Project dependencies and plugins
+-- README.md
```

---

## ??? Prerequisites

Before running the backend locally, ensure you have installed:
1. **Java Development Kit (JDK) 17** or higher
2. **MySQL Server 8+** (or XAMPP / Docker MySQL container)
3. **Maven** (optional, `mvnw` wrapper is included)
4. **Git**

---

## ?? Environment Configuration

The backend reads configuration through environment variables or system properties. 

Create a `.env` file or export the following variables in your terminal:

```properties
# MySQL Database Configuration
DB_URL=jdbc:mysql://localhost:3306/smartsupport?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC
DB_USERNAME=your_mysql_username
DB_PASSWORD=your_mysql_password

# JWT Secret (minimum 256 bits secure random key)
JWT_SECRET=your_jwt_secret_key_minimum_256_bits_long_example_random_string_here

# Frontend Application URL (for CORS)
FRONTEND_URL=http://localhost:5173
```

---

## ?? Local Setup & Running Instructions

### 1. Clone the repository
```bash
git clone https://github.com/vaishnavisonawane893/smartsupport-backend.git
cd smartsupport-backend
```

### 2. Configure Database
Ensure MySQL service is running on your machine:
```sql
CREATE DATABASE IF NOT EXISTS smartsupport;
```

### 3. Build & Run the Application

**Using Maven Wrapper on Windows (PowerShell / CMD):**
```powershell
$env:DB_URL="jdbc:mysql://localhost:3306/smartsupport?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC"
$env:DB_USERNAME="root"
$env:DB_PASSWORD="your_password"
$env:JWT_SECRET="your_secure_random_jwt_secret_key_32_bytes_or_more"
$env:FRONTEND_URL="http://localhost:5173"

.\mvnw.cmd spring-boot:run
```

**Using Maven Wrapper on Linux / macOS:**
```bash
export DB_URL="jdbc:mysql://localhost:3306/smartsupport?createDatabaseIfNotExist=true&useSSL=false&serverTimezone=UTC"
export DB_USERNAME="root"
export DB_PASSWORD="your_password"
export JWT_SECRET="your_secure_random_jwt_secret_key_32_bytes_or_more"
export FRONTEND_URL="http://localhost:5173"

./mvnw spring-boot:run
```

The service will start on `http://localhost:8080`.

---

## ?? API Overview

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/auth/register` | Public | Register new user account |
| `POST` | `/api/auth/login` | Public | Authenticate user & retrieve JWT |
| `GET` | `/api/tickets` | User / Staff / Admin | Fetch user's tickets |
| `POST` | `/api/tickets` | User | Create a new support ticket |
| `GET` | `/api/tickets/{id}` | Authenticated | Get ticket details and conversation thread |
| `POST` | `/api/tickets/{id}/reply` | Authenticated | Post reply message to a ticket |
| `GET` | `/api/admin/tickets` | Admin | View all platform tickets |
| `PUT` | `/api/admin/tickets/{id}/assign` | Admin | Assign ticket to a staff member |
| `GET` | `/api/admin/staff` | Admin | List all staff members |
| `POST` | `/api/admin/staff` | Admin | Add new staff member |
| `GET` | `/api/staff/tickets` | Staff | View tickets assigned to current staff |
| `PUT` | `/api/staff/tickets/{id}/status` | Staff | Update ticket status |

---

## ?? Docker Deployment

To build and run the backend using Docker:

```bash
# Build the Docker image
docker build -t smartsupport-backend .

# Run container with environment variables
docker run -p 8080:8080 \
  -e DB_URL="jdbc:mysql://host.docker.internal:3306/smartsupport?createDatabaseIfNotExist=true" \
  -e DB_USERNAME="root" \
  -e DB_PASSWORD="your_password" \
  -e JWT_SECRET="your_secure_jwt_secret_key_here" \
  -e FRONTEND_URL="http://localhost:5173" \
  smartsupport-backend
```

---

## ????? Author

**Vaishnavi Sonawane**  
GitHub: [@vaishnavisonawane893](https://github.com/vaishnavisonawane893)

---

## ?? License

This project is licensed under the MIT License.
