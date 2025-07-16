# Chapter 5: Spring Cloud Overview

## Overview
This lab demonstrates the implementation of service discovery using Spring Cloud Netflix Eureka. You will create a Eureka Server and two microservices that register with it.

## Learning Objectives
- Define what Spring Cloud is and its role in microservices architecture
- Identify core components of the Spring Cloud ecosystem
- Explain how Spring Cloud integrates with Spring Boot
- Implement basic service registration and discovery using Eureka
- Configure centralized application properties
- Monitor and observe services

## Architecture
The system consists of:
- **Eureka Server** (Port 8761): Service discovery server
- **User Service** (Port 8080): Microservice that registers with Eureka
- **Order Service** (Port 8081): Microservice that registers with Eureka

## Project Structure
```
Chapter 5 - Spring Cloud Overview/
├── eureka-server/
│   ├── src/main/java/com/example/eurekaserver/
│   │   └── EurekaServerApplication.java
│   ├── src/main/resources/
│   │   └── application.yml
│   └── pom.xml
├── user-service/
│   ├── src/main/java/com/example/userservice/
│   │   ├── controller/
│   │   │   └── UserController.java
│   │   └── UserServiceApplication.java
│   ├── src/main/resources/
│   │   └── application.yml
│   └── pom.xml
├── order-service/
│   ├── src/main/java/com/example/orderservice/
│   │   ├── controller/
│   │   │   └── OrderController.java
│   │   └── OrderServiceApplication.java
│   ├── src/main/resources/
│   │   └── application.yml
│   └── pom.xml
└── README.md
```

## Setup Instructions

### Prerequisites
- Java 21
- Maven 3.8+
- IDE (IntelliJ IDEA / Eclipse / VS Code)
- Basic knowledge of Spring Boot

### 1. Start Eureka Server
```bash
cd eureka-server
mvn spring-boot:run
```
Access the Eureka dashboard at: http://localhost:8761/

### 2. Start User Service
```bash
cd user-service
mvn spring-boot:run
```
User Service will start on port 8080 and register with Eureka.

### 3. Start Order Service
```bash
cd order-service
mvn spring-boot:run
```
Order Service will start on port 8081 and register with Eureka.

## Testing the Application

### 1. Access Eureka Dashboard
Open your browser and navigate to: http://localhost:8761/

You should see both services registered:
- USER-SERVICE
- ORDER-SERVICE

### 2. Test User Service
```bash
curl http://localhost:8080/users
```
Expected response: "User List from user-service"

### 3. Test Order Service
```bash
curl http://localhost:8081/orders
```
Expected response: "Order List from order-service"

## Key Components

### Eureka Server
- **@EnableEurekaServer**: Enables Eureka Server functionality
- **Configuration**: Disables self-registration and registry fetching
- **Dashboard**: Web interface to view registered services

### Spring Cloud Version
- **Spring Cloud 2023.0.4**: Compatible with Spring Boot 3.2.0 and Java 21
- **Auto-configuration**: Eureka client is automatically enabled when dependency is present

### User Service
- **Automatic Eureka Client**: Service registration is automatically enabled with the dependency
- **application.name**: Logical name for service discovery
- **eureka.client.service-url**: Points to Eureka Server

### Order Service
- Similar configuration to User Service
- Different port (8081) and application name

## Configuration Details

### Eureka Server Configuration
```yaml
server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
```

### Service Configuration
```yaml
spring:
  application:
    name: user-service # or order-service

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

## Expected Behavior

1. **Service Registration**: Both services automatically register with Eureka Server on startup
2. **Service Discovery**: Services can discover each other using their logical names
3. **Health Monitoring**: Eureka monitors service health and removes unhealthy instances
4. **Load Balancing**: Multiple instances of the same service can be registered

## Troubleshooting

### Common Issues
- **Services not appearing in Eureka**: Check that Eureka Server is running and accessible
- **Connection refused**: Verify ports are not in use by other applications
- **Registration failures**: Ensure network connectivity between services and Eureka Server

### Verification Steps
1. Check Eureka dashboard for registered services
2. Verify service logs for registration messages
3. Test individual service endpoints
4. Monitor service health status in Eureka

## Next Steps
This lab provides the foundation for:
- Implementing client-side load balancing
- Adding circuit breakers with Hystrix
- Configuring centralized configuration with Config Server
- Setting up API Gateway with Zuul or Spring Cloud Gateway

## Lessons Learned
- How Spring Cloud simplifies distributed system development
- The role of service discovery in microservices architecture
- How to configure and use Eureka for service registration
- The importance of service naming and configuration
- How to monitor service health and availability 