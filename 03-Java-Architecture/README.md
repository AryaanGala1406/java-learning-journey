# 03 - Java Architecture

> Understanding JDK, JRE, JVM and how Java programs execute

---

## 📚 Table of Contents

1. [Java Architecture Overview](#java-architecture-overview)
2. [JDK - Java Development Kit](#jdk---java-development-kit)
3. [JRE - Java Runtime Environment](#jre---java-runtime-environment)
4. [JVM - Java Virtual Machine](#jvm---java-virtual-machine)
5. [Compilation and Execution Flow](#compilation-and-execution-flow)
6. [Who Needs What?](#who-needs-what)

---

## 🏗️ Java Architecture Overview

### The Complete Picture

```
┌─────────────────────────────────────────────────┐
│                      JDK                        │
│         (Java Development Kit)                  │
│  ┌───────────────────────────────────────────┐  │
│  │              JRE                          │  │
│  │      (Java Runtime Environment)           │  │
│  │  ┌─────────────────────────────────────┐  │  │
│  │  │           JVM                       │  │  │
│  │  │   (Java Virtual Machine)            │  │  │
│  │  │  ┌────────────────┐                 │  │  │
│  │  │  │ Class Loader   │                 │  │  │
│  │  │  ├────────────────┤                 │  │  │
│  │  │  │ Bytecode       │                 │  │  │
│  │  │  │ Verifier       │                 │  │  │
│  │  │  ├────────────────┤                 │  │  │
│  │  │  │ Interpreter    │                 │  │  │
│  │  │  └────────────────┘                 │  │  │
│  │  └─────────────────────────────────────┘  │  │
│  │                                            │  │
│  │  + Java APIs & Libraries                  │  │
│  │  + Resource Allocation                    │  │
│  │  + Multithreading Support                 │  │
│  │  + Interaction with OS                    │  │
│  └───────────────────────────────────────────┘  │
│                                                  │
│  + Compiler (javac)                              │
│  + Debugger (jdb)                                │
│  + Other Development Tools                       │
└─────────────────────────────────────────────────┘
```

### Layered Architecture

```
┌──────────────────────────────┐
│    Development Tools         │  ← JDK Layer
│    (javac, jdb, etc.)        │
├──────────────────────────────┤
│    Java Libraries & APIs     │  ← JRE Layer
│    (java.lang, java.util)    │
├──────────────────────────────┤
│    JVM Components            │  ← JVM Layer
│    (ClassLoader, Verifier)   │
├──────────────────────────────┤
│    Operating System          │  ← OS Layer
│    (Windows, Linux, Mac)     │
└──────────────────────────────┘
```

---

## 🛠️ JDK - Java Development Kit

### What is JDK?

**JDK = JRE + Development Tools**

The complete software development kit for Java developers.

### Components of JDK

```
JDK
├── JRE (Java Runtime Environment)
│   └── (Everything needed to run Java)
├── javac (Compiler)
├── java (Launcher)
├── javadoc (Documentation Generator)
├── jdb (Debugger)
├── jar (Archive Tool)
└── Other development utilities
```

### Key Features

#### 1. **Compiler (javac)**

```bash
# Converts source code to bytecode
javac MyProgram.java
# Output: MyProgram.class
```

#### 2. **Development Tools**

| Tool | Purpose | Example |
|------|---------|---------|
| `javac` | Compile Java code | `javac Hello.java` |
| `java` | Run Java programs | `java Hello` |
| `javadoc` | Generate documentation | `javadoc MyClass.java` |
| `jdb` | Debug programs | `jdb Hello` |
| `jar` | Create JAR files | `jar cvf app.jar *.class` |

#### 3. **Standard Libraries**

```java
import java.util.*;     // Collections
import java.io.*;       // Input/Output
import java.net.*;      // Networking
import java.sql.*;      // Database
```

### Who Needs JDK?

✅ **Java Developers** - To write and compile code  
✅ **Development Teams** - For building applications  
✅ **Students** - Learning Java programming

❌ **End Users** - Don't need JDK to run Java apps

### Capabilities

```
With JDK, you can:
✅ Write Java code
✅ Compile Java code
✅ Debug programs
✅ Create documentation
✅ Package applications
❌ Cannot execute without JRE (included in JDK)
```

---

## ▶️ JRE - Java Runtime Environment

### What is JRE?

**JRE = JVM + Libraries + Supporting Files**

The runtime environment needed to execute Java applications.

### Components of JRE

```
JRE
├── JVM (Java Virtual Machine)
│   ├── Class Loader
│   ├── Bytecode Verifier
│   └── Interpreter
├── Java Class Libraries
│   ├── java.lang.*
│   ├── java.util.*
│   ├── java.io.*
│   └── Other core APIs
└── Configuration Files
```

### Key Responsibilities

#### 1. **Resource Allocation**

```
JRE manages:
• Memory allocation
• CPU scheduling
• Thread management
• I/O operations
```

#### 2. **OS Interaction**

```
┌─────────────┐
│ Java App    │
└──────┬──────┘
       │
┌──────▼──────┐
│     JRE     │  ← Translates Java calls to OS calls
└──────┬──────┘
       │
┌──────▼──────┐
│  Operating  │
│   System    │
└─────────────┘
```

#### 3. **Multithreading Support**

```java
// JRE handles thread management
Thread t1 = new Thread(() -> {
    System.out.println("Thread 1");
});

Thread t2 = new Thread(() -> {
    System.out.println("Thread 2");
});

t1.start();  // JRE manages execution
t2.start();  // JRE manages execution
```

### Who Needs JRE?

✅ **Everyone** - To run Java applications  
✅ **End Users** - To use Java software  
✅ **Deployment Teams** - For production servers  
✅ **Developers** - Included in JDK

### Purpose

```
JRE provides:
✅ Runtime environment
✅ Standard libraries
✅ JVM for execution
✅ OS interaction layer
❌ Does NOT include compiler (javac)
```

---

## ⚙️ JVM - Java Virtual Machine

### What is JVM?

**JVM = Virtual computer that executes Java bytecode**

> ⚠️ **Important:** JVM does NOT directly run bytecode!  
> It has components that load, verify, and then execute it.

### JVM Components

```
┌────────────────────────────────────┐
│         Java Virtual Machine       │
├────────────────────────────────────┤
│                                    │
│  ┌────────────────────────────┐    │
│  │     Class Loader           │    │
│  │  • Loads .class files      │    │
│  │  • Loading                 │    │
│  │  • Linking                 │    │
│  │  • Initialization          │    │
│  └────────────────────────────┘    │
│                                    │
│  ┌────────────────────────────┐    │
│  │   Bytecode Verifier        │    │
│  │  • Verifies .class files   │    │
│  │  • Security checks         │    │
│  │  • Format validation       │    │
│  └────────────────────────────┘    │
│                                    │
│  ┌────────────────────────────┐    │
│  │      Interpreter           │    │
│  │  • Executes bytecode       │    │
│  │  • Line-by-line execution  │    │
│  │  • Generates machine code  │    │
│  └────────────────────────────┘    │
│                                    │
│  ┌────────────────────────────┐    │
│  │    JIT Compiler            │    │
│  │  • Optimizes hot code      │    │
│  │  • Compiles to native      │    │
│  └────────────────────────────┘    │
│                                    │
│  ┌────────────────────────────┐    │
│  │   Garbage Collector        │    │
│  │  • Automatic memory mgmt   │    │
│  │  • Frees unused objects    │    │
│  └────────────────────────────┘    │
└────────────────────────────────────┘
```

### Component Details

#### 1. **Class Loader**

**Purpose:** Loads classes into memory

```
Class Loading Process:
1. Loading
   ↓
   Reads .class file
   ↓
2. Linking
   ↓
   Verification → Bytecode verification
   Preparation → Memory allocation
   Resolution → Symbolic references
   ↓
3. Initialization
   ↓
   Execute static initializers
```

**Example:**
```java
public class Demo {
    static {
        System.out.println("Class loaded!");
        // Class Loader executes this
    }
}
```

#### 2. **Bytecode Verifier**

**Purpose:** Ensures bytecode is valid and safe

```
Verification Checks:
✅ Format check - Valid .class format
✅ Security check - No illegal operations
✅ Type check - Type safety
✅ Access check - Proper access modifiers
```

**What it prevents:**
```java
// Bytecode Verifier catches:
❌ Stack overflow
❌ Invalid type casting
❌ Illegal memory access
❌ Unauthorized operations
```

#### 3. **Interpreter**

**Purpose:** Executes bytecode line by line

```
Bytecode (.class)
    ↓
Interpreter reads instruction
    ↓
Converts to machine code
    ↓
CPU executes
    ↓
Repeat for next instruction
```

**Characteristics:**
- ✅ Platform-independent execution
- ✅ Immediate execution
- ❌ Slower than native code

#### 4. **JIT Compiler (Just-In-Time)**

**Purpose:** Optimizes frequently used code

```
Interpreter finds "hot" code (frequently executed)
    ↓
JIT Compiler compiles to native machine code
    ↓
Native code stored in cache
    ↓
Subsequent calls use cached native code
    ↓
Much faster execution!
```

**Example:**
```java
// Loop executed many times
for (int i = 0; i < 1000000; i++) {
    result += compute(i);
    // JIT will compile compute() to native code
}
```

### Memory Areas in JVM

```
┌─────────────────────────────────────┐
│         Method Area                 │
│  • Class metadata                   │
│  • Static variables                 │
│  • Constant pool                    │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│           Heap                      │
│  • Objects                          │
│  • Instance variables               │
│  • Arrays                           │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│         Stack (per thread)          │
│  • Method calls                     │
│  • Local variables                  │
│  • References                       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│       PC Register                   │
│  • Current instruction address      │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│     Native Method Stack             │
│  • Native method calls              │
└─────────────────────────────────────┘
```

---

## 🔄 Compilation and Execution Flow

### Complete Flow

```
┌──────────────────┐
│  MyApp.java      │  Source Code
│  (Human Readable)│
└────────┬─────────┘
         │
         │ javac (JDK Compiler)
         ↓
┌──────────────────┐
│  MyApp.class     │  Bytecode
│  (Platform       │  (Not human readable)
│   Independent)   │  (Not machine code)
└────────┬─────────┘
         │
         │ Class Loader (JVM)
         ↓
┌──────────────────┐
│  Loaded in       │
│  Memory          │
└────────┬─────────┘
         │
         │ Bytecode Verifier (JVM)
         ↓
┌──────────────────┐
│  Verified        │
│  Bytecode        │
└────────┬─────────┘
         │
         │ Interpreter (JVM)
         ↓
┌──────────────────┐
│  Machine Code    │  Platform Specific
│  (Windows/Linux/ │
│   Mac specific)  │
└────────┬─────────┘
         │
         ↓
     Execution
```

### Step-by-Step Process

#### Step 1: Write Code

```java
// MyApp.java
public class MyApp {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

#### Step 2: Compile

```bash
javac MyApp.java
```

**What happens:**
- JDK compiler (`javac`) reads source code
- Performs syntax checking
- Generates bytecode (`MyApp.class`)

#### Step 3: Class Loading

```
Class Loader finds MyApp.class
    ↓
Loads into Method Area
    ↓
Prepares memory for static variables
    ↓
Resolves references
```

#### Step 4: Verification

```
Bytecode Verifier checks:
✅ Valid bytecode format
✅ No security violations
✅ Type safety
✅ Access control
```

#### Step 5: Execution

```
Interpreter reads bytecode
    ↓
Converts each instruction to machine code
    ↓
CPU executes
    ↓
(JIT optimizes frequently used code)
```

### Platform Independence

```
Same MyApp.class runs on:

Windows:                    Linux:                      macOS:
┌──────────┐               ┌───────────┐                ┌───────────┐
│MyApp.class│              │MyApp.class│                │MyApp.class│
└─────┬────┘               └─────┬─────┘                └──────┬────┘
      │                          │                             │
┌─────▼────┐               ┌─────▼─────┐                ┌──────▼────┐
│Windows JVM│              │Linux JVM  │                │ macOS JVM │
└─────┬────┘               └──────┬────┘                └──────┬────┘
      │                           │                            │
┌─────▼────┐               ┌──────▼────┐                ┌──────▼────┐
│Windows   │               │  Linux    │                │  macOS    │
│Machine   │               │  Machine  │                │  Machine  │
│Code      │               │  Code     │                │  Code     │
└──────────┘               └───────────┘                └───────────┘
```

---

## 👥 Who Needs What?

### Comparison Table

| User Type | Needs JDK? | Needs JRE? | Purpose |
|-----------|-----------|-----------|---------|
| **Java Developer** | ✅ Yes | ✅ Yes (included) | Write, compile, and test code |
| **End User** | ❌ No | ✅ Yes | Run Java applications |
| **Deployment Team** | ❌ No | ✅ Yes | Deploy apps on servers |
| **Student/Learner** | ✅ Yes | ✅ Yes (included) | Learn and practice Java |
| **System Admin** | Maybe | ✅ Yes | Maintain Java apps |

### Detailed Scenarios

#### Scenario 1: Developer's Machine

```
Developer needs:
✅ JDK (includes JRE)
   ├── Write code
   ├── Compile code
   ├── Debug code
   └── Run code
```

**Installation:**
```bash
# Download JDK from Oracle or OpenJDK
# JDK includes everything developer needs
```

#### Scenario 2: End User's Machine

```
End user needs:
✅ JRE only
   └── Run Java applications
       (Games, apps, etc.)

❌ Does NOT need JDK
   (No development required)
```

**Installation:**
```bash
# Download JRE
# Smaller download
# Just enough to run apps
```

#### Scenario 3: Production Server

```
Production server needs:
✅ JRE only
   └── Run deployed applications
       (Web servers, APIs, etc.)

❌ Does NOT need JDK
   (No compilation on production)
```

### Memory Analogy

```
Think of it like cooking:

JDK = Complete Kitchen
    ├── Stove (Compiler)
    ├── Oven (Tools)
    ├── Ingredients (Libraries)
    └── Dining area (Runtime)

JRE = Dining Area Only
    └── Just to enjoy the meal
        (Run the application)
```

---

## 🎯 Key Takeaways

### Essential Points

1. **JDK = JRE + Development Tools**
   - For developers
   - Includes compiler (`javac`)

2. **JRE = JVM + Libraries**
   - For everyone
   - Runs Java applications

3. **JVM has three main components:**
   - Class Loader → Loads classes
   - Bytecode Verifier → Ensures security
   - Interpreter → Executes bytecode

4. **JVM does NOT directly run bytecode**
   - Components process it first
   - Then interpreter executes

5. **Compilation Flow:**
   ```
   .java → (javac) → .class → (JVM) → Machine Code
   ```

### Quick Reference

```
To CREATE Java apps → Need JDK ✅
To RUN Java apps → Need JRE ✅
JDK includes JRE → Don't need separate JRE ✅
JRE does NOT include compiler → Can't compile code ❌
```

---

## 🔗 Navigation

- [← Previous: OOP Fundamentals](../02-OOP-Fundamentals/README.md)
- [Next: Compilation & Execution →](../04-Compilation-and-Execution/README.md)

---

## 📝 Summary

In this module, you learned:

- ✅ JDK, JRE, and JVM architecture
- ✅ Components of each layer
- ✅ How Java code compiles and executes
- ✅ Who needs what for different roles
- ✅ Platform independence mechanism

**Next Step:** Learn about file naming rules, PATH, and CLASSPATH!

---

*Happy Learning! ⚙️*