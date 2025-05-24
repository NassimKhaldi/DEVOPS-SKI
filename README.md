# 🎿 Ski Station Management System

A comprehensive Spring Boot application for managing ski station operations, including skiers, instructors, courses, registrations, subscriptions, and ski slopes (pistes).

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [Docker Deployment](#docker-deployment)
- [CI/CD Pipeline](#cicd-pipeline)
- [Monitoring](#monitoring)
- [Contributing](#contributing)

## 🎯 Overview

This system provides a complete solution for ski station management with the following core functionalities:
- **Skier Management**: Registration, subscription handling, and profile management
- **Instructor Management**: Instructor profiles and course assignments
- **Course Management**: Ski courses with different types and levels
- **Registration System**: Course enrollment with age restrictions and capacity limits
- **Piste Management**: Ski slope information and assignments
- **Subscription System**: Annual, monthly, and semester-based subscriptions

## ✨ Features

### 🏂 Skier Management
- Add and manage skier profiles
- Assign skiers to subscriptions (Annual, Monthly, Semester)
- Assign skiers to ski pistes
- Retrieve skiers by subscription type
- Age-based course restrictions

### 👨‍🏫 Instructor Management
- Instructor profile management
- Course assignment to instructors
- Track instructor teaching history

### 📚 Course Management
- Multiple course types:
  - Individual courses
  - Collective children courses (under 16)
  - Collective adult courses (16+)
- Support for different disciplines (SKI, SNOWBOARD)
- Course capacity management (max 6 participants for collective courses)

### 📝 Registration System
- Smart registration with business rules:
  - Age verification for course types
  - Course capacity checking
  - Duplicate registration prevention
  - Weekly course scheduling

### 🎿 Piste Management
- Ski slope management with color coding (Green, Blue, Red, Black)
- Slope specifications (length, difficulty)
- Skier-piste associations

### 💳 Subscription System
- Flexible subscription types with automatic end date calculation:
  - **Annual**: 1 year duration
  - **Semester**: 6 months duration
  - **Monthly**: 1 month duration

## 🛠 Technology Stack

- **Backend**: Spring Boot 2.6.9
- **Database**: MySQL 5.7
- **ORM**: Spring Data JPA with Hibernate
- **API Documentation**: Swagger/OpenAPI 3
- **Testing**: JUnit 5, Mockito
- **Build Tool**: Maven
- **Containerization**: Docker & Docker Compose
- **CI/CD**: Jenkins
- **Code Quality**: SonarQube
- **Monitoring**: Prometheus & Grafana
- **Java Version**: 8

## 📋 Prerequisites

- Java 8 or higher
- Maven 3.6+
- MySQL 5.7+
- Docker & Docker Compose (optional)

## 🚀 Installation

### Local Development Setup

1. **Clone the repository**   ```bash
   git clone https://github.com/NassimKhaldi/DEVOPS-SKI.git
   cd DEVOPS-SKI
   ```

2. **Configure MySQL Database**
   - Create a MySQL database named `stationSki`
   - Update database credentials in `src/main/resources/application.properties`

3. **Build the application**
   ```bash
   mvn clean install
   ```

4. **Run the application**
   ```bash
   mvn spring-boot:run
   ```

The application will be available at `http://localhost:8089/api`

### Docker Setup

1. **Using Docker Compose (Recommended)**
   ```bash
   docker-compose up -d
   ```

   This will start:
   - MySQL database on port 3306
   - Spring Boot application on port 8089
   - Prometheus on port 9090
   - Grafana on port 3000

2. **Build and run manually**
   ```bash
   mvn clean package -DskipTests
   docker build -t ski-station-app .
   docker run -p 8089:8080 ski-station-app
   ```

## ⚙️ Configuration

### Database Configuration

Update `src/main/resources/application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/stationSki?createDatabaseIfNotExist=true
spring.datasource.username=your_username
spring.datasource.password=your_password

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Application Properties

```properties
# Server Configuration
server.port=8089
server.servlet.context-path=/api

# Logging Configuration
logging.level.root=info
```

## 📖 API Documentation

The API documentation is available via Swagger UI at:
`http://localhost:8089/api/swagger-ui.html`

### Key API Endpoints

#### 🏂 Skier Management
- `POST /api/skier/add` - Add a new skier
- `GET /api/skier/all` - Get all skiers
- `GET /api/skier/get/{id}` - Get skier by ID
- `PUT /api/skier/assignToSub/{skierId}/{subscriptionId}` - Assign subscription
- `PUT /api/skier/assignToPiste/{skierId}/{pisteId}` - Assign to piste

#### 👨‍🏫 Instructor Management
- `POST /api/instructor/add` - Add a new instructor
- `GET /api/instructor/all` - Get all instructors
- `PUT /api/instructor/addAndAssignToCourse/{courseId}` - Add and assign to course

#### 📚 Course Management
- `POST /api/course/add` - Add a new course
- `GET /api/course/all` - Get all courses
- `GET /api/course/get/{id}` - Get course by ID

#### 📝 Registration Management
- `PUT /api/registration/addAndAssignToSkier/{skierId}` - Register skier
- `PUT /api/registration/addAndAssignToSkierAndCourse/{skierId}/{courseId}` - Full registration
- `GET /api/registration/numWeeks/{instructorId}/{support}` - Get instructor's teaching weeks

#### 🎿 Piste Management
- `POST /api/piste/add` - Add a new piste
- `GET /api/piste/all` - Get all pistes
- `DELETE /api/piste/delete/{id}` - Delete piste

## 🧪 Testing

### Run All Tests
```bash
mvn test
```

### Test Coverage
The project includes comprehensive tests for:
- **Repository Layer**: JPA repository methods
- **Service Layer**: Business logic and rules
- **Controller Layer**: REST API endpoints

### Test Configuration
Tests use H2 in-memory database for isolation and speed.

## 🐳 Docker Deployment

### Multi-Service Setup

The `docker-compose.yml` includes:

```yaml
services:
  - MySQL Database (port 3306)
  - Spring Boot Application (port 8089)
  - Prometheus (port 9090)
  - Grafana (port 3000)
```

### Production Deployment
```bash
# Build production image
mvn clean package -DskipTests
docker build -t ski-station:latest .

# Deploy with custom network
docker network create ski-network
docker-compose -f docker-compose.prod.yml up -d
```

## 🔄 CI/CD Pipeline

### Jenkins Pipeline

The project includes a comprehensive Jenkins pipeline (`Jenkinsfile`) with stages:

1. **Git Checkout** - Clone from repository
2. **Maven Build** - Compile and package
3. **Run Tests** - Execute JUnit tests
4. **SonarQube Analysis** - Code quality analysis
5. **Build Docker Image** - Create Docker image
6. **Push to Nexus** - Push to artifact repository
7. **Deploy** - Deploy to target environment

### Pipeline Features
- Automated testing with external MySQL
- Code quality gates with SonarQube
- Docker image management
- Nexus repository integration
- Automated deployment

### Setup Instructions
1. Configure Jenkins with required plugins
2. Set up SonarQube server
3. Configure Nexus repository
4. Create Jenkins credentials for:
   - `scanner` (SonarQube token)
   - `nexus` (Registry credentials)

## 📊 Monitoring

### Prometheus Metrics
- Application metrics collection
- Custom business metrics
- Performance monitoring

### Grafana Dashboards
- Application performance dashboards
- Business metrics visualization
- Alert management

Access monitoring:
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`

## 🔧 Development Guidelines

### Business Rules

1. **Registration Rules**:
   - Children (< 16) can only register for COLLECTIVE_CHILDREN courses
   - Adults (≥ 16) can only register for COLLECTIVE_ADULT or INDIVIDUAL courses
   - Maximum 6 participants per collective course
   - No duplicate registrations for same course and week

2. **Subscription Rules**:
   - Automatic end date calculation based on subscription type
   - One subscription per skier

3. **Course Management**:
   - Individual courses have no participant limit
   - Collective courses limited to 6 participants
   - Age restrictions enforced

### Code Structure
```
src/main/java/tn/esprit/spring/
├── entities/          # JPA entities
├── repositories/      # Data access layer
├── services/          # Business logic layer
├── controllers/       # REST controllers
└── configs/           # Configuration classes
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup
1. Follow coding standards
2. Write comprehensive tests
3. Update documentation
4. Ensure CI/CD pipeline passes

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the API documentation

## 🔄 Version History

- **v1.0** - Initial release with core functionality
  - Skier, Instructor, and Course management
  - Registration system with business rules
  - Subscription management
  - Docker containerization
  - CI/CD pipeline with Jenkins

---

**Happy Skiing! 🎿⛷️**
