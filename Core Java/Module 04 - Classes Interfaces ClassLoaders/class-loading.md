# Class Loading Mechanism

> **Module**: Classes Interfaces ClassLoaders  
> **Topic**: Class Loading Mechanism - Deep Dive with Diagrams and Flow Charts

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Class Loading Basics](#class-loading-basics)
- [Class Loader Hierarchy](#class-loader-hierarchy)
- [Class Loading Process](#class-loading-process)
- [Static vs Dynamic Loading](#static-vs-dynamic-loading)
- [Class Loader Delegation Model](#class-loader-delegation-model)
- [Custom Class Loaders](#custom-class-loaders)
- [Class Loading Issues](#class-loading-issues)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

---

## Introduction

Class loading is a fundamental mechanism in Java that loads classes into the JVM at runtime. Understanding class loading is crucial for debugging ClassNotFoundException, NoClassDefFoundError, and other class loading issues.

### Class Loading Overview

```
┌─────────────────────────────────────────────────────────────┐
│              Java Class Loading Overview                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Source Code (.java)                                         │
│       │                                                       │
│       ▼ javac                                                │
│  Bytecode (.class)                                           │
│       │                                                       │
│       ▼ ClassLoader                                          │
│  Loaded Class (in JVM)                                       │
│       │                                                       │
│       ▼ JVM                                                  │
│  Running Application                                         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Class Loading Basics

### What is a Class Loader?

A class loader is responsible for loading Java classes into the JVM at runtime. It reads `.class` files and converts them into `java.lang.Class` objects.

### Class Loading Phases

```
┌─────────────────────────────────────────────────────────────┐
│              Class Loading Phases                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. LOADING                                                  │
│     └─► Find and read .class file                           │
│     └─► Create Class object in memory                       │
│                                                               │
│  2. LINKING                                                  │
│     ├─► Verification: Verify bytecode correctness           │
│     ├─► Preparation: Allocate memory for static variables   │
│     └─► Resolution: Resolve symbolic references             │
│                                                               │
│  3. INITIALIZATION                                           │
│     └─► Execute static initializers                         │
│     └─► Initialize static variables                         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Detailed Phase Diagram

```
┌─────────────────────────────────────────────────────────────┐
│         Class Loading Lifecycle (Detailed)                   │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐                                            │
│  │   LOADING    │                                            │
│  │ Find .class  │                                            │
│  │ Read bytes   │                                            │
│  │ Create Class │                                            │
│  └──────┬───────┘                                            │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                            │
│  │ VERIFICATION │                                            │
│  │ Check format │                                            │
│  │ Check rules  │                                            │
│  │ Check refs   │                                            │
│  └──────┬───────┘                                            │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                            │
│  │ PREPARATION  │                                            │
│  │ Allocate mem │                                            │
│  │ Set defaults │                                            │
│  └──────┬───────┘                                            │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                            │
│  │ RESOLUTION   │                                            │
│  │ Resolve refs │                                            │
│  │ (optional)   │                                            │
│  └──────┬───────┘                                            │
│         │                                                     │
│         ▼                                                     │
│  ┌──────────────┐                                            │
│  │INITIALIZATION│                                            │
│  │ Run <clinit> │                                            │
│  │ Init statics │                                            │
│  └──────────────┘                                            │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example: Class Loading Phases

```java
public class ClassLoadingDemo {
    // Phase 2: Preparation - allocated with default value (0)
    // Phase 3: Initialization - assigned actual value (100)
    private static int value = 100;
    
    // Phase 3: Initialization - static block executed
    static {
        System.out.println("Static block executed");
        value = 200;
    }
    
    public static void main(String[] args) {
        System.out.println("Value: " + value);
    }
}

/*
 * Output:
 * Static block executed
 * Value: 200
 * 
 * Execution order:
 * 1. Loading: ClassLoadingDemo.class loaded
 * 2. Linking: Bytecode verified, memory allocated
 * 3. Initialization: static variables initialized, static block runs
 * 4. main() method executes
 */
```

---

## Class Loader Hierarchy

### Class Loader Types

```
┌─────────────────────────────────────────────────────────────┐
│              Class Loader Hierarchy                          │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│                 ┌──────────────────────┐                     │
│                 │ Bootstrap ClassLoader│                     │
│                 │  (Native C/C++)      │                     │
│                 │  rt.jar, core libs   │                     │
│                 └──────────┬───────────┘                     │
│                            │ parent                          │
│                            │                                 │
│                 ┌──────────▼───────────┐                     │
│                 │Extension ClassLoader │                     │
│                 │  (Java)              │                     │
│                 │  jre/lib/ext/*.jar   │                     │
│                 └──────────┬───────────┘                     │
│                            │ parent                          │
│                            │                                 │
│                 ┌──────────▼───────────┐                     │
│                 │ System/App ClassLoader│                    │
│                 │  (Java)              │                     │
│                 │  CLASSPATH           │                     │
│                 └──────────┬───────────┘                     │
│                            │ parent                          │
│                            │                                 │
│                 ┌──────────▼───────────┐                     │
│                 │ Custom ClassLoader   │                     │
│                 │  (User-defined)      │                     │
│                 │  Application-specific│                     │
│                 └──────────────────────┘                     │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Class Loader Responsibilities

```
┌─────────────────────────────────────────────────────────────┐
│         Class Loader Responsibilities                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Bootstrap ClassLoader (null in Java)                        │
│  ┌────────────────────────────────────────────┐             │
│  │ • Loads core Java classes                  │             │
│  │ • rt.jar (java.lang.*, java.util.*, etc.)  │             │
│  │ • Native implementation (C/C++)            │             │
│  │ • Parent of all class loaders              │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  Extension ClassLoader (Java 8)                              │
│  Platform ClassLoader (Java 9+)                              │
│  ┌────────────────────────────────────────────┐             │
│  │ • Loads extension libraries                │             │
│  │ • jre/lib/ext directory                    │             │
│  │ • java.ext.dirs system property            │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  System/Application ClassLoader                              │
│  ┌────────────────────────────────────────────┐             │
│  │ • Loads application classes                │             │
│  │ • CLASSPATH environment variable           │             │
│  │ • -cp or -classpath option                 │             │
│  │ • java.class.path system property          │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  Custom ClassLoader                                          │
│  ┌────────────────────────────────────────────┐             │
│  │ • User-defined class loading               │             │
│  │ • Load from network, database, etc.        │             │
│  │ • Hot deployment, plugin systems           │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example: Identifying Class Loaders

```java
public class ClassLoaderDemo {
    public static void main(String[] args) {
        // Get class loader for current class
        ClassLoader appClassLoader = ClassLoaderDemo.class.getClassLoader();
        System.out.println("App ClassLoader: " + appClassLoader);
        
        // Get parent (Extension/Platform ClassLoader)
        ClassLoader extClassLoader = appClassLoader.getParent();
        System.out.println("Extension ClassLoader: " + extClassLoader);
        
        // Get parent (Bootstrap ClassLoader - returns null)
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println("Bootstrap ClassLoader: " + bootstrapClassLoader);
        
        // Core Java classes loaded by Bootstrap
        ClassLoader stringClassLoader = String.class.getClassLoader();
        System.out.println("String ClassLoader: " + stringClassLoader);
        
        // ArrayList loaded by Bootstrap
        ClassLoader arrayListClassLoader = java.util.ArrayList.class.getClassLoader();
        System.out.println("ArrayList ClassLoader: " + arrayListClassLoader);
    }
}

/*
 * Output (Java 8):
 * App ClassLoader: sun.misc.Launcher$AppClassLoader@18b4aac2
 * Extension ClassLoader: sun.misc.Launcher$ExtClassLoader@2a139a55
 * Bootstrap ClassLoader: null
 * String ClassLoader: null
 * ArrayList ClassLoader: null
 * 
 * Output (Java 9+):
 * App ClassLoader: jdk.internal.loader.ClassLoaders$AppClassLoader@...
 * Platform ClassLoader: jdk.internal.loader.ClassLoaders$PlatformClassLoader@...
 * Bootstrap ClassLoader: null
 * String ClassLoader: null
 * ArrayList ClassLoader: null
 */
```

---

## Class Loading Process

### How Classes Are Loaded

```
┌─────────────────────────────────────────────────────────────┐
│              Class Loading Process Flow                      │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Application requests class "com.example.MyClass"            │
│                    │                                         │
│                    ▼                                         │
│  ┌─────────────────────────────────────┐                    │
│  │ System ClassLoader receives request │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ Delegate to parent (Extension CL)   │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ Delegate to parent (Bootstrap CL)   │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ Bootstrap: Can't find class         │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ Extension: Can't find class         │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ System: Finds and loads class       │                    │
│  └─────────────┬───────────────────────┘                    │
│                │                                             │
│                ▼                                             │
│  ┌─────────────────────────────────────┐                    │
│  │ Return Class object to application  │                    │
│  └─────────────────────────────────────┘                    │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Class Loading Example

```java
public class LoadingSequenceDemo {
    static {
        System.out.println("LoadingSequenceDemo static block");
    }
    
    public static void main(String[] args) {
        System.out.println("Main method started");
        
        // Class A is loaded when first referenced
        System.out.println("About to reference ClassA");
        ClassA a = new ClassA();
        
        // Class B is loaded when first referenced
        System.out.println("About to reference ClassB");
        ClassB b = new ClassB();
    }
}

class ClassA {
    static {
        System.out.println("ClassA static block");
    }
    
    public ClassA() {
        System.out.println("ClassA constructor");
    }
}

class ClassB {
    static {
        System.out.println("ClassB static block");
    }
    
    public ClassB() {
        System.out.println("ClassB constructor");
    }
}

/*
 * Output:
 * LoadingSequenceDemo static block
 * Main method started
 * About to reference ClassA
 * ClassA static block
 * ClassA constructor
 * About to reference ClassB
 * ClassB static block
 * ClassB constructor
 */
```

---

## Static vs Dynamic Loading

### Static Loading

```java
/**
 * Static Loading: Classes loaded with 'new' operator
 * Happens at compile time
 */
public class StaticLoadingDemo {
    public static void main(String[] args) {
        // Static loading - class must exist at compile time
        Car car = new Car();
        car.drive();
    }
}

class Car {
    public void drive() {
        System.out.println("Car is driving");
    }
}

// If Car class doesn't exist: NoClassDefFoundError at runtime
```

### Dynamic Loading

```java
/**
 * Dynamic Loading: Classes loaded at runtime
 * Using Class.forName() or ClassLoader.loadClass()
 */
public class DynamicLoadingDemo {
    public static void main(String[] args) {
        try {
            // Dynamic loading - class name determined at runtime
            String className = "com.example.Car";
            
            // Method 1: Class.forName() - initializes the class
            Class<?> clazz1 = Class.forName(className);
            Object obj1 = clazz1.getDeclaredConstructor().newInstance();
            
            // Method 2: ClassLoader.loadClass() - doesn't initialize
            ClassLoader classLoader = DynamicLoadingDemo.class.getClassLoader();
            Class<?> clazz2 = classLoader.loadClass(className);
            Object obj2 = clazz2.getDeclaredConstructor().newInstance();
            
        } catch (ClassNotFoundException e) {
            System.err.println("Class not found: " + e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Comparison

```
┌─────────────────────────────────────────────────────────────┐
│         Static vs Dynamic Class Loading                      │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Static Loading                Dynamic Loading               │
│  ┌──────────────────┐         ┌──────────────────┐          │
│  │ • Uses 'new'     │         │ • Class.forName()│          │
│  │ • Compile-time   │         │ • Runtime        │          │
│  │ • NoClassDefFound│         │ • ClassNotFound  │          │
│  │   Error          │         │   Exception      │          │
│  │ • Simpler        │         │ • More flexible  │          │
│  └──────────────────┘         └──────────────────┘          │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Class.forName() vs ClassLoader.loadClass()

```java
public class LoadingMethodsDemo {
    public static void main(String[] args) throws Exception {
        
        // Class.forName() - initializes the class (runs static blocks)
        System.out.println("=== Using Class.forName() ===");
        Class<?> clazz1 = Class.forName("DatabaseDriver");
        
        System.out.println("\n=== Using ClassLoader.loadClass() ===");
        // ClassLoader.loadClass() - doesn't initialize (lazy)
        ClassLoader cl = LoadingMethodsDemo.class.getClassLoader();
        Class<?> clazz2 = cl.loadClass("DatabaseDriver");
        
        System.out.println("\n=== Creating instance ===");
        // Now it initializes
        Object obj = clazz2.getDeclaredConstructor().newInstance();
    }
}

class DatabaseDriver {
    static {
        System.out.println("DatabaseDriver static block executed");
    }
    
    public DatabaseDriver() {
        System.out.println("DatabaseDriver constructor");
    }
}

/*
 * Output:
 * === Using Class.forName() ===
 * DatabaseDriver static block executed
 * 
 * === Using ClassLoader.loadClass() ===
 * 
 * === Creating instance ===
 * DatabaseDriver constructor
 */
```

---

## Class Loader Delegation Model

### Parent Delegation Model

```
┌─────────────────────────────────────────────────────────────┐
│           Parent Delegation Model                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Request to load "com.example.MyClass"                       │
│                                                               │
│  Custom ClassLoader                                          │
│       │                                                       │
│       ├─► Check if already loaded? ──► Yes ──► Return        │
│       │                                                       │
│       └─► No ──► Delegate to parent                          │
│                       │                                       │
│  System ClassLoader   │                                      │
│       │               │                                       │
│       ├─► Check if already loaded? ──► Yes ──► Return        │
│       │                                                       │
│       └─► No ──► Delegate to parent                          │
│                       │                                       │
│  Extension ClassLoader│                                      │
│       │               │                                       │
│       ├─► Check if already loaded? ──► Yes ──► Return        │
│       │                                                       │
│       └─► No ──► Delegate to parent                          │
│                       │                                       │
│  Bootstrap ClassLoader│                                      │
│       │               │                                       │
│       ├─► Check if already loaded? ──► Yes ──► Return        │
│       │                                                       │
│       └─► No ──► Try to load                                 │
│                       │                                       │
│                       ├─► Found? ──► Yes ──► Load & Return   │
│                       │                                       │
│                       └─► No ──► Return to child             │
│                                                               │
│  Each child tries to load if parent can't                    │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Benefits of Delegation Model

```
┌─────────────────────────────────────────────────────────────┐
│      Benefits of Parent Delegation Model                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. UNIQUENESS                                               │
│     └─► Classes loaded once, prevents duplicates            │
│                                                               │
│  2. SECURITY                                                 │
│     └─► Core classes can't be replaced by malicious code    │
│                                                               │
│  3. VISIBILITY                                               │
│     └─► Child can see parent's classes, not vice versa      │
│                                                               │
│  4. NAMESPACE ISOLATION                                      │
│     └─► Each class loader creates separate namespace        │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example: Delegation in Action

```java
public class DelegationDemo {
    public static void main(String[] args) {
        // String is loaded by Bootstrap ClassLoader
        System.out.println("String ClassLoader: " + 
            String.class.getClassLoader());
        
        // Our class is loaded by System ClassLoader
        System.out.println("DelegationDemo ClassLoader: " + 
            DelegationDemo.class.getClassLoader());
        
        // Demonstrate delegation chain
        ClassLoader cl = DelegationDemo.class.getClassLoader();
        System.out.println("\nDelegation Chain:");
        while (cl != null) {
            System.out.println("  " + cl);
            cl = cl.getParent();
        }
        System.out.println("  Bootstrap ClassLoader (null)");
    }
}
```

---

## Custom Class Loaders

### Creating a Custom Class Loader

```java
import java.io.*;

/**
 * Custom ClassLoader that loads classes from a specific directory
 */
public class CustomClassLoader extends ClassLoader {
    private String classPath;
    
    public CustomClassLoader(String classPath) {
        this.classPath = classPath;
    }
    
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        try {
            // Convert class name to file path
            String fileName = classPath + File.separator + 
                             name.replace('.', File.separatorChar) + ".class";
            
            // Read class file
            byte[] classData = loadClassData(fileName);
            
            if (classData == null) {
                throw new ClassNotFoundException(name);
            }
            
            // Define the class
            return defineClass(name, classData, 0, classData.length);
            
        } catch (IOException e) {
            throw new ClassNotFoundException(name, e);
        }
    }
    
    private byte[] loadClassData(String fileName) throws IOException {
        File file = new File(fileName);
        if (!file.exists()) {
            return null;
        }
        
        try (FileInputStream fis = new FileInputStream(file);
             ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
            
            byte[] buffer = new byte[1024];
            int bytesRead;
            
            while ((bytesRead = fis.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesRead);
            }
            
            return baos.toByteArray();
        }
    }
}

// Usage
public class CustomClassLoaderDemo {
    public static void main(String[] args) {
        try {
            CustomClassLoader loader = new CustomClassLoader("/path/to/classes");
            
            Class<?> clazz = loader.loadClass("com.example.MyClass");
            Object obj = clazz.getDeclaredConstructor().newInstance();
            
            System.out.println("Class loaded by: " + clazz.getClassLoader());
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Network Class Loader Example

```java
import java.net.*;
import java.io.*;

/**
 * ClassLoader that loads classes from a network location
 */
public class NetworkClassLoader extends ClassLoader {
    private String baseUrl;
    
    public NetworkClassLoader(String baseUrl) {
        this.baseUrl = baseUrl;
    }
    
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        try {
            String url = baseUrl + "/" + name.replace('.', '/') + ".class";
            byte[] classData = loadClassFromNetwork(url);
            
            return defineClass(name, classData, 0, classData.length);
            
        } catch (IOException e) {
            throw new ClassNotFoundException(name, e);
        }
    }
    
    private byte[] loadClassFromNetwork(String urlString) throws IOException {
        URL url = new URL(urlString);
        
        try (InputStream is = url.openStream();
             ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
            
            byte[] buffer = new byte[4096];
            int bytesRead;
            
            while ((bytesRead = is.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesRead);
            }
            
            return baos.toByteArray();
        }
    }
}
```

---

## Class Loading Issues

### Common Exceptions

```
┌─────────────────────────────────────────────────────────────┐
│           Common Class Loading Exceptions                    │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ClassNotFoundException                                      │
│  ┌────────────────────────────────────────────┐             │
│  │ • Thrown by Class.forName()                │             │
│  │ • Checked exception                        │             │
│  │ • Class not found at runtime               │             │
│  │ • Wrong classpath or missing JAR           │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  NoClassDefFoundError                                        │
│  ┌────────────────────────────────────────────┐             │
│  │ • Error (not exception)                    │             │
│  │ • Class was present at compile time        │             │
│  │ • Missing at runtime                       │             │
│  │ • Often due to static initialization error │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  ClassCastException                                          │
│  ┌────────────────────────────────────────────┐             │
│  │ • Invalid type casting                     │             │
│  │ • Class loaded by different class loaders  │             │
│  │ • Same class name, different instances     │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
│  LinkageError                                                │
│  ┌────────────────────────────────────────────┐             │
│  │ • Class depends on another class           │             │
│  │ • Dependent class has incompatible change  │             │
│  │ • Version mismatch                         │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### ClassNotFoundException vs NoClassDefFoundError

```java
public class ExceptionDemo {
    
    // ClassNotFoundException example
    public static void demonstrateClassNotFoundException() {
        try {
            // Trying to load a class that doesn't exist
            Class.forName("com.example.NonExistentClass");
        } catch (ClassNotFoundException e) {
            System.out.println("ClassNotFoundException: " + e.getMessage());
        }
    }
    
    // NoClassDefFoundError example
    public static void demonstrateNoClassDefFoundError() {
        try {
            // If DependentClass exists at compile time but not at runtime
            DependentClass obj = new DependentClass();
        } catch (NoClassDefFoundError e) {
            System.out.println("NoClassDefFoundError: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        demonstrateClassNotFoundException();
        demonstrateNoClassDefFoundError();
    }
}

// This class causes NoClassDefFoundError if removed at runtime
class DependentClass {
    static {
        // If this throws exception, NoClassDefFoundError occurs
        if (true) {
            throw new RuntimeException("Initialization failed");
        }
    }
}
```

---

## Troubleshooting

### Debugging Class Loading Issues

```java
/**
 * Enable verbose class loading to see what's being loaded
 * Run with: java -verbose:class YourClass
 */
public class VerboseClassLoadingDemo {
    public static void main(String[] args) {
        System.out.println("Application started");
        
        // This will show class loading in verbose mode
        String str = new String("Hello");
        java.util.ArrayList<String> list = new java.util.ArrayList<>();
    }
}

/*
 * Output with -verbose:class:
 * [Opened /Library/Java/JavaVirtualMachines/.../lib/rt.jar]
 * [Loaded java.lang.Object from /Library/Java/.../lib/rt.jar]
 * [Loaded java.io.Serializable from /Library/Java/.../lib/rt.jar]
 * [Loaded java.lang.String from /Library/Java/.../lib/rt.jar]
 * ...
 */
```

### Classpath Inspection

```java
public class ClasspathInspector {
    public static void main(String[] args) {
        // Print classpath
        System.out.println("=== CLASSPATH ===");
        String classpath = System.getProperty("java.class.path");
        String[] paths = classpath.split(File.pathSeparator);
        for (String path : paths) {
            System.out.println("  " + path);
        }
        
        // Print library path
        System.out.println("\n=== LIBRARY PATH ===");
        String libraryPath = System.getProperty("java.library.path");
        String[] libPaths = libraryPath.split(File.pathSeparator);
        for (String path : libPaths) {
            System.out.println("  " + path);
        }
        
        // Print boot classpath (Java 8)
        System.out.println("\n=== BOOT CLASSPATH ===");
        String bootClasspath = System.getProperty("sun.boot.class.path");
        if (bootClasspath != null) {
            String[] bootPaths = bootClasspath.split(File.pathSeparator);
            for (String path : bootPaths) {
                System.out.println("  " + path);
            }
        }
    }
}
```

### Finding Class Location

```java
import java.net.URL;
import java.security.CodeSource;

public class ClassLocationFinder {
    
    public static void findClassLocation(Class<?> clazz) {
        System.out.println("Class: " + clazz.getName());
        
        // Method 1: Using CodeSource
        CodeSource codeSource = clazz.getProtectionDomain().getCodeSource();
        if (codeSource != null) {
            URL location = codeSource.getLocation();
            System.out.println("  Location (CodeSource): " + location);
        }
        
        // Method 2: Using ClassLoader resource
        String className = clazz.getName().replace('.', '/') + ".class";
        URL resource = clazz.getClassLoader().getResource(className);
        System.out.println("  Location (Resource): " + resource);
        
        // Class loader info
        System.out.println("  ClassLoader: " + clazz.getClassLoader());
        System.out.println();
    }
    
    public static void main(String[] args) {
        // Find location of various classes
        findClassLocation(String.class);
        findClassLocation(java.util.ArrayList.class);
        findClassLocation(ClassLocationFinder.class);
    }
}
```

### Loading Resources from Classpath

```java
import java.io.*;
import java.util.Properties;

public class ResourceLoadingDemo {
    
    public static void loadPropertiesFromClasspath() {
        Properties props = new Properties();
        
        // Method 1: Using class loader
        try (InputStream is = ResourceLoadingDemo.class
                .getClassLoader()
                .getResourceAsStream("config.properties")) {
            
            if (is != null) {
                props.load(is);
                System.out.println("Properties loaded: " + props);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // Method 2: Using class (relative to class package)
        try (InputStream is = ResourceLoadingDemo.class
                .getResourceAsStream("/config.properties")) {
            
            if (is != null) {
                props.load(is);
                System.out.println("Properties loaded: " + props);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    
    public static void main(String[] args) {
        loadPropertiesFromClasspath();
    }
}
```

---

## Best Practices

### 1. Use Appropriate Class Loading Method

```java
// ✓ Good: Use Class.forName() for JDBC drivers (needs initialization)
Class.forName("com.mysql.jdbc.Driver");

// ✓ Good: Use ClassLoader.loadClass() for lazy loading
ClassLoader cl = getClass().getClassLoader();
Class<?> clazz = cl.loadClass("com.example.MyClass");
```

### 2. Handle Exceptions Properly

```java
public class ProperExceptionHandling {
    public static Class<?> loadClass(String className) {
        try {
            return Class.forName(className);
        } catch (ClassNotFoundException e) {
            // Log the error with context
            System.err.println("Failed to load class: " + className);
            System.err.println("Classpath: " + 
                System.getProperty("java.class.path"));
            throw new RuntimeException("Class loading failed", e);
        }
    }
}
```

### 3. Avoid Class Loader Leaks

```java
public class ClassLoaderLeakPrevention {
    
    // ❌ Bad: Holding reference to class loader
    private static ClassLoader leakyClassLoader;
    
    public static void badExample() {
        leakyClassLoader = new CustomClassLoader("/path");
        // Class loader can't be garbage collected
    }
    
    // ✓ Good: Use try-with-resources or proper cleanup
    public static void goodExample() {
        ClassLoader tempClassLoader = new CustomClassLoader("/path");
        try {
            // Use class loader
            Class<?> clazz = tempClassLoader.loadClass("MyClass");
            // ...
        } finally {
            // Allow garbage collection
            tempClassLoader = null;
        }
    }
}
```

### 4. Document Custom Class Loaders

```java
/**
 * Custom class loader for loading plugin classes.
 * 
 * <p>This class loader loads classes from the plugins directory
 * and provides isolation between different plugins.
 * 
 * <p>Usage:
 * <pre>
 * PluginClassLoader loader = new PluginClassLoader("/plugins");
 * Class<?> pluginClass = loader.loadClass("com.example.Plugin");
 * </pre>
 * 
 * @see ClassLoader
 */
public class PluginClassLoader extends ClassLoader {
    // Implementation
}
```

### 5. Use Context Class Loader When Needed

```java
public class ContextClassLoaderDemo {
    
    public static void useContextClassLoader() {
        // Get current thread's context class loader
        ClassLoader contextCL = Thread.currentThread().getContextClassLoader();
        
        try {
            // Load class using context class loader
            Class<?> clazz = contextCL.loadClass("com.example.MyClass");
            
            // Useful in frameworks and libraries
            // that need to load application classes
            
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    
    public static void setContextClassLoader() {
        ClassLoader customCL = new CustomClassLoader("/path");
        
        // Save current context class loader
        ClassLoader originalCL = Thread.currentThread().getContextClassLoader();
        
        try {
            // Set custom class loader as context
            Thread.currentThread().setContextClassLoader(customCL);
            
            // Do work with custom class loader
            
        } finally {
            // Restore original class loader
            Thread.currentThread().setContextClassLoader(originalCL);
        }
    }
}
```

---

## Summary

### Key Takeaways

- ✓ Class loading happens in three phases: Loading, Linking, Initialization
- ✓ Java has hierarchical class loaders: Bootstrap, Extension/Platform, System, Custom
- ✓ Parent delegation model ensures uniqueness and security
- ✓ Static loading uses `new`, dynamic loading uses `Class.forName()`
- ✓ ClassNotFoundException is checked, NoClassDefFoundError is an error
- ✓ Custom class loaders enable advanced scenarios like hot deployment
- ✓ Use `-verbose:class` to debug class loading issues
- ✓ Proper classpath configuration is crucial

### Class Loading Checklist

```
┌─────────────────────────────────────────────────────────────┐
│              Class Loading Checklist                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ☐ Understand class loader hierarchy                        │
│  ☐ Know parent delegation model                             │
│  ☐ Differentiate static vs dynamic loading                  │
│  ☐ Handle ClassNotFoundException properly                   │
│  ☐ Understand NoClassDefFoundError causes                   │
│  ☐ Know how to debug class loading issues                   │
│  ☐ Use appropriate class loading method                     │
│  ☐ Avoid class loader memory leaks                          │
│  ☐ Document custom class loaders                            │
│  ☐ Test with different classpaths                           │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Related Topics

- [Classes & Interfaces](classes-interfaces.md)
- [Abstract Classes vs Interfaces](abstract-vs-interface.md)
- [JVM Memory Model](../Module%2011%20-%20JVM/jvm-memory-model.md)
- [Performance Considerations](../Module%2014%20-%20Performance%20and%20Memory/)

---

**[⬆ Back to Top](#)**

**Last Updated**: 2026-06-03