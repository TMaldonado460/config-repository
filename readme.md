# Centralized configuration folder
## Introduction to Backend - Digital House

For this assignement we will implement reactive programing as means to improve our code consistency and performance. We will use RDC2B with H2 to connect to database.
`Maven:Java:Spring 2.7.2:Jar:Java 17`

### For any proyect:
```
spring:
  application:
    name: ms-configserver
  cloud:
    config:
      server:
        git:
          uri: ${GIT_CONFIG_URI:https://github.com/TMaldonado460/config-repository}
          default-label: ${GIT_DEFAULT_BRANCH:main}
```
#### Required dependencies (Spring initializr):
* Spring Boot DevTools
* Lombok
* Spring Reactive Web
* Spring Data R2DBC
* H2 Database
* Validator
* Spring Boot Actuator
* Eureka Discovery Client (Service registry & Service discovery)
* Config Client (Central configuration)
* OpenFeign (ms connections)
* Cloud LoadBalancer (Load balancer)

### Eureka (ms-discovery):
```
spring:
  application:
    name: ms-discovery
  config:
    import: configserver:${CONFIG_SERVER_URL:http://localhost:8888}
```
#### Required dependencies (Spring initializr):
* Eureka Server
* Config Client (Central configuration)

### Config server (ms-configserver)
```
server:
  port: ${PORT:8888}

spring:
  application:
    name: ms-configserver
  cloud:
    config:
      server:
        git:
          uri: ${GIT_CONFIG_URI:https://github.com/TMaldonado460/config-repository}
          default-label: ${GIT_DEFAULT_BRANCH:main}
```
#### Required dependencies (Spring initializr):
* Config Server

---

### Logging
```
# Logging
logging:
  level:
    io.r2dbc.postgresql.QUERY: DEBUG # for queries
    io.r2dbc.postgresql.PARAM: DEBUG # for parameter
```
### Swagger
```
# Swagger
springdoc:
  swagger-ui:
    path: /swagger-doc/swagger-ui.html
  show-actuator: true
  api-docs:
    path: /swagger-doc/v3/api-docs
    groups:
      enabled: true
```
### Actuator
```
# Actuator
management:
  endpoints:
    web:
      exposure:
        include: info, health, openapi, swagger-ui
  info:
    env:
      enabled: true
  endpoint:
    info:
      enabled: true
    health:
      enabled: true
```
#### Info Properties
```
# info properties
info:
  app:
    name: ${spring.application.name}
    version: 1.0.0
    description: Product microservice
  svc:
    port: ${server.port}
```
### Eureka Config
```
# eureka config
eureka:
  instance:
    hostname: localhost
    instance-id: ${spring.application.name}:${spring.application.instance-id:${random.value}}
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```
