# Java Annotations

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Built-in and Custom Annotations

---

## 📋 Table of Contents

- [Introduction to Annotations](#introduction-to-annotations)
  - [What are Annotations?](#what-are-annotations)
  - [Why Use Annotations?](#why-use-annotations)
  - [Annotations vs XML Configuration](#annotations-vs-xml-configuration)
- [Built-in Java Annotations](#built-in-java-annotations)
  - [@Override](#override)
  - [@Deprecated](#deprecated)
  - [@SuppressWarnings](#suppresswarnings)
  - [@SafeVarargs](#safevarargs)
  - [@FunctionalInterface](#functionalinterface)
- [Meta-Annotations](#meta-annotations)
  - [@Retention](#retention)
  - [@Target](#target)
  - [@Documented](#documented)
  - [@Inherited](#inherited)
  - [@Repeatable](#repeatable)
- [Custom Annotations](#custom-annotations)
  - [Creating Custom Annotations](#creating-custom-annotations)
  - [Processing Custom Annotations](#processing-custom-annotations)
- [Framework Annotations](#framework-annotations)
  - [JAX-RS (RESTful Web Services)](#jax-rs-restful-web-services)
  - [Spring Framework](#spring-framework)
  - [JEE CDI (Contexts and Dependency Injection)](#jee-cdi-contexts-and-dependency-injection)
  - [JPA/Hibernate](#jpahibernate)
  - [JUnit Testing](#junit-testing)
  - [Servlet 3.0+](#servlet-30)
- [Interview Questions](#interview-questions)

---

## Introduction to Annotations

### What are Annotations?

**Annotations are metadata** - data about data. They provide information about code to:
- **Compilers**: For error checking and suppressing warnings
- **Build tools**: For generating code, XML files, and documentation
- **Runtime**: For examining and modifying behavior using reflection

**Key Characteristics:**
- Declared using `@interface` keyword
- Can have elements (like methods)
- Can be applied to classes, methods, fields, parameters, etc.
- Processed at compile-time, deployment-time, or runtime

### Why Use Annotations?

**1. Declarative Programming:**
```java
// Without annotations - imperative
public class UserServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        // Handle GET request
    }
}
// web.xml configuration needed

// With annotations - declarative
@WebServlet("/users")
public class UserServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        // Handle GET request
    }
}
// No web.xml needed!
```

**2. Reduced Boilerplate:**
```java
// Without annotations
public class UserService {
    private UserDAO userDAO;
    
    public void setUserDAO(UserDAO userDAO) {
        this.userDAO = userDAO;
    }
}
// XML: <bean><property name="userDAO" ref="userDAO"/></bean>

// With annotations
public class UserService {
    @Autowired
    private UserDAO userDAO;
}
// No XML needed!
```

**3. Type Safety:**
```java
// XML - no compile-time checking
<property name="maxConnections" value="ten"/> <!-- Runtime error! -->

// Annotations - compile-time checking
@Configuration(maxConnections = 10) // Type-safe
```

### Annotations vs XML Configuration

| Aspect | Annotations | XML |
|--------|------------|-----|
| **Location** | In source code | Separate file |
| **Coupling** | Tight (code + config together) | Loose (separate) |
| **Type Safety** | Yes (compile-time) | No (runtime) |
| **Refactoring** | IDE support | Manual updates |
| **Verbosity** | Less verbose | More verbose |
| **Flexibility** | Less flexible | More flexible |
| **Best For** | Code-specific config | Environment-specific config |

**When to Use Each:**

✓ **Use Annotations for:**
- Code-specific configuration (e.g., @Entity, @Service)
- Behavior tied to specific methods/classes
- Development-time configuration
- Type-safe configuration

✓ **Use XML for:**
- Environment-specific settings (URLs, credentials)
- Application-wide constants
- Configuration that changes per deployment
- Third-party library configuration

**Example - Spring Configuration:**

```xml
<!-- XML approach - good for environment-specific settings -->
<context:component-scan base-package="com.myapp.batch" />

<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="url" value="${db.url}"/>
    <property name="username" value="${db.username}"/>
</bean>
```

```java
// Annotation approach - good for code-specific configuration
@Configuration
@ComponentScan(basePackages = "com.myapp.batch")
public class AppConfig {
    
    @Bean
    public DataSource dataSource() {
        BasicDataSource ds = new BasicDataSource();
        ds.setUrl(env.getProperty("db.url"));
        ds.setUsername(env.getProperty("db.username"));
        return ds;
    }
}
```

---

## Built-in Java Annotations

### @Override

**Purpose**: Indicates that a method overrides a method in a superclass.

**Benefits:**
- Compile-time checking for typos
- Documents intent
- Prevents accidental overloading instead of overriding

```java
public class Parent {
    public String toString() {
        return "Parent";
    }
}

public class Child extends Parent {
    @Override
    public String toString() { // Correct override
        return "Child";
    }
    
    // Compilation error - method doesn't exist in parent
    // @Override
    // public String tostring() { // Typo: lowercase 's'
    //     return "Child";
    // }
}
```

**Common Use Cases:**

```java
public class Employee {
    private String name;
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Employee employee = (Employee) obj;
        return Objects.equals(name, employee.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
    
    @Override
    public String toString() {
        return "Employee{name='" + name + "'}";
    }
}
```

---

### @Deprecated

**Purpose**: Marks code that should no longer be used.

**Syntax (Java 9+):**
```java
@Deprecated(since = "1.5", forRemoval = true)
```

**Examples:**

```java
public class LegacyAPI {
    
    /**
     * @deprecated Use {@link #newMethod(String)} instead
     */
    @Deprecated
    public void oldMethod() {
        System.out.println("Old implementation");
    }
    
    public void newMethod(String param) {
        System.out.println("New implementation: " + param);
    }
    
    // Java 9+ - more informative
    @Deprecated(since = "2.0", forRemoval = true)
    public void veryOldMethod() {
        System.out.println("Will be removed in next major version");
    }
}

// Usage - compiler warning
public class Client {
    public void useAPI() {
        LegacyAPI api = new LegacyAPI();
        api.oldMethod(); // Warning: 'oldMethod()' is deprecated
    }
}
```

**Best Practices:**
- Always provide alternative in Javadoc
- Use `since` parameter to indicate when deprecated
- Use `forRemoval = true` if planning to remove
- Provide migration path for users

---

### @SuppressWarnings

**Purpose**: Suppresses compiler warnings.

**Common Warning Types:**

```java
public class WarningExamples {
    
    // Suppress unchecked warnings
    @SuppressWarnings("unchecked")
    public void uncheckedExample() {
        List rawList = new ArrayList(); // Raw type
        rawList.add("String");
        List<String> typedList = rawList; // Unchecked cast
    }
    
    // Suppress deprecation warnings
    @SuppressWarnings("deprecation")
    public void deprecationExample() {
        Date date = new Date();
        int year = date.getYear(); // Deprecated method
    }
    
    // Suppress multiple warnings
    @SuppressWarnings({"unchecked", "deprecation"})
    public void multipleWarnings() {
        // Code with multiple warning types
    }
    
    // Suppress all warnings (use sparingly!)
    @SuppressWarnings("all")
    public void suppressAll() {
        // All warnings suppressed
    }
}
```

**Common Warning Types:**
- `unchecked` - Unchecked operations
- `deprecation` - Deprecated API usage
- `rawtypes` - Raw type usage
- `unused` - Unused code
- `serial` - Missing serialVersionUID
- `all` - All warnings (use carefully!)

**Best Practice:**
```java
// ✓ Good - specific scope
public class GoodExample {
    @SuppressWarnings("unchecked")
    private List<String> getList() {
        return (List<String>) getRawList();
    }
}

// ✗ Bad - too broad
@SuppressWarnings("all") // Suppresses everything!
public class BadExample {
    // All warnings in entire class suppressed
}
```

---

### @SafeVarargs

**Purpose**: Suppresses warnings about potentially unsafe varargs operations.

```java
public class VarargsExample {
    
    // Without @SafeVarargs - compiler warning
    public static <T> List<T> asList(T... elements) {
        List<T> list = new ArrayList<>();
        for (T element : elements) {
            list.add(element);
        }
        return list;
    }
    
    // With @SafeVarargs - no warning
    @SafeVarargs
    public static <T> List<T> asListSafe(T... elements) {
        List<T> list = new ArrayList<>();
        for (T element : elements) {
            list.add(element);
        }
        return list;
    }
    
    // Usage
    public static void main(String[] args) {
        List<String> list = asListSafe("A", "B", "C");
    }
}
```

**Requirements:**
- Method must be `static`, `final`, or `private`
- Method must not perform unsafe operations on varargs array

---

### @FunctionalInterface

**Purpose**: Indicates that an interface is intended to be a functional interface (SAM - Single Abstract Method).

```java
@FunctionalInterface
public interface Calculator {
    int calculate(int a, int b);
    
    // Default methods allowed
    default int add(int a, int b) {
        return a + b;
    }
    
    // Static methods allowed
    static int multiply(int a, int b) {
        return a * b;
    }
    
    // Compilation error - only one abstract method allowed
    // int subtract(int a, int b);
}

// Usage with lambda
public class FunctionalExample {
    public static void main(String[] args) {
        Calculator add = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;
        
        System.out.println(add.calculate(5, 3));      // 8
        System.out.println(multiply.calculate(5, 3)); // 15
    }
}
```

---

## Meta-Annotations

Meta-annotations are annotations that apply to other annotations.

### @Retention

**Purpose**: Specifies how long annotations are retained.

**Retention Policies:**

```java
import java.lang.annotation.*;

// SOURCE - Discarded by compiler
@Retention(RetentionPolicy.SOURCE)
public @interface SourceAnnotation {
    // Used by tools like Lombok, not in bytecode
}

// CLASS - In bytecode, not available at runtime (default)
@Retention(RetentionPolicy.CLASS)
public @interface ClassAnnotation {
    // Available for bytecode processors
}

// RUNTIME - Available at runtime via reflection
@Retention(RetentionPolicy.RUNTIME)
public @interface RuntimeAnnotation {
    // Can be read at runtime
}
```

**Examples:**

```java
// Compile-time only
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.METHOD)
public @interface Override {
}

// Runtime processing
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
    long timeout() default 0L;
}
```

---

### @Target

**Purpose**: Specifies where an annotation can be applied.

**Element Types:**

```java
import java.lang.annotation.*;

// Single target
@Target(ElementType.METHOD)
public @interface MethodOnly {
}

// Multiple targets
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface TypeOrMethod {
}

// All element types
@Target({
    ElementType.TYPE,           // Class, interface, enum
    ElementType.FIELD,          // Field (including enum constants)
    ElementType.METHOD,         // Method
    ElementType.PARAMETER,      // Method parameter
    ElementType.CONSTRUCTOR,    // Constructor
    ElementType.LOCAL_VARIABLE, // Local variable
    ElementType.ANNOTATION_TYPE,// Annotation type
    ElementType.PACKAGE,        // Package
    ElementType.TYPE_PARAMETER, // Type parameter (Java 8+)
    ElementType.TYPE_USE        // Any use of a type (Java 8+)
})
public @interface Everywhere {
}
```

**Examples:**

```java
// Method-only annotation
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Transactional {
    String value() default "";
}

// Field-only annotation
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Inject {
}

// Usage
public class Service {
    @Inject
    private Repository repository;
    
    @Transactional
    public void saveData() {
        // Method implementation
    }
}
```

---

### @Documented

**Purpose**: Indicates that annotations should be included in Javadoc.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Author {
    String name();
    String date();
}

// Usage
public class MyClass {
    /**
     * Processes user data
     */
    @Author(name = "John Doe", date = "2024-01-15")
    public void processData() {
        // Implementation
    }
}
// @Author will appear in generated Javadoc
```

---

### @Inherited

**Purpose**: Indicates that an annotation is automatically inherited by subclasses.

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Auditable {
    String value() default "";
}

@Auditable("Parent class")
public class Parent {
}

// Child automatically inherits @Auditable
public class Child extends Parent {
}

// Check at runtime
public class InheritanceTest {
    public static void main(String[] args) {
        System.out.println(Parent.class.isAnnotationPresent(Auditable.class)); // true
        System.out.println(Child.class.isAnnotationPresent(Auditable.class));  // true
    }
}
```

**Important**: Only works for class inheritance, not interface implementation or method overriding.

---

### @Repeatable

**Purpose**: Allows an annotation to be applied multiple times (Java 8+).

```java
// Container annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Schedules {
    Schedule[] value();
}

// Repeatable annotation
@Repeatable(Schedules.class)
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Schedule {
    String day();
    String time();
}

// Usage
public class TaskScheduler {
    @Schedule(day = "Monday", time = "09:00")
    @Schedule(day = "Wednesday", time = "14:00")
    @Schedule(day = "Friday", time = "16:00")
    public void runTask() {
        System.out.println("Task executed");
    }
}

// Reading repeatable annotations
public class ScheduleReader {
    public static void main(String[] args) throws Exception {
        Method method = TaskScheduler.class.getMethod("runTask");
        Schedule[] schedules = method.getAnnotationsByType(Schedule.class);
        
        for (Schedule schedule : schedules) {
            System.out.println(schedule.day() + " at " + schedule.time());
        }
    }
}
```

---

## Custom Annotations

### Creating Custom Annotations

**Basic Structure:**

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    // Elements (like methods)
    String value();                    // Required
    int priority() default 1;          // Optional with default
    String[] tags() default {};        // Array with default
}
```

**Example 1: @ToDo Annotation**

```java
@Documented
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.TYPE})
public @interface ToDo {
    String value();                           // Short description
    String assignedTo() default "Unassigned";
    String dateAssigned() default "Unknown";
    Priority priority() default Priority.MEDIUM;
    String[] tags() default {};
    
    enum Priority {
        LOW, MEDIUM, HIGH, CRITICAL
    }
}

// Usage
@ToDo(
    value = "Implement caching mechanism",
    assignedTo = "John Doe",
    dateAssigned = "2024-01-15",
    priority = ToDo.Priority.HIGH,
    tags = {"performance", "optimization"}
)
public class CacheService {
    
    @ToDo(
        value = "Add input validation",
        priority = ToDo.Priority.CRITICAL
    )
    public void processData(String data) {
        // Implementation
    }
}
```

**Example 2: @DeadlockRetry Annotation**

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Inherited
public @interface DeadlockRetry {
    int maxTries() default 10;
    int tryIntervalMillis() default 1000;
}

// Usage
public interface AccountService {
    @DeadlockRetry(maxTries = 10, tryIntervalMillis = 5000)
    Account getAccount(String accountNumber);
    
    @DeadlockRetry(maxTries = 5, tryIntervalMillis = 2000)
    void updateAccount(Account account);
}
```

**Example 3: @Validate Annotation**

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Validate {
    int minLength() default 0;
    int maxLength() default Integer.MAX_VALUE;
    String pattern() default "";
    boolean required() default false;
}

// Usage
public class User {
    @Validate(required = true, minLength = 3, maxLength = 50)
    private String username;
    
    @Validate(required = true, pattern = "^[A-Za-z0-9+_.-]+@(.+)$")
    private String email;
    
    @Validate(minLength = 8, maxLength = 100)
    private String password;
}
```

---

### Processing Custom Annotations

**Runtime Processing with Reflection:**

```java
public class AnnotationProcessor {
    
    // Process @ToDo annotations
    public static void generateToDoReport(Class<?> clazz) {
        System.out.println("=== TODO Report for " + clazz.getSimpleName() + " ===\n");
        
        // Check class-level annotation
        if (clazz.isAnnotationPresent(ToDo.class)) {
            ToDo todo = clazz.getAnnotation(ToDo.class);
            printToDo("Class", clazz.getSimpleName(), todo);
        }
        
        // Check method-level annotations
        for (Method method : clazz.getDeclaredMethods()) {
            if (method.isAnnotationPresent(ToDo.class)) {
                ToDo todo = method.getAnnotation(ToDo.class);
                printToDo("Method", method.getName(), todo);
            }
        }
    }
    
    private static void printToDo(String type, String name, ToDo todo) {
        System.out.println(type + ": " + name);
        System.out.println("  Description: " + todo.value());
        System.out.println("  Assigned To: " + todo.assignedTo());
        System.out.println("  Date: " + todo.dateAssigned());
        System.out.println("  Priority: " + todo.priority());
        System.out.println("  Tags: " + String.join(", ", todo.tags()));
        System.out.println();
    }
    
    // Process @Validate annotations
    public static <T> void validate(T object) throws ValidationException {
        Class<?> clazz = object.getClass();
        
        for (Field field : clazz.getDeclaredFields()) {
            if (field.isAnnotationPresent(Validate.class)) {
                Validate validate = field.getAnnotation(Validate.class);
                field.setAccessible(true);
                
                try {
                    Object value = field.get(object);
                    
                    // Check required
                    if (validate.required() && value == null) {
                        throw new ValidationException(
                            field.getName() + " is required");
                    }
                    
                    // Check string validations
                    if (value instanceof String) {
                        String strValue = (String) value;
                        
                        if (strValue.length() < validate.minLength()) {
                            throw new ValidationException(
                                field.getName() + " is too short");
                        }
                        
                        if (strValue.length() > validate.maxLength()) {
                            throw new ValidationException(
                                field.getName() + " is too long");
                        }
                        
                        if (!validate.pattern().isEmpty() && 
                            !strValue.matches(validate.pattern())) {
                            throw new ValidationException(
                                field.getName() + " has invalid format");
                        }
                    }
                } catch (IllegalAccessException e) {
                    throw new ValidationException("Cannot access field: " + 
                        field.getName(), e);
                }
            }
        }
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // Generate TODO report
        AnnotationProcessor.generateToDoReport(CacheService.class);
        
        // Validate object
        User user = new User();
        user.setUsername("jo"); // Too short
        user.setEmail("invalid-email");
        
        try {
            AnnotationProcessor.validate(user);
        } catch (ValidationException e) {
            System.err.println("Validation failed: " + e.getMessage());
        }
    }
}
```

**Dynamic Proxy for Method Interception:**

```java
public class DeadlockRetryHandler implements InvocationHandler {
    private Object target;
    
    public DeadlockRetryHandler(Object target) {
        this.target = target;
    }
    
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) 
            throws Throwable {
        
        if (!method.isAnnotationPresent(DeadlockRetry.class)) {
            return method.invoke(target, args);
        }
        
        DeadlockRetry annotation = method.getAnnotation(DeadlockRetry.class);
        int maxTries = annotation.maxTries();
        long interval = annotation.tryIntervalMillis();
        
        int attempt = 0;
        while (attempt < maxTries) {
            try {
                attempt++;
                return method.invoke(target, args);
            } catch (InvocationTargetException e) {
                Throwable cause = e.getCause();
                
                if (!isDeadlock(cause)) {
                    throw cause;
                }
                
                if (attempt >= maxTries) {
                    throw new RuntimeException(
                        "Failed after " + maxTries + " attempts", cause);
                }
                
                System.out.println("Deadlock detected, retrying... (attempt " + 
                    attempt + "/" + maxTries + ")");
                
                if (interval > 0) {
                    Thread.sleep(interval);
                }
            }
        }
        
        throw new RuntimeException("Should not reach here");
    }
    
    private boolean isDeadlock(Throwable e) {
        // Check if exception is deadlock-related
        return e instanceof SQLException && 
               e.getMessage().contains("deadlock");
    }
}

// Create proxy
AccountService service = new AccountServiceImpl();
AccountService proxy = (AccountService) Proxy.newProxyInstance(
    service.getClass().getClassLoader(),
    service.getClass().getInterfaces(),
    new DeadlockRetryHandler(service)
);

// Use proxy - automatic retry on deadlock
Account account = proxy.getAccount("12345");
```

---

## Framework Annotations

### JAX-RS (RESTful Web Services)

```java
@Path("userservice/1.0")
@Produces("application/json")
@Consumes("application/json")
public class UserWebService {
    
    @GET
    @Path("/user/{id}")
    public User getUser(@PathParam("id") String userId) {
        return userService.findById(userId);
    }
    
    @POST
    @Path("/user")
    public Response createUser(User user) {
        userService.save(user);
        return Response.status(201).entity(user).build();
    }
    
    @PUT
    @Path("/user/{id}")
    public Response updateUser(
            @PathParam("id") String userId,
            User user) {
        userService.update(userId, user);
        return Response.ok(user).build();
    }
    
    @DELETE
    @Path("/user/{id}")
    public Response deleteUser(@PathParam("id") String userId) {
        userService.delete(userId);
        return Response.noContent().build();
    }
    
    @GET
    @Path("/users")
    public List<User> searchUsers(
            @QueryParam("name") String name,
            @QueryParam("age") Integer age,
            @DefaultValue("10") @QueryParam("limit") int limit) {
        return userService.search(name, age, limit);
    }
}
```

**Common JAX-RS Annotations:**
- `@Path` - URL path mapping
- `@GET`, `@POST`, `@PUT`, `@DELETE` - HTTP methods
- `@Produces` - Response content type
- `@Consumes` - Request content type
- `@PathParam` - URL path parameters
- `@QueryParam` - Query string parameters
- `@HeaderParam` - HTTP header parameters
- `@DefaultValue` - Default parameter values

---

### Spring Framework

**Dependency Injection:**

```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    // Constructor injection (preferred)
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    // Qualifier for multiple beans
    @Autowired
    @Qualifier("primaryDataSource")
    private DataSource dataSource;
}

@Repository
public class UserRepository {
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public User findById(Long id) {
        return jdbcTemplate.queryForObject(
            "SELECT * FROM users WHERE id = ?",
            new Object[]{id},
            new UserRowMapper()
        );
    }
}
```

**Configuration:**

```java
@Configuration
@ComponentScan(
    basePackages = {
        "com.myapp.service",
        "com.myapp.repository"
    },
    includeFilters = @ComponentScan.Filter(
        type = FilterType.ANNOTATION,
        value = Repository.class
    )
)
@PropertySource("classpath:application.properties")
public class AppConfig {
    
    @Value("${db.url}")
    private String dbUrl;
    
    @Bean
    public DataSource dataSource() {
        BasicDataSource ds = new BasicDataSource();
        ds.setUrl(dbUrl);
        return ds;
    }
    
    @Bean
    @Primary
    public UserService userService() {
        return new UserServiceImpl();
    }
}
```

**Spring MVC:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody @Valid User user) {
        User created = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<String> handleNotFound(UserNotFoundException e) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(e.getMessage());
    }
}
```

**Common Spring Annotations:**
- `@Component`, `@Service`, `@Repository`, `@Controller` - Stereotype annotations
- `@Autowired`, `@Inject`, `@Resource` - Dependency injection
- `@Qualifier` - Bean selection
- `@Configuration`, `@Bean` - Java-based configuration
- `@Value` - Property injection
- `@Transactional` - Transaction management
- `@Async` - Asynchronous execution
- `@Scheduled` - Task scheduling

---

### JEE CDI (Contexts and Dependency Injection)

**Basic Injection:**

```java
// beans.xml required in META-INF or WEB-INF
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://xmlns.jcp.org/xml/ns/javaee"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
                           http://xmlns.jcp.org/xml/ns/javaee/beans_1_1.xsd"
       bean-discovery-mode="all">
</beans>
```

```java
// Default implementation
@Default
public class CashPaymentService implements PaymentService {
    public void processPayment(BigDecimal amount, Account account) {
        System.out.println("Processing cash payment: " + amount);
    }
}

// Alternative implementations
@Alternative
public class BPayPaymentService implements PaymentService {
    public void processPayment(BigDecimal amount, Account account) {
        System.out.println("Processing BPay payment: " + amount);
    }
}

@Alternative
public class CreditCardPaymentService implements PaymentService {
    public void processPayment(BigDecimal amount, Account account) {
        System.out.println("Processing credit card payment: " + amount);
    }
}

// Injection
public class PaymentProcessor {
    @Inject
    private PaymentService paymentService; // Injects @Default
    
    // Constructor injection
    @Inject
    public PaymentProcessor(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

**Qualifiers:**

```java
// Define qualifiers
@Qualifier
@Retention(RUNTIME)
@Target({TYPE, METHOD, FIELD, PARAMETER})
public @interface BPay {
}

@Qualifier
@Retention(RUNTIME)
@Target({TYPE, METHOD, FIELD, PARAMETER})
public @interface CreditCard {
}

// Apply to implementations
@BPay
public class BPayPaymentService implements PaymentService {
    // Implementation
}

@CreditCard
public class CreditCardPaymentService implements PaymentService {
    // Implementation
}

// Inject specific implementation
public class PaymentProcessor {
    @Inject @BPay
    private PaymentService bpayService;
    
    @Inject @CreditCard
    private PaymentService ccService;
    
    private PaymentService activeService;
    
    @PostConstruct
    public void init() {
        // Choose service based on configuration
        if (config.isBPayEnabled()) {
            activeService = bpayService;
        } else {
            activeService = ccService;
        }
    }
}
```

**Producers:**

```java
public class PaymentFactory {
    
    @Produces
    @RequestScoped
    public PaymentService createPaymentService(Account account) {
        if (account.isBPay()) {
            return new BPayPaymentService();
        } else {
            return new CreditCardPaymentService();
        }
    }
    
    @Produces
    @Named("maxRetries")
    public int getMaxRetries() {
        return 3;
    }
}

// Injection
public class PaymentProcessor {
    @Inject
    private PaymentService paymentService; // From producer
    
    @Inject @Named("maxRetries")
    private int maxRetries;
}
```

**Scopes:**
- `@RequestScoped` - HTTP request
- `@SessionScoped` - HTTP session
- `@ApplicationScoped` - Application lifetime
- `@ConversationScoped` - User conversation
- `@Dependent` - Dependent on injecting bean (default)

---

### JPA/Hibernate

```java
@Entity
@Table(name = "users")
public class User {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "username", nullable = false, unique = true, length = 50)
    private String username;
    
    @Column(name = "email", nullable = false)
    private String email;
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "created_at")
    private Date createdAt;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders;
    
    @ManyToOne(fetch = FetchType.EAGER)
    @JoinColumn(name = "role_id")
    private Role role;
    
    @Transient
    private String temporaryData; // Not persisted
    
    @Version
    private Long version; // Optimistic locking
}
```

---

### JUnit Testing

```java
public class UserServiceTest {
    
    @Test
    public void testFindUser() {
        User user = userService.findById(1L);
        assertNotNull(user);
        assertEquals("john", user.getUsername());
    }
    
    @Test(timeout = 100)
    public void testPerformance() {
        // Must complete within 100ms
        userService.quickOperation();
    }
    
    @Test(expected = UserNotFoundException.class)
    public void testNotFound() {
        userService.findById(999L); // Should throw exception
    }
    
    @Before
    public void setUp() {
        // Run before each test
    }
    
    @After
    public void tearDown() {
        // Run after each test
    }
    
    @BeforeClass
    public static void setUpClass() {
        // Run once before all tests
    }
    
    @AfterClass
    public static void tearDownClass() {
        // Run once after all tests
    }
    
    @Ignore("Not implemented yet")
    @Test
    public void testFutureFeature() {
        // Skipped
    }
}

// CDI Testing
@RunWith(CdiRunner.class)
public class PaymentServiceTest {
    
    @Inject
    private PaymentService paymentService;
    
    @Test
    public void testPayment() {
        paymentService.processPayment(new BigDecimal("100.00"), account);
    }
}
```

---

### Servlet 3.0+

```java
@WebServlet(
    name = "UserServlet",
    urlPatterns = {"/users", "/user/*"},
    initParams = {
        @WebInitParam(name = "encoding", value = "UTF-8"),
        @WebInitParam(name = "debug", value = "true")
    },
    loadOnStartup = 1
)
public class UserServlet extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        // Handle GET request
    }
}

@WebFilter(
    filterName = "AuthFilter",
    urlPatterns = "/*",
    initParams = @WebInitParam(name = "excludeUrls", value = "/login,/public")
)
public class AuthenticationFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
                        FilterChain chain) throws IOException, ServletException {
        // Filter logic
        chain.doFilter(request, response);
    }
}

@WebListener
public class AppContextListener implements ServletContextListener {
    
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("Application started");
    }
    
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("Application stopped");
    }
}
```

---

## Interview Questions

### Q1: Are annotations a compile-time or runtime feature?

**Answer:**

Annotations can be **both compile-time and runtime features**, depending on their `@Retention` policy.

**Compile-Time Annotations:**

```java
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.METHOD)
public @interface Override {
}

// Usage
public class Child extends Parent {
    @Override // Checked at compile-time
    public String toString() {
        return "Child";
    }
}
```

**Runtime Annotations:**

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
    long timeout() default 0L;
}

// Usage - JUnit processes at runtime
public class MyTest {
    @Test(timeout = 1000)
    public void testMethod() {
        // Test logic
    }
}
```

**Summary:**

| Retention | When Processed | Example | Use Case |
|-----------|---------------|---------|----------|
| SOURCE | Compile-time | @Override, @SuppressWarnings | Compiler checks |
| CLASS | Post-compile | Bytecode processors | Code generation |
| RUNTIME | Runtime | @Test, @Autowired | Reflection-based frameworks |

---

### Q2: Are marker interfaces obsolete with annotations?

**Answer:**

**Mostly yes, but not entirely.** Annotations are generally preferred, but marker interfaces still have specific use cases.

**Advantages of Annotations over Marker Interfaces:**

1. **No Unwanted Inheritance:**
```java
// Marker interface - all subclasses inherit
public interface Serializable {
}

public class Parent implements Serializable {
}

public class Child extends Parent {
    // Automatically Serializable - can't prevent it
}

// Annotation - explicit per class
@Serializable
public class Parent {
}

public class Child extends Parent {
    // Not automatically @Serializable
}
```

2. **More Flexible:**
```java
// Annotations can have parameters
@Cacheable(timeout = 3600, region = "users")
public class User {
}

// Marker interfaces cannot
```

3. **Multiple Markers:**
```java
// Can apply multiple annotations
@Serializable
@Cacheable
@Auditable
public class User {
}

// Can only implement one marker interface per declaration
```

**When Marker Interfaces Are Still Useful:**

1. **Type Safety at Compile-Time:**
```java
// Marker interface
public interface Comparable<T> {
    int compareTo(T o);
}

public <T extends Comparable<T>> void sort(List<T> list) {
    // Compile-time type checking
}

// Annotation - no compile-time checking
public void sort(@Sortable List<?> list) {
    // Must check at runtime
}
```

2. **Existing APIs:**
```java
// Still widely used
Serializable, Cloneable, RandomAccess, Remote, EventListener
```

**Conclusion:**
- **Use annotations** for new code (more flexible)
- **Keep marker interfaces** for type constraints and existing APIs
- **Migrate gradually** when refactoring legacy code

---

### Q3: Why are annotations popular in frameworks?

**Answer:**

Annotations enable **declarative programming** and reduce boilerplate code significantly.

**1. Reduced Configuration:**

```java
// Pre-annotations (Spring 2.x) - XML hell
<beans>
    <bean id="userService" class="com.myapp.UserService">
        <property name="userDAO" ref="userDAO"/>
    </bean>
    <bean id="userDAO" class="com.myapp.UserDAO">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="url" value="jdbc:mysql://localhost/mydb"/>
        <property name="username" value="root"/>
    </bean>
</beans>

// With annotations (Spring 3.x+) - Clean and concise
@Service
public class UserService {
    @Autowired
    private UserDAO userDAO;
}

@Repository
public class UserDAO {
    @Autowired
    private DataSource dataSource;
}

@Configuration
public class AppConfig {
    @Bean
    public DataSource dataSource() {
        BasicDataSource ds = new BasicDataSource();
        ds.setUrl("jdbc:mysql://localhost/mydb");
        ds.setUsername("root");
        return ds;
    }
}
```

**2. Type Safety:**

```java
// XML - runtime errors
<property name="maxConnections" value="ten"/> <!-- Oops! -->

// Annotations - compile-time checking
@Configuration(maxConnections = 10) // Type-safe
```

**3. Better IDE Support:**

```java
// Annotations - IDE can:
// - Auto-complete
// - Navigate to definition
// - Refactor safely
// - Find usages

@Autowired
private UserService userService; // Ctrl+Click to navigate
```

**4. Co-location:**

```java
// Configuration with code - easier to understand
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}

// vs separate XML file - harder to maintain
```

**When to Use XML vs Annotations:**

| Use Case | Prefer |
|----------|--------|
| Code-specific config | Annotations |
| Environment-specific config | XML/Properties |
| Third-party libraries | XML |
| Rapid prototyping | Annotations |
| Multiple environments | XML |

---

### Q4: How do you create and process custom annotations?

**Answer:**

See the [Custom Annotations](#custom-annotations) section above for detailed examples of:
- Creating custom annotations with `@interface`
- Using meta-annotations (@Retention, @Target, etc.)
- Processing annotations at runtime with reflection
- Using dynamic proxies for method interception

---

## 📚 Related Topics

- [Java Modifiers](./java-modifiers.md)
- [Annotation Processing](./annotation-processing.md)
- [Reflection API](../Module%2005%20-%20Java%20Objects/)
- [Design Patterns](../Module%2015%20-%20Design%20Patterns/)

---

## 💡 Key Takeaways

**Annotations Basics:**
- Annotations are metadata for code
- Can be processed at compile-time, deployment-time, or runtime
- Defined with `@interface` keyword
- Can have elements with default values

**Meta-Annotations:**
- `@Retention` - When annotation is available (SOURCE, CLASS, RUNTIME)
- `@Target` - Where annotation can be applied
- `@Documented` - Include in Javadoc
- `@Inherited` - Inherited by subclasses
- `@Repeatable` - Can be applied multiple times

**Best Practices:**
- Use annotations for code-specific configuration
- Use XML for environment-specific configuration
- Keep annotations simple and focused
- Document custom annotations thoroughly
- Use appropriate retention policy
- Specify target elements explicitly

**Framework Usage:**
- Spring: @Autowired, @Service, @Controller, @Configuration
- JPA: @Entity, @Table, @Column, @Id
- JAX-RS: @Path, @GET, @POST, @Produces
- JUnit: @Test, @Before, @After
- CDI: @Inject, @Qualifier, @Produces

---

**[⬆ Back to Top](#)**