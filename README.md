# Greenfield Application Development

## Objective
Build a new enterprise-ready application from scratch using modern technologies and best practices.

## Functional Flow
1. A **Spring Boot Data Synchronization Service** consumes employee data from the public REST endpoint:
```text
https://jsonplaceholder.typicode.com/users
```
2. Employee data is synchronized and stored in **PostgreSQL**.
3. A separate **Spring Boot Employee Management API** exposes secure CRUD operations for employee records.
4. **Keycloak** handles authentication and authorization.
5. A modern frontend built with **Next.js** consumes secured APIs.

<img width="857" height="381" alt="image" src="https://github.com/user-attachments/assets/085a265d-ae3c-4e1c-baa8-bcff7b7027d7" />

---
## Architecture Overview
### Data Synchronization Service
* Standalone Spring Boot application
* Scheduled CronJob/Listener
* Consumes external REST API
* Synchronizes employee data into PostgreSQL
* No authentication required
### Employee Management API
* Separate Spring Boot application
* Full CRUD operations
* Employee search capabilities
* PostgreSQL integration
* Secured endpoints
### Security Layer
* Keycloak authentication
* JWT authorization
* Role-based access control
### Frontend Application
Built using:
* Next.js
* React
* TypeScript
* Axios
* Tailwind CSS
* shadcn/ui
  
Frontend capabilities:
* Login page
* Dashboard
* Employee table
* Search employees
* Create employees
* Update employees
* Delete employees
* Responsive design for desktop, tablet, and mobile devices

---

# Target Capabilities / Expected Outcomes
## Backend Architecture
### Data Synchronization Service
Standalone **Spring Boot** application responsible for:
* Consuming employee data from the public REST endpoint:
```text
https://jsonplaceholder.typicode.com/users
```
* Synchronizing employee data into PostgreSQL
* Running scheduled CronJobs/listeners
* Transforming existing MySQL scripts into PostgreSQL-compatible scripts
```text

  CREATE TABLE employee (
    id INT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(150),
    username VARCHAR(100),
    age INT,
    gender VARCHAR(20)
  );
```
* No authentication required (internal synchronization process only)
## Synchronization Validation Rules
The synchronization process must validate employee records before persisting data:
- If an employee email does not exist in PostgreSQL → create a new employee record
- If an employee email already exists → update the existing employee information
- If an employee record exists in PostgreSQL but no longer exists in the external REST service → delete the employee record from the database

This ensures PostgreSQL remains fully synchronized with the external data source.
---
### Employee Management API
Separate **Spring Boot** application responsible for:
* Full CRUD operations for employee records
* Employee search functionality
* Database interaction with PostgreSQL
* Secured endpoints using Keycloak
* JWT-based authentication and authorization
* Role-based access control
* API documentation using OpenAPI standards
### Backend Configuration Management
All backend configuration values must be externalized using `application.yaml`.
Examples include:
* Database connection properties
* Keycloak configuration
* OpenAPI/Swagger UI configuration
* Server ports
* Environment variables
* External service endpoints
No configuration values should be hardcoded in the backend source code.

---

## Roles & Permissions
### USER Role
Can:
* Search employee records
### ADMIN Role
Can:
* Create employee records
* Update employee records
* Delete employee records

---

## Testing Layer
Implement automated backend testing using **JUnit** to validate business logic:

* Create employee tests
* Search employee tests
* Update employee tests
* Delete employee tests
* Service-layer validation tests

### Testing Scope

* Validate CRUD functionality
* Test service layer methods
* Authentication/authorization testing is out of scope

---

## Frontend Requirements

The frontend application must include:

* A custom application login page (users should authenticate directly within the application UI)
* Integration with Keycloak authentication services behind the scenes
* JWT token consumption for secured API requests
* Dashboard interface
* Employee management table
* Search functionality
* CRUD actions
* Responsive design for desktop, tablet, and mobile devices
* Modern UX/UI practices

### Frontend Configuration Management

All frontend configuration values must be externalized and managed through a `.env` file.

Examples include:

* API base URLs
* Keycloak configuration values
* Environment-specific settings
* Frontend application URLs

No configuration values should be hardcoded in the frontend source code.

---

## DevOps / Deployment

### Easy Deployment Strategy

Adopt a Docker-first deployment model where every component is fully containerized.

Included services:

* Data Synchronization Backend Application
* Employee Management API
* Frontend Application
* PostgreSQL
* Keycloak

Each service must include its own Docker configuration.

### Deployment Goal

Enable fast environment provisioning and application testing through a single Docker Compose configuration.

```bash
docker-compose up
```

This approach allows reviewers and developers to quickly deploy, validate, and test the entire platform with minimal setup effort.

---

## Test Credentials & Environment Access

Include a `test-credentials.md` file in the root folder containing:

* PostgreSQL credentials
* Keycloak admin credentials
* Test user credentials (`USER` role)
* Test admin credentials (`ADMIN` role)
* Application URLs/endpoints

This ensures reviewers can quickly validate the complete solution.
