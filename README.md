# 🚀 Oryzo — Customer Support System

Oryzo is a premium, multi-tenant customer support and ticketing platform designed for modern enterprise ecosystems. Built on top of **Spring Boot 3.5.4** and **Java 17**, it provides companies with a unified dashboard to manage customer inquiries, support agents, knowledge bases (FAQs), performance reporting, and customer feedback.

---

## 🌟 Key Features

Oryzo is loaded with features covering the entire customer support lifecycle:

### 🏢 Multi-Tenant (Multi-Company) Architecture
* Register separate companies under different subscription tiers (`BASIC`, `PREMIUM`, `ENTERPRISE`).
* Resource allocation limit management (e.g., maximum agents and customers per company).
* Isolated tenant data keeping ticket logs and customers secure.

### 🔐 Role-Based Access Control (RBAC)
Equipped with 4 distinct roles, each mapped to strict security policies:
1. **Super Admin**: System-wide control. Approves system feedback, manages company registrations, views platform-wide metrics, and monitors global audit logs.
2. **Company Admin**: Organization-level control. Registers support agents, manages user roles, generates performance reports, and configures company-specific rules.
3. **Support Agent**: Handles assigned tickets, logs internal notes, searches the FAQ database, and answers queries.
4. **Customer**: Submits support tickets, manages active requests, views knowledge base articles, votes on FAQ helpfulness, and publishes reviews.

### 🎫 Advanced Ticketing Engine
* Custom-formatted tickets with auto-generated ticket numbers (e.g., `TKT-YYYYMMDD-[ID]`).
* Ticket priorities (`LOW`, `MEDIUM`, `HIGH`, `URGENT`) and statuses (`OPEN`, `IN_PROGRESS`, `PENDING_CUSTOMER`, `RESOLVED`, `CLOSED`, `CANCELLED`).
* Interactive discussion threads support between customers and support agents with attachment capabilities.

### 📚 Self-Service Knowledge Base (FAQ)
* Searchable FAQs mapping keywords and categories.
* Customer helpfulness voting (`Helpful` / `Not Helpful`) to refine self-service quality.
* Automated database triggers recalculate average customer satisfaction scores.

### 📩 Notification & Template Engine
* Built-in SMTP email support (Gmail integrated) for automated ticket assignments, resolution updates, and user registration.
* HTML-rich email templates styled dynamically using **Thymeleaf**.

### 📊 Platform Analytics & PDF Reporting
* Real-time performance dashboards tracking average resolution times, open ticket queues, and agent ratings.
* Export feature supporting **PDF generation** of invoices or performance reports (powered by `openhtmltopdf-pdfbox`).

---

## 🛠️ Technology Stack

* **Backend Framework**: Spring Boot 3.5.4
* **Security & Authentication**: Spring Security, JJWT (JSON Web Token) for stateless authentication
* **Database**: MySQL 8.0+ (utilizes custom DB triggers to maintain integrity/scores)
* **Data Access**: Spring Data JPA & Hibernate ORM
* **Frontend Template Engine**: Thymeleaf (serving HTML, CSS, & interactive JS dashboards)
* **Build Tool**: Apache Maven (wrapper included)

---

## 📂 Project Structure

```text
customer-support-system/
├── src/
│   ├── main/
│   │   ├── java/com/customersupport/
│   │   │   ├── config/          # JWT, CORS, Database & Mail Configurations
│   │   │   ├── controller/      # REST API & MVC Web Controllers
│   │   │   ├── dto/             # Data Transfer Objects (Requests/Responses)
│   │   │   ├── entity/          # JPA Hibernate Entity Models (Company, User, Ticket...)
│   │   │   ├── exception/       # Centralized Exception Handling
│   │   │   ├── repository/      # Spring Data JPA Database Interfaces
│   │   │   ├── service/         # Core Business Logic Implementation
│   │   │   └── util/            # Utilities (JWT helpers, dates, etc.)
│   │   └── resources/
│   │       ├── templates/       # Thymeleaf HTML views for Admins, Agents, and Customers
│   │       ├── static/          # Static Web Assets (CSS, JS, Fonts, index.html)
│   │       └── application.properties # Global Application Configurations
│   └── test/                    # JUnit and TestNG Unit Tests
├── project.sql                  # Database Schema, Triggers, and Seed Scripts
├── pom.xml                      # Maven Configuration File
└── README.md                    # Project Documentation
```

---

## 🚀 Getting Started

### 📋 Prerequisites
Ensure you have the following installed on your local environment:
* **Java Development Kit (JDK) 17**
* **MySQL Server 8.0+**
* **Apache Maven** (Optional, `mvnw` included)

---

### ⚙️ Installation & Configuration

#### Step 1: Initialize the MySQL Database
1. Open your MySQL client and create a database named `customer_support_system`:
   ```sql
   CREATE DATABASE customer_support_system;
   ```
2. Import the database schema, table structures, relationships, triggers, and sample seeds using the provided `project.sql` file:
   ```bash
   mysql -u your_username -p customer_support_system < project.sql
   ```

#### Step 2: Configure Environment Properties
Navigate to `src/main/resources/application.properties` and verify/update the database credentials:
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/customer_support_system?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
spring.datasource.username=your_mysql_user
spring.datasource.password=your_mysql_password
```
*(Optional)* If you plan to test email delivery, verify the mail configuration:
```properties
spring.mail.username=your_email@gmail.com
spring.mail.password=your_app_password
```

> [!WARNING]
> The database property `spring.jpa.hibernate.ddl-auto` is set to `validate`. This means the MySQL database schema and initial tables **must** be imported via the `project.sql` script *before* the application starts. Otherwise, the application will throw a validation error on start.

---

### 🏃 Running the Application

1. **Build the project** using Maven:
   ```bash
   ./mvnw clean install
   ```
2. **Start the application**:
   ```bash
   ./mvnw spring-boot:run
   ```
3. Once running, access the web client at:
   ```text
   http://localhost:8082
   ```

---

## 🔑 Access Credentials & Testing

### Default Super Administrator
An initial super admin account is seed-populated in `project.sql` to get you up and running:
* **Username**: `superadmin@system.com`
* **Temporary Password Reset**:
  To reset/retrieve the password for testing, you can trigger the email-free password reset endpoint:
  ```http
  POST http://localhost:8082/api/test/reset-password?email=superadmin@system.com
  ```
  This will print the updated login password in the API response.

### Testing Endpoints
A dedicated test controller is available for verifying service status:
* Check application test ping:
  ```http
  GET http://localhost:8082/api/test
  ```

---

## 🛡️ License

Oryzo is proprietary software. All rights reserved.
