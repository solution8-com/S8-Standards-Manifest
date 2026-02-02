# Java Coding Standards

## Overview

Java is used for enterprise applications and backend services. We follow Oracle's Java Code Conventions with modern best practices.

## Java Version

- Use **Java 17 LTS** or newer for all new projects
- Leverage modern Java features (records, sealed classes, pattern matching)

## Code Style

### Naming Conventions
```java
// PascalCase for classes and interfaces
public class UserService {
    // ...
}

public interface UserRepository {
    // ...
}

// camelCase for methods and variables
public User getUserById(String userId) {
    String userName = fetchUserName(userId);
    return new User(userId, userName);
}

// UPPER_CASE for constants
public static final int MAX_RETRY_COUNT = 3;
public static final String API_ENDPOINT = "https://api.example.com";

// Package names in lowercase
package com.solution8.users.service;
```

### Formatting
- Use 4 spaces for indentation
- Opening brace on the same line
- Maximum line length: 120 characters
- One statement per line

```java
public class Example {
    public void method() {
        if (condition) {
            doSomething();
        } else {
            doSomethingElse();
        }
    }
}
```

## Modern Java Features

### Records (Java 14+)
```java
// Use records for immutable data carriers
public record User(String id, String name, String email) {
    // Compact constructor for validation
    public User {
        Objects.requireNonNull(id, "id must not be null");
        Objects.requireNonNull(email, "email must not be null");
    }
}
```

### Sealed Classes (Java 17+)
```java
public sealed interface Shape 
    permits Circle, Rectangle, Triangle {
    double area();
}

public final class Circle implements Shape {
    private final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}
```

### Pattern Matching
```java
// Pattern matching for instanceof (Java 16+)
public String formatObject(Object obj) {
    if (obj instanceof String s) {
        return s.toUpperCase();
    } else if (obj instanceof Integer i) {
        return String.format("Number: %d", i);
    }
    return obj.toString();
}

// Pattern matching for switch (Java 17+)
public String describeShape(Shape shape) {
    return switch (shape) {
        case Circle c -> "Circle with radius " + c.radius();
        case Rectangle r -> "Rectangle " + r.width() + "x" + r.height();
        case Triangle t -> "Triangle";
    };
}
```

### Text Blocks (Java 15+)
```java
String json = """
    {
        "name": "John Doe",
        "email": "john@example.com",
        "age": 30
    }
    """;
```

## Class Design

### Single Responsibility Principle
```java
// Good - Single responsibility
public class UserValidator {
    public boolean isValid(User user) {
        return user.email() != null && 
               user.email().contains("@");
    }
}

public class UserRepository {
    public User save(User user) {
        // Database logic
    }
}
```

### Dependency Injection
```java
@Service
public class UserService {
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    @Autowired
    public UserService(
        UserRepository userRepository,
        EmailService emailService
    ) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
    
    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        emailService.sendWelcomeEmail(savedUser);
        return savedUser;
    }
}
```

## Error Handling

### Use Specific Exceptions
```java
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(String userId) {
        super("User not found: " + userId);
    }
}

public class ValidationException extends RuntimeException {
    public ValidationException(String message) {
        super(message);
    }
}
```

### Try-with-Resources
```java
public String readFile(String path) throws IOException {
    try (BufferedReader reader = new BufferedReader(
            new FileReader(path))) {
        return reader.lines()
                     .collect(Collectors.joining("\n"));
    }
}
```

### Optional for Null Safety
```java
public Optional<User> findUserById(String id) {
    User user = userRepository.findById(id);
    return Optional.ofNullable(user);
}

// Usage
findUserById("123")
    .map(User::name)
    .orElse("Unknown");
```

## Collections and Streams

### Use Streams for Data Processing
```java
List<String> activeUserNames = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .sorted()
    .collect(Collectors.toList());

// Group by
Map<String, List<User>> usersByRole = users.stream()
    .collect(Collectors.groupingBy(User::role));

// Sum and aggregate
int totalAge = users.stream()
    .mapToInt(User::age)
    .sum();
```

### Immutable Collections
```java
// Use List.of(), Set.of(), Map.of() for immutable collections
List<String> names = List.of("Alice", "Bob", "Charlie");
Set<Integer> numbers = Set.of(1, 2, 3);
Map<String, Integer> ages = Map.of(
    "Alice", 30,
    "Bob", 25
);
```

## Testing

### JUnit 5
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @Mock
    private EmailService emailService;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    @DisplayName("Should create user successfully")
    void shouldCreateUserSuccessfully() {
        // Given
        User user = new User("1", "John", "john@example.com");
        when(userRepository.save(any(User.class))).thenReturn(user);
        
        // When
        User result = userService.createUser(user);
        
        // Then
        assertNotNull(result);
        assertEquals("John", result.name());
        verify(emailService).sendWelcomeEmail(user);
    }
    
    @Test
    @DisplayName("Should throw exception for invalid email")
    void shouldThrowExceptionForInvalidEmail() {
        // Given
        User user = new User("1", "John", "invalid-email");
        
        // When & Then
        assertThrows(ValidationException.class, () -> {
            userService.createUser(user);
        });
    }
    
    @ParameterizedTest
    @ValueSource(strings = {"test@example.com", "user@domain.co"})
    void shouldAcceptValidEmails(String email) {
        assertTrue(EmailValidator.isValid(email));
    }
}
```

## Documentation

### JavaDoc
```java
/**
 * Service for managing user operations.
 * 
 * <p>This service handles user creation, updates, and deletion,
 * ensuring proper validation and notification.
 *
 * @author Solution8 Team
 * @version 1.0
 * @since 2026-01-01
 */
public class UserService {
    
    /**
     * Creates a new user in the system.
     *
     * @param user the user to create, must not be null
     * @return the created user with generated ID
     * @throws ValidationException if user data is invalid
     * @throws DuplicateUserException if user already exists
     */
    public User createUser(User user) {
        // Implementation
    }
}
```

## Project Structure (Maven)

```
project/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── solution8/
│   │   │           ├── model/
│   │   │           ├── repository/
│   │   │           ├── service/
│   │   │           └── controller/
│   │   └── resources/
│   │       ├── application.properties
│   │       └── logback.xml
│   └── test/
│       ├── java/
│       │   └── com/
│       │       └── solution8/
│       └── resources/
├── pom.xml
└── README.md
```

## Build Tools

### Maven (pom.xml)
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.solution8</groupId>
    <artifactId>user-service</artifactId>
    <version>1.0.0</version>
    
    <properties>
        <java.version>17</java.version>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>3.2.0</version>
        </dependency>
        
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

### Gradle (build.gradle)
```gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.0'
}

group = 'com.solution8'
version = '1.0.0'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
}

test {
    useJUnitPlatform()
}
```

## Code Quality Tools

- **Checkstyle**: Enforce coding standards
- **PMD**: Find common programming flaws
- **SpotBugs**: Static analysis for bugs
- **JaCoCo**: Code coverage

### Checkstyle Configuration
```xml
<!DOCTYPE module PUBLIC
    "-//Checkstyle//DTD Checkstyle Configuration 1.3//EN"
    "https://checkstyle.org/dtds/configuration_1_3.dtd">

<module name="Checker">
    <module name="TreeWalker">
        <module name="JavadocMethod"/>
        <module name="ConstantName"/>
        <module name="LocalFinalVariableName"/>
        <module name="LocalVariableName"/>
        <module name="MemberName"/>
        <module name="MethodName"/>
        <module name="PackageName"/>
        <module name="ParameterName"/>
        <module name="StaticVariableName"/>
        <module name="TypeName"/>
    </module>
</module>
```
