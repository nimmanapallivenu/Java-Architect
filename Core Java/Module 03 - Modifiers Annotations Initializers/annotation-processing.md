# Annotation Processing

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Annotation Types, Processing, and Processors

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Annotation Terminology](#annotation-terminology)
  - [Annotation Types](#annotation-types)
  - [Annotations](#annotations)
  - [Annotation Processors](#annotation-processors)
- [Annotation Processing Lifecycle](#annotation-processing-lifecycle)
  - [Compile-Time Processing](#compile-time-processing)
  - [Runtime Processing](#runtime-processing)
  - [Post-Compile Processing](#post-compile-processing)
- [Annotation Type Categories](#annotation-type-categories)
  - [Marker Annotations](#marker-annotations)
  - [Single-Element Annotations](#single-element-annotations)
  - [Multi-Element Annotations](#multi-element-annotations)
- [Meta-Annotations Deep Dive](#meta-annotations-deep-dive)
  - [@Retention Policy](#retention-policy)
  - [@Target Elements](#target-elements)
  - [@Documented](#documented)
  - [@Inherited](#inherited)
- [Creating Annotation Processors](#creating-annotation-processors)
  - [Compile-Time Processors (APT)](#compile-time-processors-apt)
  - [Runtime Processors (Reflection)](#runtime-processors-reflection)
  - [Bytecode Processors](#bytecode-processors)
- [Real-World Examples](#real-world-examples)
  - [Code Generation](#code-generation)
  - [Validation Framework](#validation-framework)
  - [Dependency Injection](#dependency-injection)
- [Best Practices](#best-practices)
- [Interview Questions](#interview-questions)

---

## Introduction

**Annotation processing** is the mechanism by which annotations are read and acted upon by various tools and frameworks. Understanding annotation processing is crucial for:

- Creating custom annotations
- Building frameworks and libraries
- Generating boilerplate code
- Implementing compile-time validation
- Runtime behavior modification

---

## Annotation Terminology

### Annotation Types

**Annotation types** are the definitions/templates for annotations. They define:
- What information the annotation carries
- Where it can be applied
- When it's available
- How it behaves

```java
// This is an ANNOTATION TYPE definition
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
    long timeout() default 0L;
    Class<? extends Throwable> expected() default Test.None.class;
    
    class None extends Throwable {
        private None() {}
    }
}
```

**Key Components:**
- `@interface` keyword - Declares an annotation type
- Elements - Methods that define annotation parameters
- Default values - Optional default values for elements
- Meta-annotations - Annotations on the annotation type

### Annotations

**Annotations** are the actual usage/instances of annotation types in code.

```java
// These are ANNOTATIONS (instances of annotation types)
public class MyTest {
    
    @Test  // Using annotation with defaults
    public void simpleTest() {
        // Test logic
    }
    
    @Test(timeout = 1000)  // Using annotation with custom value
    public void timedTest() {
        // Test logic
    }
    
    @Test(timeout = 500, expected = IllegalArgumentException.class)
    public void exceptionTest() {
        // Test logic
    }
}
```

### Annotation Processors

**Annotation processors** are tools/programs that read and act on annotations.

**Types of Processors:**

1. **Compiler** - Built into javac
2. **APT (Annotation Processing Tool)** - Compile-time code generation
3. **Runtime Frameworks** - Spring, Hibernate, JUnit (using reflection)
4. **Bytecode Processors** - ASM, Javassist (post-compile)
5. **IDEs** - Eclipse, IntelliJ (for code assistance)
6. **Build Tools** - Maven, Gradle plugins

```java
// Example: JUnit processes @Test at runtime
public class TestRunner {
    public void runTests(Class<?> testClass) {
        for (Method method : testClass.getDeclaredMethods()) {
            if (method.isAnnotationPresent(Test.class)) {
                Test test = method.getAnnotation(Test.class);
                // Process the annotation
                runTest(method, test.timeout(), test.expected());
            }
        }
    }
}
```

---

## Annotation Processing Lifecycle

### Compile-Time Processing

**When**: During compilation (javac)  
**How**: Annotation Processing Tool (APT) API  
**Purpose**: Code generation, validation, error checking

```java
// Annotation type for compile-time processing
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface GenerateBuilder {
    String className() default "";
}

// Usage
@GenerateBuilder
public class User {
    private String name;
    private int age;
}

// Processor generates UserBuilder.java at compile-time
// Generated code:
public class UserBuilder {
    private String name;
    private int age;
    
    public UserBuilder name(String name) {
        this.name = name;
        return this;
    }
    
    public UserBuilder age(int age) {
        this.age = age;
        return this;
    }
    
    public User build() {
        User user = new User();
        user.setName(name);
        user.setAge(age);
        return user;
    }
}
```

**Examples:**
- Lombok - Generates getters, setters, builders
- Dagger - Generates dependency injection code
- AutoValue - Generates value classes
- MapStruct - Generates mapper implementations

### Runtime Processing

**When**: During application execution  
**How**: Java Reflection API  
**Purpose**: Behavior modification, validation, configuration

```java
// Annotation type for runtime processing
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Transactional {
    String value() default "";
    int timeout() default -1;
}

// Usage
public class UserService {
    @Transactional(timeout = 30)
    public void saveUser(User user) {
        // Business logic
    }
}

// Runtime processor using reflection
public class TransactionInterceptor {
    public Object intercept(Method method, Object[] args) throws Throwable {
        if (method.isAnnotationPresent(Transactional.class)) {
            Transactional tx = method.getAnnotation(Transactional.class);
            
            // Start transaction
            Transaction transaction = beginTransaction(tx.timeout());
            
            try {
                Object result = method.invoke(target, args);
                transaction.commit();
                return result;
            } catch (Exception e) {
                transaction.rollback();
                throw e;
            }
        }
        return method.invoke(target, args);
    }
}
```

**Examples:**
- Spring - @Autowired, @Transactional, @Cacheable
- JUnit - @Test, @Before, @After
- JAX-RS - @Path, @GET, @POST
- JPA - @Entity, @Table, @Column

### Post-Compile Processing

**When**: After compilation, before runtime  
**How**: Bytecode manipulation (ASM, Javassist)  
**Purpose**: Code injection, aspect weaving, optimization

```java
// Annotation for bytecode processing
@Retention(RetentionPolicy.CLASS)
@Target(ElementType.METHOD)
public @interface Logged {
    String level() default "INFO";
}

// Original code
public class Service {
    @Logged(level = "DEBUG")
    public void process() {
        // Business logic
    }
}

// Bytecode processor injects logging code
// Resulting bytecode equivalent to:
public class Service {
    public void process() {
        Logger.log("DEBUG", "Entering process()");
        try {
            // Business logic
        } finally {
            Logger.log("DEBUG", "Exiting process()");
        }
    }
}
```

**Examples:**
- AspectJ - Aspect weaving
- Hibernate - Lazy loading bytecode enhancement
- JaCoCo - Code coverage instrumentation

---

## Annotation Type Categories

### Marker Annotations

**Definition**: Annotations with no elements (parameters).

```java
// Marker annotation definition
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Deprecated {
    // No elements
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Override {
    // No elements
}

// Custom marker annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Singleton {
    // No elements - just marks class as singleton
}

// Usage
@Singleton
public class DatabaseConnection {
    private static DatabaseConnection instance;
    
    private DatabaseConnection() {}
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
}

// Processing
public class SingletonValidator {
    public static void validate(Class<?> clazz) {
        if (clazz.isAnnotationPresent(Singleton.class)) {
            // Verify only one constructor
            // Verify constructor is private
            // Verify getInstance() method exists
        }
    }
}
```

**Characteristics:**
- No parentheses needed when using: `@Override`
- Acts as a flag or marker
- Presence/absence is the only information
- Simple and clean syntax

### Single-Element Annotations

**Definition**: Annotations with exactly one element, typically named `value`.

```java
// Single-element annotation definition
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Component {
    String value() default "";
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface RequestMapping {
    String value();  // Required
}

// Usage - shorthand syntax
@Component("userService")
public class UserService {
    // Equivalent to: @Component(value = "userService")
}

@RequestMapping("/users")
public void getUsers() {
    // Equivalent to: @RequestMapping(value = "/users")
}

// If element is not named 'value', must use full syntax
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Column {
    String name();  // Not 'value', so shorthand not available
}

// Must use full syntax
@Column(name = "user_name")
private String userName;
```

**Characteristics:**
- Element must be named `value` for shorthand syntax
- Can omit `value =` when using
- Can have default value
- Most common annotation pattern

### Multi-Element Annotations

**Definition**: Annotations with multiple elements.

```java
// Multi-element annotation definition
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Entity {
    String name() default "";
    String schema() default "";
    String catalog() default "";
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface RequestMapping {
    String[] value() default {};
    RequestMethod[] method() default {};
    String[] params() default {};
    String[] headers() default {};
    String[] consumes() default {};
    String[] produces() default {};
}

// Usage - must specify element names
@Entity(name = "users", schema = "public")
public class User {
}

@RequestMapping(
    value = {"/users", "/user"},
    method = {RequestMethod.GET, RequestMethod.POST},
    produces = "application/json"
)
public List<User> getUsers() {
    // Implementation
}

// Complex example with nested annotations
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Table {
    String name();
    String catalog() default "";
    String schema() default "";
    UniqueConstraint[] uniqueConstraints() default {};
    Index[] indexes() default {};
}

@Table(
    name = "users",
    schema = "public",
    uniqueConstraints = {
        @UniqueConstraint(columnNames = {"email"}),
        @UniqueConstraint(columnNames = {"username"})
    },
    indexes = {
        @Index(name = "idx_email", columnList = "email"),
        @Index(name = "idx_created", columnList = "created_at")
    }
)
public class User {
}
```

**Characteristics:**
- Multiple configuration options
- Must use `name = value` syntax
- Can have arrays of values
- Can have nested annotations
- More verbose but more flexible

---

## Meta-Annotations Deep Dive

### @Retention Policy

**Purpose**: Controls when annotation information is available.

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

// SOURCE - Discarded by compiler
@Retention(RetentionPolicy.SOURCE)
public @interface SourceLevel {
    // Available only in source code
    // Not in .class files
    // Not at runtime
    // Examples: @Override, @SuppressWarnings
}

// CLASS - In bytecode, not at runtime (DEFAULT)
@Retention(RetentionPolicy.CLASS)
public @interface ClassLevel {
    // Available in source code
    // Available in .class files
    // NOT available at runtime
    // Examples: Bytecode processors, build tools
}

// RUNTIME - Available at runtime
@Retention(RetentionPolicy.RUNTIME)
public @interface RuntimeLevel {
    // Available in source code
    // Available in .class files
    // Available at runtime via reflection
    // Examples: @Test, @Autowired, @Entity
}
```

**Decision Guide:**

```
Use SOURCE when:
✓ Compile-time checking only (@Override)
✓ IDE hints and warnings
✓ Code generation at compile-time
✓ No runtime overhead needed

Use CLASS when:
✓ Bytecode processing
✓ Build-time tools
✓ Not needed at runtime
✓ Default if not specified

Use RUNTIME when:
✓ Framework configuration (@Autowired)
✓ Runtime behavior modification
✓ Reflection-based processing
✓ Dynamic behavior needed
```

**Example - Choosing Retention:**

```java
// SOURCE - Compile-time validation only
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.METHOD)
public @interface Override {
    // Compiler checks method actually overrides
    // No need to keep in bytecode or runtime
}

// RUNTIME - Framework needs to read at runtime
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Entity {
    // JPA needs to read this at runtime
    // to map classes to database tables
}

// CLASS - Bytecode processor needs it
@Retention(RetentionPolicy.CLASS)
@Target(ElementType.METHOD)
public @interface Generated {
    // Build tools can read from .class files
    // Runtime doesn't need it
}
```

### @Target Elements

**Purpose**: Specifies where an annotation can be applied.

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

// TYPE - Classes, interfaces, enums, annotations
@Target(ElementType.TYPE)
public @interface Entity {
}

// FIELD - Fields (including enum constants)
@Target(ElementType.FIELD)
public @interface Column {
}

// METHOD - Methods
@Target(ElementType.METHOD)
public @interface Test {
}

// PARAMETER - Method parameters
@Target(ElementType.PARAMETER)
public @interface PathParam {
}

// CONSTRUCTOR - Constructors
@Target(ElementType.CONSTRUCTOR)
public @interface Inject {
}

// LOCAL_VARIABLE - Local variables
@Target(ElementType.LOCAL_VARIABLE)
public @interface SuppressWarnings {
}

// ANNOTATION_TYPE - Annotation types (meta-annotation)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
}

// PACKAGE - Package declarations
@Target(ElementType.PACKAGE)
public @interface PackageInfo {
}

// TYPE_PARAMETER - Type parameters (Java 8+)
@Target(ElementType.TYPE_PARAMETER)
public @interface NonNull {
}

// TYPE_USE - Any use of a type (Java 8+)
@Target(ElementType.TYPE_USE)
public @interface NotNull {
}

// Multiple targets
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})
public @interface Deprecated {
}
```

**Examples:**

```java
// TYPE usage
@Entity
@Table(name = "users")
public class User {
    
    // FIELD usage
    @Column(name = "user_name")
    private String userName;
    
    // CONSTRUCTOR usage
    @Inject
    public User(UserService service) {
    }
    
    // METHOD usage
    @Transactional
    public void save() {
    }
    
    // PARAMETER usage
    public User findById(@PathParam("id") Long id) {
        return null;
    }
    
    // LOCAL_VARIABLE usage
    public void process() {
        @SuppressWarnings("unchecked")
        List<String> list = (List<String>) getList();
    }
}

// TYPE_USE usage (Java 8+)
public class TypeUseExample {
    private @NotNull String name;  // Field type
    
    public @NotNull String getName() {  // Return type
        return name;
    }
    
    public void setName(@NotNull String name) {  // Parameter type
        this.name = name;
    }
    
    List<@NotNull String> names;  // Type argument
    
    @NotNull String @NotNull [] array;  // Array and elements
}
```

### @Documented

**Purpose**: Includes annotation in Javadoc documentation.

```java
import java.lang.annotation.Documented;

// Without @Documented - not in Javadoc
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Internal {
    String value();
}

// With @Documented - appears in Javadoc
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface PublicAPI {
    String since();
    String author();
}

// Usage
public class MyClass {
    
    @Internal("For internal use only")
    private void internalMethod() {
        // Not documented in Javadoc
    }
    
    /**
     * Public API method
     */
    @PublicAPI(since = "1.0", author = "John Doe")
    public void publicMethod() {
        // Annotation appears in Javadoc
    }
}
```

**When to Use:**
- ✓ Public APIs that users should see
- ✓ Important metadata for documentation
- ✗ Internal implementation details
- ✗ Framework-specific annotations

### @Inherited

**Purpose**: Annotation is inherited by subclasses.

```java
import java.lang.annotation.Inherited;

// With @Inherited
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Auditable {
    String value() default "";
}

// Without @Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Cacheable {
    int timeout() default 3600;
}

// Usage
@Auditable("Parent auditing")
@Cacheable(timeout = 1800)
public class Parent {
}

public class Child extends Parent {
    // Inherits @Auditable (has @Inherited)
    // Does NOT inherit @Cacheable (no @Inherited)
}

// Verification
public class InheritanceTest {
    public static void main(String[] args) {
        System.out.println("Parent has @Auditable: " + 
            Parent.class.isAnnotationPresent(Auditable.class));  // true
        System.out.println("Child has @Auditable: " + 
            Child.class.isAnnotationPresent(Auditable.class));   // true
        
        System.out.println("Parent has @Cacheable: " + 
            Parent.class.isAnnotationPresent(Cacheable.class));  // true
        System.out.println("Child has @Cacheable: " + 
            Child.class.isAnnotationPresent(Cacheable.class));   // false
    }
}
```

**Important Limitations:**
- Only works for **class inheritance**
- Does NOT work for:
  - Interface implementation
  - Method overriding
  - Field declarations

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Security {
}

// Works - class inheritance
@Security
public class Parent {
}

public class Child extends Parent {
    // Inherits @Security ✓
}

// Does NOT work - interface implementation
@Security
public interface MyInterface {
}

public class Implementation implements MyInterface {
    // Does NOT inherit @Security ✗
}
```

---

## Creating Annotation Processors

### Compile-Time Processors (APT)

**Purpose**: Generate code, validate annotations, produce errors/warnings during compilation.

**Step 1: Create Annotation**

```java
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface Builder {
    String className() default "";
}
```

**Step 2: Create Processor**

```java
import javax.annotation.processing.*;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.*;
import javax.tools.JavaFileObject;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Set;

@SupportedAnnotationTypes("com.example.Builder")
@SupportedSourceVersion(SourceVersion.RELEASE_8)
public class BuilderProcessor extends AbstractProcessor {
    
    @Override
    public boolean process(Set<? extends TypeElement> annotations, 
                          RoundEnvironment roundEnv) {
        
        for (Element element : roundEnv.getElementsAnnotatedWith(Builder.class)) {
            if (element.getKind() != ElementKind.CLASS) {
                processingEnv.getMessager().printMessage(
                    Diagnostic.Kind.ERROR,
                    "@Builder can only be applied to classes",
                    element
                );
                continue;
            }
            
            TypeElement typeElement = (TypeElement) element;
            Builder annotation = element.getAnnotation(Builder.class);
            
            try {
                generateBuilderClass(typeElement, annotation);
            } catch (IOException e) {
                processingEnv.getMessager().printMessage(
                    Diagnostic.Kind.ERROR,
                    "Failed to generate builder: " + e.getMessage(),
                    element
                );
            }
        }
        
        return true;
    }
    
    private void generateBuilderClass(TypeElement typeElement, Builder annotation) 
            throws IOException {
        
        String className = typeElement.getSimpleName().toString();
        String builderClassName = annotation.className().isEmpty() 
            ? className + "Builder" 
            : annotation.className();
        String packageName = processingEnv.getElementUtils()
            .getPackageOf(typeElement).toString();
        
        JavaFileObject builderFile = processingEnv.getFiler()
            .createSourceFile(packageName + "." + builderClassName);
        
        try (PrintWriter out = new PrintWriter(builderFile.openWriter())) {
            // Generate package
            if (!packageName.isEmpty()) {
                out.println("package " + packageName + ";");
                out.println();
            }
            
            // Generate class
            out.println("public class " + builderClassName + " {");
            
            // Generate fields
            for (Element enclosed : typeElement.getEnclosedElements()) {
                if (enclosed.getKind() == ElementKind.FIELD) {
                    VariableElement field = (VariableElement) enclosed;
                    out.println("    private " + field.asType() + " " + 
                        field.getSimpleName() + ";");
                }
            }
            
            out.println();
            
            // Generate setter methods
            for (Element enclosed : typeElement.getEnclosedElements()) {
                if (enclosed.getKind() == ElementKind.FIELD) {
                    VariableElement field = (VariableElement) enclosed;
                    String fieldName = field.getSimpleName().toString();
                    
                    out.println("    public " + builderClassName + " " + 
                        fieldName + "(" + field.asType() + " " + fieldName + ") {");
                    out.println("        this." + fieldName + " = " + fieldName + ";");
                    out.println("        return this;");
                    out.println("    }");
                    out.println();
                }
            }
            
            // Generate build method
            out.println("    public " + className + " build() {");
            out.println("        " + className + " obj = new " + className + "();");
            
            for (Element enclosed : typeElement.getEnclosedElements()) {
                if (enclosed.getKind() == ElementKind.FIELD) {
                    String fieldName = enclosed.getSimpleName().toString();
                    out.println("        obj.set" + capitalize(fieldName) + 
                        "(this." + fieldName + ");");
                }
            }
            
            out.println("        return obj;");
            out.println("    }");
            
            out.println("}");
        }
    }
    
    private String capitalize(String str) {
        return str.substring(0, 1).toUpperCase() + str.substring(1);
    }
}
```

**Step 3: Register Processor**

Create `META-INF/services/javax.annotation.processing.Processor`:

```
com.example.BuilderProcessor
```

**Step 4: Use Annotation**

```java
@Builder
public class User {
    private String name;
    private int age;
    private String email;
    
    // Getters and setters
}

// Generated UserBuilder.java is automatically created
// Usage:
User user = new UserBuilder()
    .name("John")
    .age(30)
    .email("john@example.com")
    .build();
```

### Runtime Processors (Reflection)

**Purpose**: Modify behavior, validate data, inject dependencies at runtime.

```java
// Annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Inject {
    String value() default "";
}

// Processor
public class DependencyInjector {
    
    private Map<String, Object> beans = new HashMap<>();
    
    public void registerBean(String name, Object bean) {
        beans.put(name, bean);
    }
    
    public <T> T createInstance(Class<T> clazz) throws Exception {
        T instance = clazz.getDeclaredConstructor().newInstance();
        
        // Inject dependencies
        for (Field field : clazz.getDeclaredFields()) {
            if (field.isAnnotationPresent(Inject.class)) {
                Inject inject = field.getAnnotation(Inject.class);
                String beanName = inject.value().isEmpty() 
                    ? field.getType().getSimpleName().toLowerCase()
                    : inject.value();
                
                Object bean = beans.get(beanName);
                if (bean == null) {
                    throw new RuntimeException(
                        "No bean found for: " + beanName);
                }
                
                field.setAccessible(true);
                field.set(instance, bean);
            }
        }
        
        return instance;
    }
}

// Usage
public class UserService {
    @Inject
    private UserRepository userRepository;
    
    @Inject("emailService")
    private EmailService emailService;
}

// Bootstrap
DependencyInjector injector = new DependencyInjector();
injector.registerBean("userrepository", new UserRepositoryImpl());
injector.registerBean("emailService", new EmailServiceImpl());

UserService service = injector.createInstance(UserService.class);
```

### Bytecode Processors

**Purpose**: Modify compiled bytecode for aspects, instrumentation, optimization.

```java
// Using Javassist
import javassist.*;

public class LoggingEnhancer {
    
    public static void enhance(String className) throws Exception {
        ClassPool pool = ClassPool.getDefault();
        CtClass ctClass = pool.get(className);
        
        for (CtMethod method : ctClass.getDeclaredMethods()) {
            if (method.hasAnnotation(Logged.class)) {
                Logged logged = (Logged) method.getAnnotation(Logged.class);
                String level = logged.level();
                
                // Add logging before method
                method.insertBefore(
                    "System.out.println(\"[" + level + "] Entering: " + 
                    method.getName() + "\");"
                );
                
                // Add logging after method
                method.insertAfter(
                    "System.out.println(\"[" + level + "] Exiting: " + 
                    method.getName() + "\");"
                );
            }
        }
        
        // Write modified class
        ctClass.writeFile();
    }
}
```

---

## Real-World Examples

### Code Generation

**Lombok-style @Getter/@Setter:**

```java
@Retention(RetentionPolicy.SOURCE)
@Target(ElementType.TYPE)
public @interface Data {
}

// Usage
@Data
public class User {
    private String name;
    private int age;
}

// Processor generates:
// - getName(), setName()
// - getAge(), setAge()
// - equals(), hashCode(), toString()
```

### Validation Framework

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface NotNull {
    String message() default "Field cannot be null";
}

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Size {
    int min() default 0;
    int max() default Integer.MAX_VALUE;
    String message() default "Size must be between {min} and {max}";
}

// Validator
public class Validator {
    public static <T> List<String> validate(T object) {
        List<String> errors = new ArrayList<>();
        
        for (Field field : object.getClass().getDeclaredFields()) {
            field.setAccessible(true);
            
            try {
                Object value = field.get(object);
                
                if (field.isAnnotationPresent(NotNull.class)) {
                    if (value == null) {
                        NotNull annotation = field.getAnnotation(NotNull.class);
                        errors.add(field.getName() + ": " + annotation.message());
                    }
                }
                
                if (field.isAnnotationPresent(Size.class) && value != null) {
                    Size annotation = field.getAnnotation(Size.class);
                    int length = value.toString().length();
                    
                    if (length < annotation.min() || length > annotation.max()) {
                        String message = annotation.message()
                            .replace("{min}", String.valueOf(annotation.min()))
                            .replace("{max}", String.valueOf(annotation.max()));
                        errors.add(field.getName() + ": " + message);
                    }
                }
            } catch (IllegalAccessException e) {
                errors.add("Cannot access field: " + field.getName());
            }
        }
        
        return errors;
    }
}

// Usage
public class User {
    @NotNull(message = "Username is required")
    @Size(min = 3, max = 20, message = "Username must be 3-20 characters")
    private String username;
    
    @NotNull
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;
}

User user = new User();
List<String> errors = Validator.validate(user);
errors.forEach(System.out::println);
```

### Dependency Injection

See the [Runtime Processors](#runtime-processors-reflection) section above for a complete DI example.

---

## Best Practices

**1. Choose Appropriate Retention:**
```java
// ✓ Good - SOURCE for compile-time only
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}

// ✗ Bad - RUNTIME when not needed (overhead)
@Retention(RetentionPolicy.RUNTIME)
public @interface Override {
}
```

**2. Be Specific with @Target:**
```java
// ✓ Good - specific target
@Target(ElementType.METHOD)
public @interface Test {
}

// ✗ Bad - too broad
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD, 
         ElementType.PARAMETER, ElementType.CONSTRUCTOR})
public @interface Test {
}
```

**3. Provide Meaningful Defaults:**
```java
// ✓ Good - sensible defaults
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Cacheable {
    int timeout() default 3600;  // 1 hour
    String region() default "default";
}

// ✗ Bad - no defaults, verbose usage
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Cacheable {
    int timeout();  // Always required
    String region();  // Always required
}
```

**4. Document Your Annotations:**
```java
/**
 * Marks a method as transactional.
 * 
 * @since 1.0
 * @author John Doe
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Transactional {
    /**
     * Transaction timeout in seconds.
     * Default is -1 (no timeout).
     */
    int timeout() default -1;
}
```

**5. Validate Annotation Usage:**
```java
// In processor
if (element.getKind() != ElementKind.METHOD) {
    processingEnv.getMessager().printMessage(
        Diagnostic.Kind.ERROR,
        "@Test can only be applied to methods",
        element
    );
}
```

---

## Interview Questions

### Q1: What are annotation types, annotations, and annotation processors?

**Answer:**

**Annotation Types** are the definitions/templates:
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {  // This is an annotation TYPE
    long timeout() default 0L;
}
```

**Annotations** are the instances/usage:
```java
@Test(timeout = 1000)  // This is an annotation (instance)
public void myTest() {
}
```

**Annotation Processors** read and act on annotations:
```java
// Runtime processor
for (Method method : testClass.getDeclaredMethods()) {
    if (method.isAnnotationPresent(Test.class)) {
        Test test = method.getAnnotation(Test.class);
        runTest(method, test.timeout());
    }
}
```

---

### Q2: When are annotations processed?

**Answer:**

Annotations can be processed at **three different times**:

**1. Compile-Time (SOURCE retention):**
- By javac compiler
- By annotation processors (APT)
- For code generation and validation
- Example: @Override, Lombok

**2. Post-Compile (CLASS retention):**
- By bytecode processors
- For code injection and weaving
- Example: AspectJ, Hibernate enhancement

**3. Runtime (RUNTIME retention):**
- By frameworks using reflection
- For behavior modification
- Example: Spring, JUnit, JPA

---

### Q3: What are meta-annotations and how do they work?

**Answer:**

**Meta-annotations** are annotations that apply to other annotations. Java provides five built-in meta-annotations:

1. **@Retention** - When annotation is available
2. **@Target** - Where annotation can be applied
3. **@Documented** - Include in Javadoc
4. **@Inherited** - Inherited by subclasses
5. **@Repeatable** - Can be applied multiple times (Java 8+)

See the [Meta-Annotations Deep Dive](#meta-annotations-deep-dive) section for detailed examples.

---

### Q4: What are the different annotation type categories?

**Answer:**

**1. Marker Annotations** - No elements:
```java
@Override
public void method() {}
```

**2. Single-Element Annotations** - One element (usually `value`):
```java
@Component("userService")
public class UserService {}
```

**3. Multi-Element Annotations** - Multiple elements:
```java
@RequestMapping(value = "/users", method = GET, produces = "application/json")
public List<User> getUsers() {}
```

See the [Annotation Type Categories](#annotation-type-categories) section for detailed examples.

---

## 📚 Related Topics

- [Java Modifiers](./java-modifiers.md)
- [Annotations](./annotations.md)
- [Reflection API](../Module%2005%20-%20Java%20Objects/)
- [Design Patterns](../Module%2015%20-%20Design%20Patterns/)

---

## 💡 Key Takeaways

**Annotation Basics:**
- Annotation types are definitions (@interface)
- Annotations are instances/usage
- Processors read and act on annotations

**Processing Times:**
- SOURCE - Compile-time (javac, APT)
- CLASS - Post-compile (bytecode processors)
- RUNTIME - Runtime (reflection)

**Meta-Annotations:**
- @Retention - When available
- @Target - Where applicable
- @Documented - In Javadoc
- @Inherited - Inherited by subclasses
- @Repeatable - Multiple applications

**Annotation Categories:**
- Marker - No elements
- Single-element - One element (value)
- Multi-element - Multiple elements

**Best Practices:**
- Choose appropriate retention policy
- Be specific with target elements
- Provide meaningful defaults
- Document thoroughly
- Validate usage in processors

---

**[⬆ Back to Top](#)**