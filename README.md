# Customer Support System - FAQ Service

[![Java Version](https://img.shields.io/badge/Java-17-orange.svg?style=flat-square)](https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.4-brightgreen.svg?style=flat-square)](https://spring.io/projects/spring-boot)
[![Maven](https://img.shields.io/badge/Maven-3.x-blue.svg?style=flat-square)](https://maven.apache.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)

A robust backend REST API built using **Spring Boot 3.5.x** and **Java 17** to manage Frequently Asked Questions (FAQs) for multiple companies. The service features categorizations, rich keyword searching, view tracking, and user helpfulness feedback (voting system).

---

## 🚀 Key Features

*   **Multitenant Structure**: FAQs are segmented by `Company`.
*   **Hierarchical Categorization**: Support for grouping FAQs under specific categories.
*   **Search Engine**: Rich keyword-based searching across questions, answers, and tags.
*   **FAQ Metrics & Analytics**:
    *   Automatic **view tracking** to gauge interest.
    *   **Helpfulness Voting**: Users can mark FAQs as helpful or not helpful, generating a real-time helpfulness score.
*   **Content Filtering**:
    *   *Featured* FAQs for pinning popular resources.
    *   *Published* vs *Draft* state toggle for content moderation.
    *   List sorting based on customizable priority ordering.
*   **Security Stack**: Preconfigured dependency structure for Spring Security and JJWT authentication.

---

## 🛠️ Technology Stack

*   **Core Framework**: Spring Boot 3.5.4 (Starter Web, Starter Validation, Starter Mail, Thymeleaf)
*   **Persistence**: Spring Data JPA (Hibernate)
*   **Database**: MySQL Connector/J
*   **Authentication & Security**: JJWT (JSON Web Token) API & Impl (0.11.5)
*   **Developer Tooling**: Project Lombok (code generation), Apache Commons Lang3
*   **Testing**: TestNG & JUnit (Starter Test)

---

## 📂 Project Structure

```text
src/main/java/com/customersupport/
├── CustomerSupportSystemApplication.java   # Application entry point
├── controller/
│   ├── FaqController.java                  # FAQ REST endpoints
│   └── SuperAdminController.java           # Super admin dashboard (Placeholder)
├── service/
│   ├── FaqService.java                     # FAQ business logic
│   └── CompanyService.java                 # Company business logic (Placeholder)
├── repository/
│   ├── FaqRepository.java                  # JPA Database queries for FAQ
│   └── CompanyRepository.java              # JPA Database queries for Company (Placeholder)
├── entity/
│   ├── Faq.java                            # Core FAQ database entity
│   ├── FaqVoting.java                      # Helpful/not helpful record tracking
│   └── Company.java                        # Corporate entity structure (Placeholder)
└── dto/
    ├── FaqDTO.java                         # FAQ Data Transfer Object
    └── CompanyDTO.java                     # Company Data Transfer Object (Placeholder)
```

> [!NOTE]  
> This project contains placeholders for `User`, `Category`, `UserRepository`, and `CategoryRepository`. To fully compile and run this application, these entities and repositories must be implemented or imported from a shared authentication/user module.

---

## 🔌 API Endpoints

### FAQ Management (`/api/faqs`)

All endpoints support Cross-Origin Resource Sharing (`@CrossOrigin(origins = "*")`).

| HTTP Method | Endpoint | Description | Query Parameters |
| :--- | :--- | :--- | :--- |
| **POST** | `/api/faqs` | Create a new FAQ entry | `createdById` (Long) |
| **GET** | `/api/faqs/{id}` | Retrieve details of a specific FAQ | None |
| **PUT** | `/api/faqs/{id}` | Update an existing FAQ entry | `updatedById` (Long) |
| **DELETE** | `/api/faqs/{id}` | Delete a specific FAQ | None |
| **GET** | `/api/faqs/company/{companyId}` | Get all FAQs for a company ordered by priority | None |
| **GET** | `/api/faqs/company/{companyId}/published` | Get all published FAQs for a company | None |
| **GET** | `/api/faqs/company/{companyId}/featured` | Get featured FAQs for a company | None |
| **GET** | `/api/faqs/category/{categoryId}` | Get FAQs under a specific category | None |
| **GET** | `/api/faqs/company/{companyId}/search` | Search all FAQs for a company | `keyword` (String) |
| **GET** | `/api/faqs/company/{companyId}/published/search`| Search published FAQs for a company | `keyword` (String) |
| **GET** | `/api/faqs/company/{companyId}/most-viewed` | Get most viewed FAQs | `limit` (int, default=5) |
| **GET** | `/api/faqs/company/{companyId}/most-helpful` | Get most helpful FAQs (by helpful count) | `limit` (int, default=5) |
| **GET** | `/api/faqs/company/{companyId}/recent` | Get recently created FAQs | `limit` (int, default=5) |
| **POST** | `/api/faqs/{id}/view` | Increment view counter by 1 | None |
| **POST** | `/api/faqs/{id}/helpful` | Increment "Helpful" vote by 1 | None |
| **POST** | `/api/faqs/{id}/not-helpful` | Increment "Not Helpful" vote by 1 | None |

---

## ⚙️ Configuration

Set up your database connection and mail servers inside `src/main/resources/application.properties`:

```properties
spring.application.name=customer-support-system

# Database Configurations
spring.datasource.url=jdbc:mysql://localhost:3606/customer_support?useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=your_mysql_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA/Hibernate Configurations
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
```

---

## 🏃 Getting Started

### Prerequisites
*   **Java Development Kit (JDK) 17** or higher
*   **MySQL Server** running locally or remotely
*   **Maven 3.x** (or use the provided Maven Wrapper `./mvnw`)

### Build & Run
1.  **Clone and Navigate to Project Directory**:
    ```bash
    cd Customer-Support-System
    ```
2.  **Configure Environment**:
    Update `src/main/resources/application.properties` with your database URL, username, and password.
3.  **Build the Project**:
    ```bash
    ./mvnw clean package -DskipTests
    ```
4.  **Run the Application**:
    ```bash
    ./mvnw spring-boot:run
    ```
    The application will default to running on `http://localhost:8080`.
