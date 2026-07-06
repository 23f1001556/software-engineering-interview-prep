# The Master Spring Boot Interview Prep Guide
## Enterprise Java Development for Flask Developers

This guide is designed for developers who know **Flask (Python)** and are preparing for **Spring Boot (Java)** backend interviews. It contains a **Spring Boot to Flask Glossary**, an **Annotations Reference Dictionary**, a **Directory Structure Guide**, and all code files for a **Complete Product API Demo Project** (fully resolved with validation, enums, and field mappings).

---

## 📂 Table of Contents
1. [Spring Boot to Flask Concept Glossary](#spring-boot-to-flask-concept-glossary)
2. [Spring Boot Annotations Reference Dictionary](#spring-boot-annotations-reference-dictionary)
3. [Spring Boot Project Directory Structure](#spring-boot-project-directory-structure)
4. [The Complete Demo Project: Product Management REST API (With Fields Mapping)](#the-complete-demo-project-product-management-rest-api-with-fields-mapping)
    *   [End-to-End API Data Flow Walkthrough](#end-to-end-api-data-flow-walkthrough)
    *   [File 1: Build Configuration (`pom.xml`)](#file-1-build-configuration-pomxml)
    *   [File 2: Application Properties (`application.yml`)](#file-2-application-properties-applicationyml)
    *   [File 3: Category Enumeration (`ProductCategory.java`)](#file-3-category-enumeration-productcategoryjava)
    *   [File 4: Database Entity (`Product.java`)](#file-4-database-entity-productjava)
    *   [File 5: Database Repository (`ProductRepository.java`)](#file-5-database-repository-productrepositoryjava)
    *   [File 6: Input Validation DTO (`ProductDto.java`)](#file-6-input-validation-dto-productdtojava)
    *   [File 7: Business Service (`ProductService.java`)](#file-7-business-service-productservicejava)
    *   [File 8: HTTP Controller (`ProductController.java`)](#file-8-http-controller-productcontrollerjava)
    *   [File 9: Exception Handler (`GlobalExceptionHandler.java`)](#file-9-exception-handler-globalexceptionhandlerjava)
    *   [File 10: Application Entry Point (`DemoApplication.java`)](#file-10-application-entry-point-demoapplicationjava)
5. [30 High-Frequency Spring Boot Questions & Answers](#30-high-frequency-spring-boot-questions--answers)
6. [Module 17: The N+1 Query Problem & Optimization Solutions](#module-17-the-n1-query-problem--optimization-solutions)

---

## Spring Boot to Flask Concept Glossary

Use this glossary to study basic, intermediate, and advanced Spring Boot and Java Enterprise definitions in detail:

*   **Inversion of Control (IoC) Container:** The core engine of the Spring Framework. It manages the lifecycle, configuration, instantiation, and assembly of application classes (Beans). Instead of the developer manually writing instantiation routines (`new MyService()`), the IoC container instantiates objects during startup, stores them in an application registry context, and coordinates their usage.
    *   *Flask Contrast:* Flask does not have an IoC container. To use another module, you import the python file and instantiate classes or database clients manually inside your scripts.
*   **Spring Bean:** An object that is instantiated, assembled, and managed by the Spring IoC Container. Beans are typically singletons by default (meaning only one instance of the object is created and shared across the entire application thread pool) to optimize memory consumption and execution speed.
*   **Dependency Injection (DI):** The architectural design pattern used by Spring to implement Inversion of Control. Instead of a class manually initializing its dependencies, the Spring container injects the required dependency Beans automatically via constructors or setters. This decouples classes, making code modular and unit testing with mock objects straightforward.
    *   *Flask Contrast:* If you need a database context inside a helper function, you import the global `db` variable or manually pass the database connection client down as a function argument.
*   **HikariCP (Hikari Connection Pool):** An extremely fast, light, and optimized JDBC database connection pool. Establishing a new database socket connection requires a TCP handshake and database authentication, which is highly resource-intensive and slow if executed on every HTTP request. Hikari CP starts a configured number of active database sockets at startup and maintains them in memory. When a route executes a query, it leases a connection from the pool instantly and returns it on query completion. Spring Boot uses HikariCP by default because of micro-optimizations (like bytecode generation and custom collections to minimize thread contention).
*   **Jakarta EE (Formerly Java EE):** A set of standardized enterprise specifications (e.g. JPA for ORM, Servlets for HTTP handling, Validation for inputs) managed by the Eclipse Foundation. Previously, these packages were managed by Sun/Oracle under the trademarked `javax.*` namespace. Due to licensing transfers, Spring Boot 3 has migrated to the open-source `jakarta.*` namespace. Consequently, all imports for database mapping and validation use package paths like `jakarta.persistence.*` instead of the legacy `javax.persistence.*`.
*   **Hibernate:** An open-source Object-Relational Mapping (ORM) framework for Java. Hibernate is the concrete implementation engine that maps Java classes to database tables and handles object state tracking, caching, and SQL generation. It complies with the JPA standard specifications.
    *   *Flask Equivalent:* SQLAlchemy. Like SQLAlchemy, Hibernate tracks object state modifications and updates database records automatically.
*   **Spring Data JPA:** A library that sits on top of Hibernate (JPA). It provides a repository abstraction layer. By defining a repository interface that extends `JpaRepository`, Spring Data JPA generates database queries and CRUD sql operations (saving, deleting, finding) automatically at runtime based on the method names you declare, eliminating raw SQL query coding.
*   **`@Autowired`:** An annotation that instructs the Spring IoC Container to resolve and inject a matching Bean dependency into a class. Spring automatically executes this binding during startup.
*   **`@Component`:** The fundamental stereotype annotation in Spring. It marks a Java class as a managed component. When Spring scans the project during bootstrapping, it identifies classes annotated with `@Component`, instantiates them, and registers them as Beans inside the IoC Container.
*   **`@Service`:** A semantic specialization of `@Component`. It registers a class as a Bean specifically inside the business-logic layer, separating controllers from raw database clients.
    *   *Flask Equivalent:* Separate business-logic helper files (e.g. `user_logic.py`).
*   **`@Repository`:** A semantic specialization of `@Component` mapping database access structures. It handles database communication exceptions and translates vendor-specific SQL errors into Spring's unified database exception hierarchy.
*   **`@RestController`:** Combines the `@Controller` and `@ResponseBody` annotations. It registers a class as an API endpoint router. Instead of rendering HTML templates on the server side, it instructs Spring to serialize returned Java objects directly into HTTP response payloads (typically JSON).
    *   *Flask Equivalent:* A Flask Blueprint instance containing JSON API routing decorators.
*   **`@Transactional`:** An annotation that defines transaction boundaries. When placed on a method, Spring uses a dynamic proxy to wrap the execution inside a database transaction block automatically. If the method runs successfully, the transaction commits. If the method throws a RuntimeException, the transaction rolls back immediately, guaranteeing database consistency.
    *   *Flask Contrast:* In Flask, you write manual transactional try-except blocks:
        ```python
        try:
            # perform database modifications...
            db.session.commit()
        except:
            db.session.rollback()
        ```
*   **`@RestControllerAdvice`:** A specialized `@Component` class using Aspect-Oriented Programming (AOP). It intercepts exceptions thrown by any HTTP controller route, allowing you to format error trace stacks into clean JSON payloads with proper status codes.
    *   *Flask Equivalent:* Registering global error handler decorators with `@app.errorhandler`.
*   **Spring Security Filter Chain:** A sequence of servlet filters that intercept every incoming HTTP request. Before reaching the route controller, requests must pass through these filters to check security concerns like CORS, JWT signature validations, request rate limits, and user role authorizations.
    *   *Flask Equivalent:* A series of `@app.before_request` decorators.
*   **Maven / Gradle:** Build automation tools for Java projects. They manage external dependency downloads, verify package compatibility, compile Java files to bytecode, execute test suites, and package applications into executable JAR files.
    *   *Flask Equivalent:* `pip` package manager combined with a `requirements.txt` file.

---

## Spring Boot Annotations Reference Dictionary

Use this dictionary to review exactly what each annotation does, how it functions under the hood, and how it maps to your Flask concepts:

*   **`@SpringBootApplication`**
    *   *What it does:* Bootstraps the entire application. It triggers package component scanning, configuration loading, and auto-config setup.
    *   *Mechanism:* A compound annotation that wraps:
        1. `@SpringBootConfiguration`: Registers configuration Beans.
        2. `@EnableAutoConfiguration`: Automatically configures settings based on classes available on your classpath.
        3. `@ComponentScan`: Scans the parent package and sub-packages to register classes marked with `@Component`.
    *   *Flask Equivalent:* Instantiating the core application object: `app = Flask(__name__)`.
*   **`@Component`**
    *   *What it does:* Marks a class as a Spring Bean to be managed by the IoC Container.
    *   *Mechanism:* Spring's scanner finds classes with this annotation, instantiates a single instance (Singleton by default), and stores it in the IoC context.
    *   *Flask Equivalent:* A global instance of a utility module.
*   **`@Service`**
    *   *What it does:* Specialization of `@Component` that designates the class as a business-logic layer.
    *   *Flask Equivalent:* Business helper modules (e.g. `services.py`).
*   **`@Repository`**
    *   *What it does:* Specialization of `@Component` designating the class as a database-access layer.
    *   *Mechanism:* In addition to registering the bean, it automatically translates raw SQL exceptions into Spring's unified data access exception hierarchy.
    *   *Flask Equivalent:* Database helper files (e.g. `db_helpers.py`).
*   **`@RestController`**
    *   *What it does:* Registers a class as a REST endpoint handler.
    *   *Mechanism:* Automatically serializes returned objects into JSON response payloads by combining `@Controller` and `@ResponseBody`.
    *   *Flask Equivalent:* Flask `Blueprint` instance.
*   **`@RequestMapping`**
    *   *What it does:* Defines a base URL prefix pattern at the class level.
    *   *Flask Equivalent:* `url_prefix` parameter in Flask Blueprints.
*   **`@GetMapping`**
    *   *What it does:* Maps HTTP GET queries to a method.
    *   *Flask Equivalent:* `@app.route('/path', methods=['GET'])`.
*   **`@PostMapping`**
    *   *What it does:* Maps HTTP POST queries to a method.
    *   *Flask Equivalent:* `@app.route('/path', methods=['POST'])`.
*   **`@PutMapping`**
    *   *What it does:* Maps HTTP PUT queries to a method.
    *   *Flask Equivalent:* `@app.route('/path', methods=['PUT'])`.
*   **`@DeleteMapping`**
    *   *What it does:* Maps HTTP DELETE queries to a method.
    *   *Flask Equivalent:* `@app.route('/path', methods=['DELETE'])`.
*   **`@PathVariable`**
    *   *What it does:* Extracts dynamic values from the URL path.
    *   *Flask Equivalent:* `<int:variable>` inside path patterns.
*   **`@RequestParam`**
    *   *What it does:* Extracts URL query parameters.
    *   *Flask Equivalent:* `request.args.get('param')`.
*   **`@RequestBody`**
    *   *What it does:* Deserializes JSON payloads to Java objects.
    *   *Flask Equivalent:* `request.get_json()`.
*   **`@Autowired`**
    *   *What it does:* Dynamically injects dependencies from the container.
    *   *Flask Equivalent:* Importing and passing client instances to functions.
*   **`@Value`**
    *   *What it does:* Injects properties values from configuration files.
    *   *Flask Equivalent:* `os.getenv('PROPERTY_NAME')`.
*   **`@Configuration`**
    *   *What it does:* Defines classes containing Bean declaration setups.
    *   *Flask Equivalent:* Initialization settings files.
*   **`@Bean`**
    *   *What it does:* Declares a method whose return value is registered as a Bean.
    *   *Flask Equivalent:* Creating a global variable instance (e.g. `db = SQLAlchemy()`).
*   **`@Entity`**
    *   *What it does:* Maps a Java class to a relational database table.
    *   *Flask Equivalent:* SQLAlchemy Model class (`class User(db.Model)`).
*   **`@Table`**
    *   *What it does:* Specifies the database table name config.
    *   *Flask Equivalent:* `__tablename__ = 'users'` in SQLAlchemy.
*   **`@Id`**
    *   *What it does:* Marks a field as the primary key of the table.
    *   *Flask Equivalent:* `primary_key=True` in SQLAlchemy Column definitions.
*   **`@GeneratedValue`**
    *   *What it does:* Configures auto-incrementing identity keys.
    *   *Flask Equivalent:* `db.Column(db.Integer, primary_key=True)` (default behavior).
*   **`@Column`**
    *   *What it does:* Defines database field properties (nullable, length).
    *   *Flask Equivalent:* `db.Column(db.String(80), nullable=False)`.
*   **`@Enumerated`**
    *   *What it does:* Configures how Enum fields are saved in the database.
    *   *Mechanism:* `@Enumerated(EnumType.STRING)` saves values as text strings (e.g. `"ELECTRONICS"`). `@Enumerated(EnumType.ORDINAL)` saves values as indexes (e.g. `0`, `1`), which is fragile if enums change order.
*   **`@Transactional`**
    *   *What it does:* Wraps a method in an automated transaction context.
    *   *Mechanism:* Intercepts calls using dynamic proxies to run commit/rollback operations automatically.
    *   *Flask Equivalent:* Manual transaction blocks with `try...except` calling `db.session.commit()` and `db.session.rollback()`.
*   **`@RestControllerAdvice`**
    *   *What it does:* Centralizes global exception handlers across controllers.
    *   *Flask Equivalent:* Global `@app.errorhandler()` decorators.
*   **`@ExceptionHandler`**
    *   *What it does:* Maps a specific Java exception class to a handler method.
    *   *Flask Equivalent:* `@app.errorhandler(ResourceNotFoundException)`.
*   **`@Valid`**
    *   *What it does:* Triggers input verification on incoming DTO objects.
    *   *Flask Equivalent:* Invoking Marshmallow `schema.load()`.
*   **`@Async`**
    *   *What it does:* Runs a method in a separate thread asynchronously.
    *   *Flask Equivalent:* Spawning a Celery background job (`task.delay()`).

---

## Spring Boot Project Directory Structure

Unlike Python projects which have highly flexible structures, Java applications compiled with Maven or Gradle follow a strict, standardized directory layout:

```
my-spring-project/
├── pom.xml                                 # Maven Build Configuration file (equivalent to requirements.txt + setup.py)
├── src/
│   ├── main/
│   │   ├── java/                           # Root folder for Java source code
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── demo/
│   │   │               ├── DemoApplication.java   # App Entry Point (Main bootstrap class)
│   │   │               ├── controller/            # HTTP Routers (like Flask Blueprints)
│   │   │               ├── service/               # Business Logic Layer
│   │   │               ├── repository/            # Database Query Interfaces (JPA repositories)
│   │   │               ├── model/                 # JPA Entities (SQL tables)
│   │   │               ├── dto/                   # Data Transfer Objects (Request/Response layouts)
│   │   │               └── exception/             # Global error interceptors
│   │   └── resources/                      # Non-java resources
│   │       ├── application.yml             # Global configuration file (equivalent to .env files)
│   │       ├── static/                     # Static assets (images, css, js)
│   │       └── templates/                  # Server-rendered templates (like Jinja2 templates)
│   └── test/
│       └── java/                           # Directory containing unit/integration test files
```

---

## The Complete Demo Project: Product Management REST API (With Fields Mapping)

This is a complete, working Spring Boot microservice. It provides a REST API to Create, Read, and Delete products, using an **H2 In-Memory Database** (ideal for interviews/tests as it requires zero local database installation).

This project incorporates:
1.  **Field Validation:** Checks character lengths and decimal values.
2.  **Enum Mapping:** Standardizes categories using a Java `enum` (`ProductCategory`).
3.  **Data Flow Architecture:** Demonstrates how data properties are parsed, validated, mapped, and queried through layers.

---

### End-to-End API Data Flow Walkthrough

```
[ Client Request: HTTP POST /api/products ]
  Format: JSON { "name": "Laptop", "price": 999.99, "description": "Fast Laptop", "category": "ELECTRONICS" }
       │
       ▼
[ Spring Security Filter Chain ]  --> Validates CORS headers and JWT tokens.
       │
       ▼
[ ProductController.addProduct() ] --> Maps JSON keys to Java ProductDto properties.
       │
       ▼
[ Validation: Hibernate Validator ] --> Intercepts properties checking `@NotBlank`, `@Size`, and `@NotNull`.
       │  (If invalid: Triggers exception -> GlobalExceptionHandler formats 422 JSON response)
       ▼
[ ProductService.createProduct() ]  --> Starts a Database `@Transactional` block.
       │                                Maps validated DTO fields to new Product Entity.
       ▼
[ ProductRepository.save() ]      --> JPA translates java Object changes to native database SQL.
       │                                Query: "INSERT INTO products (name, price, desc, cat) VALUES (?, ?, ?, ?);"
       ▼
[ H2 SQL Database (In-Memory) ]   --> Records product row, generates primary ID, and confirms writes.
       │
       ▼
[ Return Response Pipeline ]      --> Transaction commits. Service returns saved Entity. Controller wraps
                                        Entity inside ResponseEntity. Spring serializes Java object to HTTP JSON 
                                        response with HTTP Status 201 Created.
```

---

### File 1: Build Configuration (`pom.xml`)
The `pom.xml` handles dependency installations and compilation tasks (equivalent to Python's `requirements.txt`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.2</version>
        <relativePath/> 
    </parent>

    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Product Demo API</name>
    <description>Demo Project for Product Management</description>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <!-- Spring Web: HTTP Controllers, Routing, and Embedded Tomcat Server -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Data JPA: Hibernate ORM database layer -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- H2 Database: In-memory relational SQL database (requires zero local setup) -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Validation: Validation constraints for request body DTO parameters -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>

        <!-- Testing: JUnit 5 & Mockito slice testing tools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

### File 2: Application Properties (`application.yml`)
Configures the server port, database connections, and enables H2 developer consoles (equivalent to `.env` setups).

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:h2:mem:productdb;DB_CLOSE_DELAY=-1 # In-memory database URL
    driver-class-name: org.h2.Driver
    username: sa
    password: password
  h2:
    console:
      enabled: true # Web console at: http://localhost:8080/h2-console
  jpa:
    hibernate:
      ddl-auto: update # Automatically creates tables matching Entity models
    show-sql: true # Log raw SQL executions to console
```

---

### File 3: Category Enumeration (`ProductCategory.java`)
Defines structured Enum values.
*   *Flask Equivalent:* Standard Python `enum.Enum` declarations.

```java
// src/main/java/com/example/demo/model/ProductCategory.java
package com.example.demo.model;

public enum ProductCategory {
    ELECTRONICS,
    CLOTHING,
    HOME,
    BOOKS
}
```

---

### File 4: Database Entity (`Product.java`)
Represents the database schema model (equivalent to a SQLAlchemy model inheriting `db.Model`).

```java
package com.example.demo.model;

import jakarta.persistence.*;
import java.math.BigDecimal;

@Entity // Registers this class as a database entity table
@Table(name = "products") // Name of SQL table
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // Auto-incrementing primary key
    private Long id;

    @Column(nullable = false, length = 100) // SQL constraints
    private String name;

    @Column(nullable = false)
    private BigDecimal price;

    @Column(nullable = false, length = 255) // String field mapped to products table
    private String description;

    @Enumerated(EnumType.STRING) // Saves Enum values as text in database (not numbers)
    @Column(nullable = false, length = 50)
    private ProductCategory category;

    // Default constructor (required by Hibernate)
    public Product() {}

    public Product(String name, BigDecimal price, String description, ProductCategory category) {
        this.name = name;
        this.price = price;
        this.description = description;
        this.category = category;
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public BigDecimal getPrice() { return price; }
    public void setPrice(BigDecimal price) { this.price = price; }
    public String getDescription() { return description; }
    public void setDescription(String description) { this.description = description; }
    public ProductCategory getCategory() { return category; }
    public void setCategory(ProductCategory category) { this.category = category; }
}
```

---

### File 5: Database Repository (`ProductRepository.java`)
Declares database operation interfaces. Extends `JpaRepository`, meaning Spring automatically generates database interaction scripts behind the scenes.

```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository // Registers this database client interface in the IoC
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    // Spring automatically generates: SELECT * FROM products WHERE name = ?;
    Optional<Product> findByName(String name);
}
```

---

### File 6: Input Validation DTO (`ProductDto.java`)
Defines the client request structure with validations (equivalent to a Marshmallow validation schema).

```java
package com.example.demo.dto;

import com.example.demo.model.ProductCategory;
import jakarta.validation.constraints.DecimalMin;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Size;
import java.math.BigDecimal;

public class ProductDto {

    @NotBlank(message = "Product name cannot be empty")
    @Size(min = 2, max = 50, message = "Name must be between 2 and 50 characters")
    private String name;

    @NotNull(message = "Price is required")
    @DecimalMin(value = "0.01", message = "Price must be greater than zero")
    private BigDecimal price;

    @NotBlank(message = "Description cannot be empty")
    @Size(max = 255, message = "Description cannot exceed 255 characters") // Max size constraints
    private String description;

    @NotNull(message = "Product category is required")
    private ProductCategory category; // Automatically validates matching Enum values

    // Getters and Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public BigDecimal getPrice() { return price; }
    public void setPrice(BigDecimal price) { this.price = price; }
    public String getDescription() { return description; }
    public void setDescription(String description) { this.description = description; }
    public ProductCategory getCategory() { return category; }
    public void setCategory(ProductCategory category) { this.category = category; }
}
```

---

### File 7: Business Service (`ProductService.java`)
Contains the core business logic. Annotated with `@Transactional` to manage transaction boundaries.

```java
package com.example.demo.service;

import com.example.demo.dto.ProductDto;
import com.example.demo.model.Product;
import com.example.demo.repository.ProductRepository;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service // Registers business service layer in IoC
public class ProductService {

    private final ProductRepository productRepository;

    // Constructor-based injection of dependency
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Transactional(readOnly = true)
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    @Transactional
    public Product createProduct(ProductDto dto) {
        // Business check
        if (productRepository.findByName(dto.getName()).isPresent()) {
            throw new IllegalArgumentException("Product name already exists");
        }
        
        // Map data fields from DTO directly to new Entity Object
        Product product = new Product(
            dto.getName(), 
            dto.getPrice(), 
            dto.getDescription(),
            dto.getCategory() // Map the category Enum
        );
        
        return productRepository.save(product); // SQL INSERT
    }

    @Transactional
    public void deleteProduct(Long id) {
        if (!productRepository.existsById(id)) {
            throw new IllegalArgumentException("Product ID does not exist");
        }
        productRepository.deleteById(id); // SQL DELETE
    }
}
```

---

### File 8: HTTP Controller (`ProductController.java`)
Maps URLs to services, returning JSON payloads (equivalent to a Flask Blueprint).

```java
package com.example.demo.controller;

import com.example.demo.dto.ProductDto;
import com.example.demo.model.Product;
import com.example.demo.service.ProductService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController // Automatically converts return values to JSON
@RequestMapping("/api/products") // Base URL pattern
public class ProductController {

    private final ProductService productService;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }

    // GET /api/products
    @GetMapping
    public ResponseEntity<List<Product>> getProducts() {
        return ResponseEntity.ok(productService.getAllProducts());
    }

    // POST /api/products
    @PostMapping
    public ResponseEntity<Product> addProduct(@Valid @RequestBody ProductDto productDto) {
        // @Valid triggers input validation checks on ProductDto fields
        Product created = productService.createProduct(productDto);
        return new ResponseEntity<>(created, HttpStatus.CREATED); // 201 Created
    }

    // DELETE /api/products/5
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> removeProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
        return new ResponseEntity<>(HttpStatus.NO_CONTENT); // 204 No Content
    }
}
```

---

### File 9: Exception Handler (`GlobalExceptionHandler.java`)
Global interceptor for errors (equivalent to `@app.errorhandler`).

```java
package com.example.demo.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice // Catches errors thrown by any controller
public class GlobalExceptionHandler {

    // Handles validation errors (e.g. invalid price, empty name)
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationErrors(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        return new ResponseEntity<>(errors, HttpStatus.UNPROCESSABLE_ENTITY); // 422
    }

    // Handles business rule exceptions
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<Map<String, String>> handleBusinessException(IllegalArgumentException ex) {
        return new ResponseEntity<>(
            Map.of("error", ex.getMessage()), 
            HttpStatus.CONFLICT // 409 Conflict
        );
    }
}
```

---

### File 10: Application Entry Point (`DemoApplication.java`)
Bootstraps the Tomcat server and configures the environment (equivalent to standard python execution scripts).

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // Triggers component scans and configuration bindings
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

### How to Run this Demo Project
1.  **Dependencies Compilation:** Execute the compilation script in your project root folder:
    ```bash
    mvn clean compile
    ```
2.  **Launch Server:** Start the application:
    ```bash
    mvn spring-boot:run
    ```
3.  **Execute Requests:** Run HTTP queries to interact with your API:
    ```bash
    # Create a product with category and description (POST)
    curl -X POST http://localhost:8080/api/products \
      -H "Content-Type: application/json" \
      -d '{"name": "Mouse", "price": 25.50, "description": "Wireless mouse", "category": "ELECTRONICS"}'
    
    # Read products (GET)
    curl http://localhost:8080/api/products
    ```

---

## 30 High-Frequency Spring Boot Questions & Answers

#### Q1: What is Spring Boot, and how does it relate to the Spring Framework?
Spring is a massive enterprise Java framework that historically required verbose XML configurations. Spring Boot is an opinionated extension that automatically configures starter setups, embedded Tomcat servers, and dependency bindings natively out-of-the-box with zero XML properties.

#### Q2: What is Inversion of Control (IoC), and what is a Spring Bean?
*   **IoC:** Transfer of object construction control from standard developer invocation (`new Object()`) directly to the Spring container.
*   **Spring Bean:** An object instantiated and managed by the Spring IoC container (e.g. controllers, services, repositories).

#### Q3: Explain Dependency Injection (DI).
A pattern to implement IoC. Instead of components manually initializing their own sub-dependencies, the container injects required matching Beans via class constructors automatically.

#### Q4: What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`?
Annotations registering a class as a Spring Bean across distinct layers:
*   `@Component`: General utility configuration helper.
*   `@Service`: Business-logic implementation layer.
*   `@Repository`: Relational database operations layer.
*   `@Controller`: HTTP route presentation handler layer.

#### Q5: How does Spring Boot start up? Explain `@SpringBootApplication`.
Runs via standard Java `main()` method calling `SpringApplication.run()`. The `@SpringBootApplication` annotation combines `@SpringBootConfiguration` (setup configurations), `@EnableAutoConfiguration` (load classpath packages), and `@ComponentScan` (scan classes for annotations).

#### Q6: What is `@RestController`?
A combination annotation wrapping `@Controller` and `@ResponseBody`, marking class endpoints as JSON APIs that serialize returned Java data structures.

#### Q7: Explain `@GetMapping` vs. `@PostMapping`.
Direct Web mapping shortcuts for HTTP requests:
*   `@GetMapping("/path")` maps HTTP GET routes.
*   `@PostMapping("/path")` maps HTTP POST routes.

#### Q8: How do you extract request parameters in Spring Boot?
*   `@PathVariable`: Extracts dynamic values in routing paths.
*   `@RequestParam`: Extracts URL query arguments.
*   `@RequestBody`: Deserializes JSON payloads into Java POJO objects.

#### Q9: What is Spring Data JPA?
An automated query constructor. Extending the `JpaRepository` interface yields fully configured database interaction operations (SQL SELECT/INSERT/UPDATE) without code creation.

#### Q10: Explain Hibernate and how it fits into JPA.
JPA is the abstract specification guideline defining Object-Relational mapping standard rules; Hibernate is the concrete implementation library executing ORM actions.

#### Q11: What does the `@Transactional` annotation do?
Automatically encapsulates a method in a database transaction block, committing variables if successful, and executing rollbacks if runtime exceptions are triggered.

#### Q12: How do you handle database migrations in Spring Boot?
Using **Flyway** or **Liquibase** packages which trace and run versioned SQL scripts (like `V1__init.sql`) on start up.

#### Q13: Explain Spring Security. How does it intercept requests?
A framework of security filters (**Filter Chain**) processing HTTP calls (verifying JWTs, checking CORS properties, managing roles) before they reach route controller methods.

#### Q14: How do you implement JWT authentication in Spring Security?
By extending `OncePerRequestFilter`, pulling the `Authorization` Bearer header, verifying token signatures cryptographically, and binding matching validations to `SecurityContextHolder`.

#### Q15: What is the role of `OncePerRequestFilter` in JWT verification?
Guarantees the JWT verification filter code executes exactly once per request.

#### Q16: How do you configure CORS in Spring Boot?
By establishing a `CorsConfigurationSource` Bean inside the Security Configuration file defining allowed origins, headers, and methods.

#### Q17: What is `@RestControllerAdvice`?
A global exception interceptor class. Methods inside decorated with `@ExceptionHandler` map exception types to custom JSON structures.

#### Q18: How do you configure application settings?
By storing variables inside `application.yml` or `application.properties`, and accessing them in Java code using `@Value("${property.name}")`.

#### Q19: How do you run code asynchronously in Spring Boot?
By enabling it with `@EnableAsync` and adding `@Async` annotations to methods, executing logic in native background thread pools without Redis/Celery queue dependencies.

#### Q20: Constructor Injection vs. Field Injection.
Constructor injection is preferred over field-based `@Autowired` because it allows immutable fields (`final`), explicitly declares dependencies, and avoids reflection during test mocks.

#### Q21: What is Maven/Gradle?
Build tools tracking dependencies in files like `pom.xml`, automating builds, compilations, testing runs, and packaging.

#### Q22: Explain Bean Scopes: Singleton vs. Prototype.
*   **Singleton (Default):** The container creates a single instance shared everywhere.
*   **Prototype:** The container instantiates a new class object on every invocation request.

#### Q23: What are Spring Interceptors?
Interceptors implementing `HandlerInterceptor` (having `preHandle` and `postHandle` hooks) to intercept request matching (analogous to Flask before/after hooks).

#### Q24: How do you implement input validation in Spring Boot?
By applying validation annotations (like `@NotBlank`, `@Email`) on DTO fields and validating requests with the `@Valid` annotation in route arguments.

#### Q25: How do you handle file uploads?
By mapping controller input arguments to `MultipartFile` types.

#### Q26: How do you write tests in Spring Boot?
Use `@SpringBootTest` for full database/endpoint integration testing, and `@WebMvcTest` with `@MockBean` for controller slice testing.

#### Q27: What is Spring Boot Actuator?
Exposes operational metric endpoints (like `/actuator/health` or `/actuator/metrics`) to observe running applications.

#### Q28: How do you manage connection pooling?
Spring Boot automatically uses **HikariCP** as its default connection pool provider.

#### Q29: How do you optimize Spring Boot startup time?
By enabling lazy initialization configurations, restricting component scans, or compiling binaries using GraalVM.

#### Q30: What is a Profile in Spring Boot?
A naming mechanism to map configurations exclusively to environments (e.g. `application-dev.yml` vs. `application-prod.yml`).

---

## Module 17: The N+1 Query Problem & Optimization Solutions

This module details the classic database execution bottleneck in Object-Relational Mapping (ORM) frameworks and how to fix it in Spring Boot.

### Conceptual Deep Dive: What is the N+1 Query Problem?
The N+1 query problem occurs when an ORM framework executes **1 SQL query** to fetch parent records, followed by **N separate SQL queries** to load children associated with each of the N parents. 

Suppose you have an `Author` entity who has a one-to-many relationship with `Book` entities. You want to retrieve all authors and print their books.

#### The Problem Scenario Setup:
1.  **Lazy Loading Configured (Default behavior):** 
    ```java
    // Author.java
    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
    private List<Book> books;
    ```
2.  **The Naive Query:** 
    ```java
    // Under the hood: SELECT * FROM authors;
    List<Author> authors = authorRepository.findAll(); 
    ```
3.  **The Iteration Loop:**
    ```java
    for (Author author : authors) {
        // Triggering lazily-loaded association execution
        // Under the hood: SELECT * FROM books WHERE author_id = ?; (Executed N times!)
        System.out.println(author.getBooks().size()); 
    }
    ```
*   **Result:** If you have 1000 authors in the database, Hibernate fires **1001 database queries** (1 to get all authors + 1000 individual queries to get books for each author), leading to latency and database CPU overload.

---

### Code Implementation: Entity Setup

```java
// src/main/java/com/example/demo/model/Author.java
package com.example.demo.model;

import jakarta.persistence.*;
import java.util.List;

@Entity
@Table(name = "authors")
public class Author {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // Lazy load ensures books are only pulled when accessed, which triggers N+1 if not optimized.
    @OneToMany(mappedBy = "author", fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    private List<Book> books;

    // Getters, Setters, Constructors...
}
```

```java
// src/main/java/com/example/demo/model/Book.java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
@Table(name = "books")
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "author_id")
    private Author author;

    // Getters, Setters, Constructors...
}
```

---

### Three Ways to Solve the N+1 Query Problem

#### Solution 1: JOIN FETCH (Custom JPQL Query)
The most common solution. You override the default fetch strategy using `JOIN FETCH` inside a custom JPQL query. This forces Hibernate to perform an SQL `LEFT OUTER JOIN` in a single query database trip, returning all authors and books loaded together.

```java
// src/main/java/com/example/demo/repository/AuthorRepository.java
package com.example.demo.repository;

import com.example.demo.model.Author;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
public interface AuthorRepository extends JpaRepository<Author, Long> {

    // Solve N+1: Executes exactly 1 SQL Join query instead of 1 + N queries.
    @Query("SELECT DISTINCT a FROM Author a LEFT JOIN FETCH a.books")
    List<Author> findAllWithBooksJoinFetch();
}
```

#### Solution 2: Entity Graphs (`@EntityGraph`)
If you want to keep repository interfaces clean without manually typing JPQL queries, use the `@EntityGraph` annotation. It instructs Spring Data JPA to dynamically generate a `JOIN FETCH` query for the declared attributes.

```java
@Repository
public interface AuthorRepository extends JpaRepository<Author, Long> {

    // Solve N+1: Tells Spring to load the 'books' list eagerly for this call using a join.
    @EntityGraph(attributePaths = {"books"})
    List<Author> findAll();
}
```

#### Solution 3: Batch Fetching (`@BatchSize`)
If joining tables generates large cartesian products (e.g. you have multiple deep relationships like books, publishers, and awards), a full join can be slow. Batch Fetching is a hybrid solution. Instead of fetching children one by one, Hibernate loads them in batches using an SQL `IN` clause.

```java
// Author.java
// When looping, Hibernate queries books for 20 authors in a single statement:
// Query: SELECT * FROM books WHERE author_id IN (?, ?, ?, ... ?, ?);
@OneToMany(mappedBy = "author", fetch = FetchType.LAZY)
@BatchSize(size = 20) 
private List<Book> books;
```
*   **Result:** Reduces the $1 + N$ queries down to $1 + (N / 20)$ queries.

---

## Module 18: Spring Security — Full JWT Authentication Setup

Spring Security is a filter chain. Every HTTP request passes through it before hitting any controller. The full JWT flow involves:
1. **Login** → verify credentials → sign & return JWT
2. **Every subsequent request** → extract token → verify signature → load user into `SecurityContext`

### Dependency (`pom.xml`)
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.3</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.3</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.3</version>
    <scope>runtime</scope>
</dependency>
```

### Step 1: JwtService (Token Generation & Validation)
```java
// service/JwtService.java
package com.example.demo.service;

import io.jsonwebtoken.*;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Service;

import javax.crypto.SecretKey;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.function.Function;

@Service
public class JwtService {

    @Value("${security.jwt.secret-key}")      // from application.yml
    private String secretKey;

    @Value("${security.jwt.expiration-ms}")   // e.g. 900000 = 15 minutes
    private long jwtExpiration;

    // Extract username (subject) from token
    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    // Generic claim extractor using function
    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    // Generate token with only username as subject
    public String generateToken(UserDetails userDetails) {
        return generateToken(new HashMap<>(), userDetails);
    }

    // Generate token with extra claims (e.g. roles)
    public String generateToken(Map<String, Object> extraClaims, UserDetails userDetails) {
        return Jwts.builder()
                .claims(extraClaims)
                .subject(userDetails.getUsername())
                .issuedAt(new Date(System.currentTimeMillis()))
                .expiration(new Date(System.currentTimeMillis() + jwtExpiration))
                .signWith(getSignInKey(), Jwts.SIG.HS256)
                .compact();
    }

    // Validate token: check username match + not expired
    public boolean isTokenValid(String token, UserDetails userDetails) {
        final String username = extractUsername(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return extractClaim(token, Claims::getExpiration).before(new Date());
    }

    private Claims extractAllClaims(String token) {
        return Jwts.parser()
                .verifyWith(getSignInKey())
                .build()
                .parseSignedClaims(token)
                .getPayload();
    }

    private SecretKey getSignInKey() {
        byte[] keyBytes = Decoders.BASE64.decode(secretKey);
        return Keys.hmacShaKeyFor(keyBytes);
    }
}
```

### Step 2: JwtAuthenticationFilter (OncePerRequestFilter)
```java
// security/JwtAuthenticationFilter.java
package com.example.demo.security;

import com.example.demo.service.JwtService;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.RequiredArgsConstructor;
import org.springframework.lang.NonNull;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;

import java.io.IOException;

@Component
@RequiredArgsConstructor
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(
            @NonNull HttpServletRequest request,
            @NonNull HttpServletResponse response,
            @NonNull FilterChain filterChain
    ) throws ServletException, IOException {

        // 1. Extract Authorization header
        final String authHeader = request.getHeader("Authorization");

        // 2. Skip if no Bearer token present
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        // 3. Extract raw JWT (strip "Bearer " prefix)
        final String jwt = authHeader.substring(7);

        // 4. Extract username from token
        final String username = jwtService.extractUsername(jwt);

        // 5. If username found and no existing auth in context
        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {

            // 6. Load user from database
            UserDetails userDetails = this.userDetailsService.loadUserByUsername(username);

            // 7. Validate token
            if (jwtService.isTokenValid(jwt, userDetails)) {

                // 8. Create authentication token and set in security context
                UsernamePasswordAuthenticationToken authToken =
                    new UsernamePasswordAuthenticationToken(
                        userDetails,
                        null,
                        userDetails.getAuthorities()
                    );
                authToken.setDetails(
                    new WebAuthenticationDetailsSource().buildDetails(request)
                );
                // 9. Mark request as authenticated
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        // 10. Pass to next filter
        filterChain.doFilter(request, response);
    }
}
```

### Step 3: SecurityConfig (Filter Chain + CORS)
```java
// security/SecurityConfig.java
package com.example.demo.security;

import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;

import java.util.List;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity   // enables @PreAuthorize on methods
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthFilter;
    private final UserDetailsService userDetailsService;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())        // REST APIs don't need CSRF
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))
            .authorizeHttpRequests(auth -> auth
                // Public endpoints — no token required
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers("/actuator/health").permitAll()
                // Role-based access
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                // All other endpoints require authentication
                .anyRequest().authenticated()
            )
            // Stateless: don't create sessions (JWT handles state)
            .sessionManagement(session ->
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            // Register custom auth provider (DB-backed)
            .authenticationProvider(authenticationProvider())
            // Insert JWT filter BEFORE Spring's default username/password filter
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
        authProvider.setUserDetailsService(userDetailsService);
        authProvider.setPasswordEncoder(passwordEncoder());
        return authProvider;
    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration config
    ) throws Exception {
        return config.getAuthenticationManager();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();   // bcrypt with cost factor 10
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration config = new CorsConfiguration();
        config.setAllowedOrigins(List.of("http://localhost:5173"));  // React dev origin
        config.setAllowedMethods(List.of("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        config.setAllowedHeaders(List.of("Authorization", "Content-Type"));
        config.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return source;
    }
}
```

### Step 4: AuthController (Login endpoint)
```java
// controller/AuthController.java
@RestController
@RequestMapping("/api/auth")
@RequiredArgsConstructor
public class AuthController {

    private final AuthenticationManager authenticationManager;
    private final UserDetailsService userDetailsService;
    private final JwtService jwtService;

    @PostMapping("/login")
    public ResponseEntity<Map<String, String>> login(@RequestBody LoginRequest request) {
        // Throws BadCredentialsException if wrong password
        authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(request.username(), request.password())
        );

        UserDetails user = userDetailsService.loadUserByUsername(request.username());
        String token = jwtService.generateToken(user);

        return ResponseEntity.ok(Map.of("token", token));
    }
}

// Using Java 16+ record for DTO
record LoginRequest(String username, String password) {}
```

### Method-Level Security with @PreAuthorize
```java
// On any @Service or @Controller method
@PreAuthorize("hasRole('ADMIN')")
public void deleteUser(Long id) { /* only ADMIN can call this */ }

@PreAuthorize("hasAnyRole('ADMIN', 'MANAGER')")
public List<User> getAllUsers() { /* ADMIN or MANAGER */ }

@PreAuthorize("#userId == authentication.principal.id")
public User getMyProfile(Long userId) { /* user can only see their own profile */ }
```

---

## Module 19: Pagination with Pageable

Spring Data JPA provides built-in pagination — no manual LIMIT/OFFSET SQL needed.

```java
// repository/ProductRepository.java
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {

    // Spring generates: SELECT * FROM products WHERE category = ? LIMIT ? OFFSET ?
    Page<Product> findByCategory(String category, Pageable pageable);

    // Custom query with pagination
    @Query("SELECT p FROM Product p WHERE p.price BETWEEN :min AND :max ORDER BY p.price")
    Page<Product> findByPriceRange(
        @Param("min") BigDecimal min,
        @Param("max") BigDecimal max,
        Pageable pageable
    );
}

// service/ProductService.java
@Service
@RequiredArgsConstructor
public class ProductService {
    private final ProductRepository repo;

    public Page<Product> getProductsPaginated(int page, int size, String sortBy, String direction) {
        // Build sort direction
        Sort sort = direction.equalsIgnoreCase("desc")
            ? Sort.by(sortBy).descending()
            : Sort.by(sortBy).ascending();

        // Create Pageable object — page is 0-indexed!
        Pageable pageable = PageRequest.of(page, size, sort);

        return repo.findAll(pageable);
    }
}

// controller/ProductController.java
@GetMapping
public ResponseEntity<Map<String, Object>> getProducts(
    @RequestParam(defaultValue = "0") int page,      // 0-indexed
    @RequestParam(defaultValue = "10") int size,
    @RequestParam(defaultValue = "id") String sortBy,
    @RequestParam(defaultValue = "asc") String direction,
    @RequestParam(required = false) String category
) {
    Page<Product> pageResult = productService.getProductsPaginated(page, size, sortBy, direction);

    // Build standard paginated response
    Map<String, Object> response = new HashMap<>();
    response.put("data", pageResult.getContent());          // items for this page
    response.put("currentPage", pageResult.getNumber());    // 0-indexed
    response.put("totalItems", pageResult.getTotalElements());
    response.put("totalPages", pageResult.getTotalPages());
    response.put("isFirst", pageResult.isFirst());
    response.put("isLast", pageResult.isLast());

    return ResponseEntity.ok(response);
}
```

**Request Example:**
```bash
GET /api/products?page=0&size=5&sortBy=price&direction=desc
```

---

## Module 20: @Async — True Background Threading

`@Async` runs a method in a Spring-managed thread pool without blocking the HTTP request thread. Unlike Celery (Python), no external broker is needed.

```java
// config/AsyncConfig.java — custom thread pool (do NOT use default executor in production)
@Configuration
@EnableAsync
public class AsyncConfig {

    @Bean(name = "taskExecutor")
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);          // always-alive threads
        executor.setMaxPoolSize(20);          // max threads under load
        executor.setQueueCapacity(100);       // queue before spawning new threads
        executor.setThreadNamePrefix("Async-"); // thread names in logs
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        executor.initialize();
        return executor;
    }
}

// service/EmailService.java
@Service
public class EmailService {

    // @Async: method runs in taskExecutor thread pool, not HTTP request thread
    @Async("taskExecutor")
    public CompletableFuture<String> sendWelcomeEmail(String to, String name) {
        // Simulates 2-second email sending
        Thread.sleep(2000);
        System.out.printf("[Thread: %s] Sending email to %s%n",
            Thread.currentThread().getName(), to);

        return CompletableFuture.completedFuture("Email sent to " + to);
    }

    // Fire-and-forget: returns void, caller doesn't wait
    @Async("taskExecutor")
    public void sendNotification(Long userId) {
        // runs in background — caller gets back immediately
        notificationRepo.createFor(userId);
    }
}

// controller/UserController.java
@PostMapping("/register")
public ResponseEntity<String> register(@RequestBody UserDto dto) {
    User user = userService.createUser(dto);

    // This returns IMMEDIATELY — email sends in background
    emailService.sendWelcomeEmail(user.getEmail(), user.getName());

    return ResponseEntity.status(201).body("User created. Welcome email queued.");
}

// Waiting for async result (when you need the result later)
@GetMapping("/process")
public ResponseEntity<String> process() throws Exception {
    CompletableFuture<String> future1 = emailService.sendWelcomeEmail("a@b.com", "A");
    CompletableFuture<String> future2 = emailService.sendWelcomeEmail("c@d.com", "C");

    // Wait for both to complete (blocking here is intentional)
    CompletableFuture.allOf(future1, future2).join();

    return ResponseEntity.ok("Both emails sent: " + future1.get() + ", " + future2.get());
}
```

> [!WARNING]
> **Common @Async Pitfall:** Calling an `@Async` method from within the **same class** bypasses the Spring AOP proxy and runs synchronously. Always inject the service and call it from another class.

---

## Module 21: @Scheduled — Cron Jobs

```java
// config/SchedulerConfig.java
@Configuration
@EnableScheduling   // Must enable scheduling
public class SchedulerConfig {}

// service/ScheduledTasks.java
@Service
@Slf4j
public class ScheduledTasks {

    // ===== Fixed Rate: runs every N milliseconds from start =====
    // Does NOT wait for previous execution to finish
    @Scheduled(fixedRate = 5000)   // every 5 seconds
    public void pollExternalApi() {
        log.info("[{}] Polling external API...", LocalDateTime.now());
    }

    // ===== Fixed Delay: runs N milliseconds AFTER previous finishes =====
    // Waits for previous to complete before scheduling next
    @Scheduled(fixedDelay = 10000)
    public void processQueue() {
        log.info("Processing message queue...");
        // even if this takes 30 seconds, next run waits 10s AFTER it finishes
    }

    // ===== Cron: standard cron expression (5 or 6 fields) =====
    // Format: second minute hour day-of-month month day-of-week
    @Scheduled(cron = "0 0 2 * * ?")        // 2:00 AM every day
    public void generateDailyReport() {
        log.info("Generating daily report...");
    }

    @Scheduled(cron = "0 0 9 * * MON-FRI")  // 9:00 AM Monday–Friday
    public void sendMorningDigest() {
        log.info("Sending morning digest...");
    }

    @Scheduled(cron = "0 */15 * * * ?")     // every 15 minutes
    public void refreshCache() {
        log.info("Refreshing distributed cache...");
    }

    // ===== From application.yml =====
    @Scheduled(cron = "${app.jobs.cleanup-cron}")  // externalized cron
    public void cleanupExpiredSessions() {
        sessionRepository.deleteExpired();
    }

    // ===== Dynamic scheduling (disable/enable at runtime) =====
    // Use ScheduledTaskRegistrar or SchedulingConfigurer for dynamic control
}
```

**application.yml:**
```yaml
app:
  jobs:
    cleanup-cron: "0 0 3 * * ?"   # 3 AM daily
spring:
  task:
    scheduling:
      pool:
        size: 5   # thread pool for scheduled tasks
```

---

## Module 22: @Cacheable — Distributed Caching with Redis

`@Cacheable` transparently caches method results using Spring's caching abstraction (backed by Redis, EhCache, Caffeine, etc.).

### Dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

### Redis Cache Configuration
```java
// config/CacheConfig.java
@Configuration
@EnableCaching
public class CacheConfig {

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory factory) {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofMinutes(10))             // default TTL: 10 min
            .serializeKeysWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new GenericJackson2JsonRedisSerializer()))
            .disableCachingNullValues();                   // don't cache null

        // Per-cache TTL overrides
        Map<String, RedisCacheConfiguration> cacheConfigs = new HashMap<>();
        cacheConfigs.put("products", config.entryTtl(Duration.ofMinutes(30)));
        cacheConfigs.put("users",    config.entryTtl(Duration.ofHours(1)));

        return RedisCacheManager.builder(factory)
            .cacheDefaults(config)
            .withInitialCacheConfigurations(cacheConfigs)
            .build();
    }
}
```

### Caching Annotations
```java
// service/ProductService.java
@Service
@RequiredArgsConstructor
public class ProductService {

    private final ProductRepository repo;

    // @Cacheable: cache result. On second call, returns cached value WITHOUT hitting DB.
    // Cache key: "products::42"
    @Cacheable(value = "products", key = "#id")
    public Product getById(Long id) {
        log.info("CACHE MISS — querying database for product {}", id);
        return repo.findById(id).orElseThrow(() -> new RuntimeException("Not found"));
    }

    // Custom key with SpEL expression
    @Cacheable(value = "products", key = "#category + '_' + #page")
    public List<Product> getByCategory(String category, int page) {
        return repo.findByCategory(category, PageRequest.of(page, 10)).getContent();
    }

    // Conditional caching — only cache if result is not empty
    @Cacheable(value = "products", key = "#id", condition = "#id > 0",
               unless = "#result == null")
    public Product findProduct(Long id) {
        return repo.findById(id).orElse(null);
    }

    // @CachePut: ALWAYS executes method AND updates cache (used after updates)
    @CachePut(value = "products", key = "#result.id")
    public Product updateProduct(Long id, ProductDto dto) {
        Product p = repo.findById(id).orElseThrow();
        p.setName(dto.getName());
        p.setPrice(dto.getPrice());
        return repo.save(p);  // returns saved, which becomes new cache value
    }

    // @CacheEvict: removes entry from cache (used after deletes/updates)
    @CacheEvict(value = "products", key = "#id")
    public void deleteProduct(Long id) {
        repo.deleteById(id);
    }

    // Evict ALL entries in a cache
    @CacheEvict(value = "products", allEntries = true)
    public void clearAllProductCache() {
        log.info("Cleared entire products cache");
    }

    // @Caching: combine multiple cache operations on one method
    @Caching(evict = {
        @CacheEvict(value = "products", key = "#id"),
        @CacheEvict(value = "product-list", allEntries = true)
    })
    public void deleteProductAndClearList(Long id) {
        repo.deleteById(id);
    }
}
```

**application.yml:**
```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
      password: ${REDIS_PASSWORD:}   # optional
      timeout: 2000ms
  cache:
    type: redis
```

---

## Module 23: Kafka Integration (Producer & Consumer)

Apache Kafka is a distributed event streaming platform. Spring Boot integrates via **Spring Kafka**.

### Dependency
```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

### application.yml
```yaml
spring:
  kafka:
    bootstrap-servers: localhost:9092
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
      acks: all                   # wait for all replicas to acknowledge
      retries: 3
      properties:
        enable.idempotence: true  # exactly-once producer
    consumer:
      group-id: my-service-group
      auto-offset-reset: earliest # start from beginning if no committed offset
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      properties:
        spring.json.trusted.packages: "com.example.demo.events"
```

### Kafka Configuration Bean
```java
// config/KafkaConfig.java
@Configuration
public class KafkaConfig {

    @Bean
    public NewTopic orderCreatedTopic() {
        // Create topic with 3 partitions and replication factor 1 (dev)
        return TopicBuilder.name("order-created")
            .partitions(3)
            .replicas(1)
            .build();
    }

    @Bean
    public NewTopic inventoryUpdateTopic() {
        return TopicBuilder.name("inventory-update").partitions(3).replicas(1).build();
    }
}
```

### Event DTO
```java
// events/OrderCreatedEvent.java
public record OrderCreatedEvent(
    Long orderId,
    Long userId,
    List<Long> productIds,
    BigDecimal totalAmount,
    LocalDateTime createdAt
) {}
```

### Kafka Producer
```java
// service/OrderProducerService.java
@Service
@RequiredArgsConstructor
@Slf4j
public class OrderProducerService {

    private final KafkaTemplate<String, OrderCreatedEvent> kafkaTemplate;

    public void publishOrderCreated(OrderCreatedEvent event) {
        // Key = orderId as String → ensures same order goes to same partition
        String key = String.valueOf(event.orderId());

        CompletableFuture<SendResult<String, OrderCreatedEvent>> future =
            kafkaTemplate.send("order-created", key, event);

        future.whenComplete((result, ex) -> {
            if (ex == null) {
                log.info("Order {} sent to partition {} offset {}",
                    event.orderId(),
                    result.getRecordMetadata().partition(),
                    result.getRecordMetadata().offset());
            } else {
                log.error("Failed to send order {}: {}", event.orderId(), ex.getMessage());
                // Implement retry / dead-letter queue logic here
            }
        });
    }

    // Synchronous send (blocks until broker acknowledges)
    public void publishOrderCreatedSync(OrderCreatedEvent event) throws Exception {
        SendResult<String, OrderCreatedEvent> result =
            kafkaTemplate.send("order-created", event).get();  // blocks
        log.info("Sent synchronously to offset: {}", result.getRecordMetadata().offset());
    }
}
```

### Kafka Consumer
```java
// service/OrderConsumerService.java
@Service
@Slf4j
public class OrderConsumerService {

    // @KafkaListener: registers method as consumer in group 'my-service-group'
    @KafkaListener(
        topics = "order-created",
        groupId = "my-service-group",
        containerFactory = "kafkaListenerContainerFactory"
    )
    public void consumeOrderCreated(
        @Payload OrderCreatedEvent event,
        @Header(KafkaHeaders.RECEIVED_PARTITION) int partition,
        @Header(KafkaHeaders.OFFSET) long offset
    ) {
        log.info("Received order {} from partition {} offset {}",
            event.orderId(), partition, offset);

        // Process the event
        inventoryService.reserveStock(event.productIds());
        notificationService.notifyUser(event.userId(), "Order confirmed!");
    }

    // Batch consumer — consume multiple records at once
    @KafkaListener(topics = "order-created", groupId = "batch-group")
    public void consumeBatch(List<OrderCreatedEvent> events) {
        log.info("Batch processing {} events", events.size());
        events.forEach(e -> processOrder(e));
    }

    // Manual acknowledgment (for exactly-once semantics)
    @KafkaListener(topics = "order-created", groupId = "manual-group")
    public void consumeWithAck(OrderCreatedEvent event, Acknowledgment ack) {
        try {
            processOrder(event);
            ack.acknowledge();   // commit offset only after successful processing
        } catch (Exception e) {
            log.error("Processing failed — NOT acknowledging offset");
            // Message will be redelivered
        }
    }

    // Dead Letter Topic — failed messages go to "order-created.DLT"
    @KafkaListener(topics = "order-created.DLT", groupId = "dlt-group")
    public void consumeDeadLetter(OrderCreatedEvent event) {
        log.error("Dead letter event: {}", event);
        alertingService.sendAlert("Unprocessable Kafka message: " + event);
    }
}
```

---

## Module 24: Spring Actuator — Production Monitoring

Spring Actuator exposes operational endpoints to monitor and manage your running app.

### Dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<!-- For Prometheus metrics (Grafana stack) -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

### application.yml configuration
```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics, prometheus, env, loggers, beans, httptrace
      base-path: /actuator
  endpoint:
    health:
      show-details: always    # show component health details
      show-components: always
    shutdown:
      enabled: false          # NEVER enable in production without auth
  health:
    redis:
      enabled: true
    db:
      enabled: true
  metrics:
    tags:
      application: ${spring.application.name}  # tag all metrics with app name

info:
  app:
    name: My Service
    version: 1.0.0
    environment: ${SPRING_PROFILES_ACTIVE:dev}
```

### Key Actuator Endpoints
| Endpoint | Description |
|---|---|
| `GET /actuator/health` | App + component health (DB, Redis, Kafka) |
| `GET /actuator/info` | App metadata (name, version from `info.*`) |
| `GET /actuator/metrics` | All available metric names |
| `GET /actuator/metrics/jvm.memory.used` | Specific metric with tags |
| `GET /actuator/prometheus` | Prometheus-format metrics for Grafana |
| `GET /actuator/env` | All environment properties |
| `GET /actuator/loggers` | View/change log levels at runtime |
| `POST /actuator/loggers/com.example` | Set log level: `{"configuredLevel": "DEBUG"}` |
| `GET /actuator/beans` | All Spring beans in context |
| `GET /actuator/mappings` | All HTTP route mappings |
| `GET /actuator/threaddump` | Full JVM thread dump |
| `GET /actuator/heapdump` | Download JVM heap dump file |

### Custom Health Indicator
```java
// health/DatabaseHealthIndicator.java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {

    private final DataSource dataSource;

    public DatabaseHealthIndicator(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    public Health health() {
        try (Connection conn = dataSource.getConnection()) {
            conn.prepareStatement("SELECT 1").execute();
            return Health.up()
                .withDetail("database", "PostgreSQL")
                .withDetail("status", "Connection pool healthy")
                .build();
        } catch (SQLException e) {
            return Health.down()
                .withDetail("error", e.getMessage())
                .build();
        }
    }
}
```

### Custom Metrics with Micrometer
```java
// service/OrderService.java
@Service
public class OrderService {
    private final Counter ordersCreatedCounter;
    private final Timer orderProcessingTimer;
    private final AtomicInteger activeOrders;

    public OrderService(MeterRegistry registry) {
        // Counter: monotonically increasing count
        this.ordersCreatedCounter = Counter.builder("orders.created")
            .tag("status", "success")
            .description("Total orders created")
            .register(registry);

        // Timer: measures duration of operations
        this.orderProcessingTimer = Timer.builder("orders.processing.duration")
            .description("Time to process an order")
            .register(registry);

        // Gauge: current snapshot value (active orders in queue)
        this.activeOrders = registry.gauge("orders.active",
            new AtomicInteger(0));
    }

    public Order createOrder(OrderDto dto) {
        return orderProcessingTimer.record(() -> {
            Order order = processOrder(dto);
            ordersCreatedCounter.increment();
            activeOrders.incrementAndGet();
            return order;
        });
    }
}
```

---

## Module 25: WebFlux — Reactive Programming

Spring WebFlux is the reactive, non-blocking alternative to Spring MVC. Uses Project Reactor (`Mono` and `Flux`).

**When to use WebFlux:**
- High concurrency with I/O-bound operations (thousands of concurrent connections)
- Streaming data (SSE, WebSockets)
- Microservice-to-microservice calls with `WebClient`

### Mono and Flux
```java
// Mono<T>: 0 or 1 element (like Optional but async)
// Flux<T>: 0 to N elements (like Stream but async)

Mono<String> mono = Mono.just("Hello");
Mono<String> empty = Mono.empty();
Mono<String> error = Mono.error(new RuntimeException("oops"));

Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);
Flux<Integer> range = Flux.range(1, 100);
Flux<Long> interval = Flux.interval(Duration.ofSeconds(1));  // ticks every second

// Operators (similar to Stream API but lazy + async)
flux
    .filter(n -> n % 2 == 0)
    .map(n -> n * 10)
    .flatMap(n -> Mono.just(n + 1))    // async map (returns Mono/Flux)
    .take(3)
    .doOnNext(n -> log.info("Value: {}", n))
    .subscribe(System.out::println);
```

### Reactive REST Controller
```java
// Must use spring-boot-starter-webflux (replaces spring-boot-starter-web)
@RestController
@RequestMapping("/api/reactive/products")
@RequiredArgsConstructor
public class ReactiveProductController {

    private final ReactiveProductRepository repo;

    @GetMapping                            // returns stream of all products
    public Flux<Product> getAll() {
        return repo.findAll();
    }

    @GetMapping("/{id}")                   // returns single product or 404
    public Mono<ResponseEntity<Product>> getById(@PathVariable Long id) {
        return repo.findById(id)
            .map(ResponseEntity::ok)
            .defaultIfEmpty(ResponseEntity.notFound().build());
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Product> create(@RequestBody Mono<Product> productMono) {
        return productMono.flatMap(repo::save);
    }

    // Server-Sent Events — real-time streaming to browser
    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<Product> streamProducts() {
        return repo.findAll().delayElements(Duration.ofMillis(500));
    }
}
```

### WebClient (Reactive HTTP Client — replaces RestTemplate)
```java
// config/WebClientConfig.java
@Configuration
public class WebClientConfig {
    @Bean
    public WebClient webClient() {
        return WebClient.builder()
            .baseUrl("https://api.example.com")
            .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
            .codecs(config -> config.defaultCodecs().maxInMemorySize(2 * 1024 * 1024))
            .build();
    }
}

// service/ExternalApiService.java
@Service
@RequiredArgsConstructor
public class ExternalApiService {

    private final WebClient webClient;

    // GET — returns Mono
    public Mono<Product> fetchProduct(Long id) {
        return webClient.get()
            .uri("/products/{id}", id)
            .retrieve()
            .onStatus(HttpStatusCode::is4xxClientError,
                resp -> Mono.error(new RuntimeException("Client error: " + resp.statusCode())))
            .onStatus(HttpStatusCode::is5xxServerError,
                resp -> Mono.error(new RuntimeException("Server error")))
            .bodyToMono(Product.class)
            .timeout(Duration.ofSeconds(5))         // timeout after 5s
            .retryWhen(Retry.backoff(3, Duration.ofMillis(500)));  // retry 3x with backoff
    }

    // POST with body
    public Mono<Order> createOrder(OrderDto dto) {
        return webClient.post()
            .uri("/orders")
            .bodyValue(dto)
            .retrieve()
            .bodyToMono(Order.class);
    }

    // Parallel calls — fetch product and user simultaneously
    public Mono<Map<String, Object>> getProductAndUser(Long productId, Long userId) {
        Mono<Product> productMono = fetchProduct(productId);
        Mono<User> userMono = fetchUser(userId);

        return Mono.zip(productMono, userMono, (product, user) ->
            Map.of("product", product, "user", user)
        );
    }
}
```

---

## Module 26: Spring Batch — Large-Scale Data Processing

Spring Batch processes large volumes of data in chunks: **read → process → write**.

### Core Concepts
```
Job → Step → ItemReader → ItemProcessor → ItemWriter
              (read 1)     (transform 1)   (write N as chunk)
```

### Simple Batch Job
```java
// config/BatchConfig.java
@Configuration
@EnableBatchProcessing
@RequiredArgsConstructor
public class BatchConfig {

    // ===== READER: reads records from CSV file =====
    @Bean
    public FlatFileItemReader<UserCsvDto> csvReader() {
        return new FlatFileItemReaderBuilder<UserCsvDto>()
            .name("userCsvReader")
            .resource(new ClassPathResource("users.csv"))
            .delimited()
            .names("firstName", "lastName", "email", "age")
            .targetType(UserCsvDto.class)
            .linesToSkip(1)    // skip header
            .build();
    }

    // ===== PROCESSOR: transforms/filters each record =====
    @Bean
    public ItemProcessor<UserCsvDto, User> userProcessor() {
        return csvDto -> {
            // Skip records with invalid email
            if (!csvDto.email().contains("@")) return null;  // null = skip

            User user = new User();
            user.setName(csvDto.firstName() + " " + csvDto.lastName());
            user.setEmail(csvDto.email().toLowerCase());
            user.setAge(Integer.parseInt(csvDto.age()));
            user.setCreatedAt(LocalDateTime.now());
            return user;
        };
    }

    // ===== WRITER: bulk-inserts to DB in chunks =====
    @Bean
    public JdbcBatchItemWriter<User> dbWriter(DataSource dataSource) {
        return new JdbcBatchItemWriterBuilder<User>()
            .itemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>())
            .sql("INSERT INTO users (name, email, age, created_at) " +
                 "VALUES (:name, :email, :age, :createdAt)")
            .dataSource(dataSource)
            .build();
    }

    // ===== STEP: ties reader → processor → writer with chunk size =====
    @Bean
    public Step importUsersStep(JobRepository jobRepo, PlatformTransactionManager txManager,
                                 FlatFileItemReader<UserCsvDto> reader,
                                 ItemProcessor<UserCsvDto, User> processor,
                                 JdbcBatchItemWriter<User> writer) {
        return new StepBuilder("importUsersStep", jobRepo)
            .<UserCsvDto, User>chunk(100, txManager)   // process 100 records per chunk
            .reader(reader)
            .processor(processor)
            .writer(writer)
            .faultTolerant()
            .skip(ValidationException.class)            // skip bad records
            .skipLimit(10)                              // max 10 skips before job fails
            .listener(new StepExecutionListener() {
                @Override
                public void afterStep(StepExecution stepExecution) {
                    log.info("Step finished. Read: {}, Written: {}, Skipped: {}",
                        stepExecution.getReadCount(),
                        stepExecution.getWriteCount(),
                        stepExecution.getSkipCount());
                }
            })
            .build();
    }

    // ===== JOB: orchestrates steps =====
    @Bean
    public Job importUsersJob(JobRepository jobRepo, Step importUsersStep) {
        return new JobBuilder("importUsersJob", jobRepo)
            .incrementer(new RunIdIncrementer())        // allows re-running job
            .start(importUsersStep)
            .next(sendEmailsStep)                       // chain multiple steps
            .on("FAILED").to(cleanupStep)               // conditional flow
            .end()
            .build();
    }
}

// Trigger job programmatically (e.g. from REST endpoint or @Scheduled)
@Service
@RequiredArgsConstructor
public class BatchLauncherService {
    private final JobLauncher jobLauncher;
    private final Job importUsersJob;

    public void runImport() throws Exception {
        JobParameters params = new JobParametersBuilder()
            .addLong("startTime", System.currentTimeMillis())  // unique param to re-run
            .addString("filename", "users_2024.csv")
            .toJobParameters();

        JobExecution execution = jobLauncher.run(importUsersJob, params);
        log.info("Job status: {}", execution.getStatus());
    }
}
```

---

## Module 27: Docker & Kubernetes Deployment

### Dockerfile (Multi-Stage Build — minimizes image size)
```dockerfile
# Stage 1: Build — downloads deps and compiles JAR
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /workspace

# Cache Maven dependencies separately (only re-downloads when pom.xml changes)
COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .
RUN ./mvnw dependency:go-offline -B

# Copy source and build
COPY src src
RUN ./mvnw package -DskipTests -B
RUN mkdir -p target/extracted && java -Djarmode=layertools \
    -jar target/*.jar extract --destination target/extracted

# Stage 2: Runtime — minimal JRE image (~80MB vs ~400MB full JDK)
FROM eclipse-temurin:21-jre-alpine AS runtime
WORKDIR /app

# Add non-root user for security
RUN addgroup -S spring && adduser -S spring -G spring
USER spring

# Copy layered JAR (faster builds — libs layer rarely changes)
COPY --from=build /workspace/target/extracted/dependencies/ ./
COPY --from=build /workspace/target/extracted/spring-boot-loader/ ./
COPY --from=build /workspace/target/extracted/snapshot-dependencies/ ./
COPY --from=build /workspace/target/extracted/application/ ./

EXPOSE 8080

# Use exec form to receive OS signals properly (graceful shutdown)
ENTRYPOINT ["java", \
    "-Xms256m", "-Xmx512m", \
    "-Djava.security.egd=file:/dev/./urandom", \
    "org.springframework.boot.loader.launch.JarLauncher"]
```

### docker-compose.yml (local development stack)
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_PROFILES_ACTIVE: docker
      SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/appdb
      SPRING_DATASOURCE_USERNAME: appuser
      SPRING_DATASOURCE_PASSWORD: ${DB_PASSWORD}
      SPRING_DATA_REDIS_HOST: redis
      SPRING_KAFKA_BOOTSTRAP_SERVERS: kafka:9092
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_started
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/actuator/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U appuser -d appdb"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1

volumes:
  postgres_data:
  redis_data:
```

### Kubernetes Deployment (k8s/deployment.yaml)
```yaml
# Deployment — manages Pod replicas
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-app
  namespace: production
  labels:
    app: spring-app
spec:
  replicas: 3                        # run 3 instances
  selector:
    matchLabels:
      app: spring-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1              # keep at least 2 healthy during deploy
      maxSurge: 1
  template:
    metadata:
      labels:
        app: spring-app
    spec:
      containers:
        - name: spring-app
          image: myregistry/spring-app:1.2.0
          ports:
            - containerPort: 8080
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: "production"
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:             # pull from K8s Secret
                  name: app-secrets
                  key: db-password
            - name: SPRING_DATASOURCE_URL
              valueFrom:
                configMapKeyRef:          # pull from K8s ConfigMap
                  name: app-config
                  key: datasource-url
          resources:
            requests:
              memory: "256Mi"
              cpu: "250m"
            limits:
              memory: "512Mi"
              cpu: "500m"
          livenessProbe:                  # K8s restarts pod if this fails
            httpGet:
              path: /actuator/health/liveness
              port: 8080
            initialDelaySeconds: 60
            periodSeconds: 10
            failureThreshold: 3
          readinessProbe:                 # K8s removes pod from LB if this fails
            httpGet:
              path: /actuator/health/readiness
              port: 8080
            initialDelaySeconds: 30
            periodSeconds: 5
            failureThreshold: 3

---
# Service — load balancer across pods
apiVersion: v1
kind: Service
metadata:
  name: spring-app-svc
  namespace: production
spec:
  selector:
    app: spring-app
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP   # use LoadBalancer for external access

---
# HorizontalPodAutoscaler — scale pods based on CPU
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: spring-app-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: spring-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70   # scale out when CPU > 70%
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
```

### Graceful Shutdown Configuration
```yaml
# application.yml — critical for K8s rolling updates
server:
  shutdown: graceful         # wait for in-flight requests to finish

spring:
  lifecycle:
    timeout-per-shutdown-phase: 30s   # wait up to 30s before forcing shutdown
```

```java
// Liveness vs Readiness for K8s
// Liveness: "Is the app alive? If not, restart the pod."
// Readiness: "Is the app ready to receive traffic? If not, remove from load balancer."

// Spring Actuator automatically exposes both when configured:
// /actuator/health/liveness  → checks if app process is healthy
// /actuator/health/readiness → checks if app can serve requests (DB connected, warm)
```

---

## Module 28: FAANG Spring Boot Cheat Sheet

| Topic | Key Points to Remember |
|---|---|
| **IoC / DI** | Container manages beans. Constructor injection > field injection. |
| **Bean Scopes** | Singleton (default), Prototype, Request, Session |
| **@Transactional** | AOP proxy wraps method. Self-call bypasses it. `readOnly=true` optimizes reads. |
| **JWT Filter** | `OncePerRequestFilter` → extract → verify → `SecurityContextHolder.setAuthentication()` |
| **N+1 Fix** | `JOIN FETCH`, `@EntityGraph`, `@BatchSize` |
| **@Cacheable** | Cache hit = skip method. `@CachePut` = always update. `@CacheEvict` = remove. |
| **@Async** | Runs in thread pool. Return `CompletableFuture`. Never self-call. Enable `@EnableAsync`. |
| **@Scheduled** | `fixedRate` = strict interval. `fixedDelay` = wait after finish. `cron` = cron expr. |
| **Pageable** | `PageRequest.of(page, size, sort)` → `Page<T>` has content, totalPages, totalElements |
| **Kafka** | `KafkaTemplate.send()` → producer. `@KafkaListener` → consumer. Group ID = consumer group. |
| **WebFlux** | `Mono` (0-1), `Flux` (0-N). `WebClient` replaces `RestTemplate`. Non-blocking I/O. |
| **Actuator** | `/health`, `/metrics`, `/prometheus`. `@HealthIndicator` for custom checks. |
| **Docker** | Multi-stage build. Non-root user. Layered JAR for faster builds. `ENTRYPOINT` exec form. |
| **K8s** | Liveness = restart. Readiness = remove from LB. HPA = auto-scale. RollingUpdate = zero-downtime. |
