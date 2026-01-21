# 04 - Compilation and Execution

> Understanding file naming rules, compilation process, PATH, CLASSPATH, and execution flow

---

## 📚 Table of Contents

1. [File Naming and Compilation Rules](#file-naming-and-compilation-rules)
2. [Multiple Classes in One File](#multiple-classes-in-one-file)
3. [Multiple main() Methods](#multiple-main-methods)
4. [.class File Generation](#class-file-generation)
5. [PATH and CLASSPATH](#path-and-classpath)
6. [JAR Files](#jar-files)
7. [Error Handling and Re-execution](#error-handling-and-re-execution)

---

## 📁 File Naming and Compilation Rules

### The Golden Rule

> **If a class is declared `public`, the file name MUST match the class name.**

```
Public Class Name = File Name (case-sensitive)
```

### ✅ Correct Examples

#### Example 1: Public Class

```java
// File name: Abc.java ✅
public class Abc {
    public static void main(String[] args) {
        System.out.println("Hello from Abc!");
    }
}
```

**Compilation:**
```bash
javac Abc.java  # ✅ Success
```

#### Example 2: Non-Public Class

```java
// File name: Anything.java ✅
// File name can be different when class is not public
class Xyz {
    public static void main(String[] args) {
        System.out.println("Hello from Xyz!");
    }
}
```

**Compilation:**
```bash
javac Anything.java  # ✅ Success
# Generates: Xyz.class
```

**Execution:**
```bash
java Xyz  # ✅ Run using class name, not file name!
```

### ❌ Incorrect Examples

#### Error Case 1: Mismatch

```java
// File name: Abc.java ❌
public class Xyz {
    public static void main(String[] args) {
        System.out.println("Error!");
    }
}
```

**Compilation:**
```bash
javac Abc.java
# ❌ Error: class Xyz is public, should be declared in a file named Xyz.java
```

#### Error Case 2: Case Sensitivity

```java
// File name: abc.java ❌
public class Abc {
    // Code here
}
```

**Compilation:**
```bash
javac abc.java
# ❌ Error on case-sensitive systems (Linux/Mac)
# Java is case-sensitive!
```

### Decision Tree

```
Does the class have 'public' keyword?
    │
    ├─ Yes → File name MUST match class name
    │         Example: public class Abc → Abc.java
    │
    └─ No  → File name can be anything
              Example: class Xyz → AnyName.java
```

### Important Points

1. **Java uses file name as entry point**
   - To locate the public class
   - File name helps compiler find the right class

2. **Only ONE public class per file**
   - Multiple public classes → Compilation error
   - Can have multiple non-public classes

3. **main() method class doesn't need to be public**
   - No restriction on which class has main()
   - File name doesn't need to match main() class

---

## 📦 Multiple Classes in One File

### Allowed Configuration

```java
// File name: Abc.java
public class Abc {
    void methodA() {
        System.out.println("From Abc");
    }
}

class Xyz {
    public static void main(String[] args) {
        System.out.println("From Xyz");
    }
}

class Pqr {
    void methodP() {
        System.out.println("From Pqr");
    }
}
```

### Compilation Result

```bash
javac Abc.java
```

**Generated Files:**
```
Abc.class  # For public class Abc
Xyz.class  # For class Xyz
Pqr.class  # For class Pqr
```

**Execution:**
```bash
java Xyz  # Runs Xyz's main method
# Output: From Xyz
```

### Rules for Multiple Classes

| Rule | Allowed? | Example |
|------|----------|---------|
| **Multiple public classes** | ❌ No | `public class A {}` `public class B {}` |
| **One public, many non-public** | ✅ Yes | `public class A {}` `class B {}` `class C {}` |
| **All non-public classes** | ✅ Yes | `class A {}` `class B {}` `class C {}` |
| **Multiple main() methods** | ✅ Yes | Each class can have its own main() |

### Access Modifiers Default

```java
// By default, classes are package-private (not public)
class MyClass {  // package-private access
    // Only accessible within same package
}
```

**Default Access:**
- No `public` keyword = package-private
- Accessible only within the same package
- Cannot be accessed from other packages

---

## 🔀 Multiple main() Methods

### Yes, It's Allowed!

Java allows **multiple main() methods** in different classes.

### Example 1: Sequential Execution

```java
// File: Demo.java
class X {
    public static void main(String[] args) {
        System.out.println("Hello 1");
        Y.main(args);  // Call Y's main
    }
}

class Y {
    public static void main(String[] args) {
        System.out.println("Hello 2");
        Z.main(args);  // Call Z's main
    }
}

class Z {
    public static void main(String[] args) {
        System.out.println("Hello 3");
    }
}
```

**Compilation:**
```bash
javac Demo.java
# Generates: X.class, Y.class, Z.class
```

**Execution:**
```bash
java X
```

**Output:**
```
Hello 1
Hello 2
Hello 3
```

### Example 2: Different Entry Points

```java
// File: MultiMain.java
class App1 {
    public static void main(String[] args) {
        System.out.println("Running App1");
    }
}

class App2 {
    public static void main(String[] args) {
        System.out.println("Running App2");
    }
}

class App3 {
    public static void main(String[] args) {
        System.out.println("Running App3");
    }
}
```

**Execute Different Apps:**
```bash
java App1  # Output: Running App1
java App2  # Output: Running App2
java App3  # Output: Running App3
```

### Important Concepts

#### 1. **main() as Entry Point**

```
JVM always starts from main()
    ↓
main() is special only as the entry point
    ↓
After execution starts, main() is just a normal method
    ↓
Can be called like any other static method
```

#### 2. **Calling main() After Execution Starts**

```java
class Demo {
    public static void main(String[] args) {
        System.out.println("First call");
        
        // Call main again!
        if (args.length == 0) {
            main(new String[]{"recursion"});
        }
        
        System.out.println("Execution continues");
    }
}
```

**Output:**
```
First call
First call
Execution continues
Execution continues
```

#### 3. **main() Signature Requirements**

```java
// ✅ Correct signatures
public static void main(String[] args)
public static void main(String args[])
public static void main(String... args)

// ❌ Incorrect - won't be recognized as entry point
static void main(String[] args)        // Not public
public void main(String[] args)        // Not static
public static int main(String[] args)  // Not void
public static void main()              // No String array
```

---

## 📄 .class File Generation

### What Generates .class Files?

Every API or keyword that defines a type generates a `.class` file:

```
✅ Classes              → .class file
✅ Interfaces           → .class file
✅ Enums                → .class file
✅ Abstract classes     → .class file
✅ Even empty classes   → .class file
```

### Examples

#### Example 1: Multiple Types

```java
// File: AllTypes.java
public class AllTypes {
    public static void main(String[] args) {
        System.out.println("Demo");
    }
}

interface MyInterface {
    void display();
}

abstract class MyAbstract {
    abstract void show();
}

enum Color {
    RED, GREEN, BLUE
}

class EmptyClass {
    // Completely empty!
}
```

**Compilation:**
```bash
javac AllTypes.java
```

**Generated Files:**
```
AllTypes.class
MyInterface.class
MyAbstract.class
Color.class
EmptyClass.class
```

#### Example 2: Nested Classes

```java
public class Outer {
    class Inner {
        // Inner class
    }
    
    static class StaticNested {
        // Static nested class
    }
}
```

**Generated Files:**
```
Outer.class
Outer$Inner.class           # Inner class
Outer$StaticNested.class    # Static nested class
```

### Managing .class Files

#### Delete All .class Files

**Windows:**
```bash
del *.class
```

**Linux/Mac:**
```bash
rm *.class
```

#### List All .class Files

**Windows:**
```bash
dir *.class
```

**Linux/Mac:**
```bash
ls *.class
```

---

## 🛤️ PATH and CLASSPATH

### Understanding PATH

**Purpose:** Tell OS where to find **executable files** (.exe)

```
PATH → Location of javac.exe and java.exe
```

### Setting PATH

#### Windows

```bash
# Method 1: Temporary (current session only)
set path=C:\Program Files\Java\jdk-17\bin;.;%path%

# Method 2: Permanent (Environment Variables)
# Control Panel → System → Advanced → Environment Variables
# Add to PATH: C:\Program Files\Java\jdk-17\bin
```

#### Linux/Mac

```bash
# Method 1: Temporary (current session)
export PATH=/usr/lib/jvm/java-17/bin:$PATH

# Method 2: Permanent (add to ~/.bashrc or ~/.zshrc)
echo 'export PATH=/usr/lib/jvm/java-17/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Why Set PATH?

**Without PATH:**
```bash
javac Hello.java
# Error: 'javac' is not recognized as an internal or external command
```

**With PATH:**
```bash
javac Hello.java
# ✅ Works! OS can find javac.exe
```

### Understanding CLASSPATH

**Purpose:** Tell JVM where to find **.class files**

```
CLASSPATH → Location of .class files
```

### Setting CLASSPATH

#### Windows

```bash
# Current directory only
set classpath=.

# Current directory + other locations
set classpath=.;C:\myproject\classes;%classpath%

# Multiple locations
set classpath=.;C:\lib1;C:\lib2;D:\myapp\bin
```

#### Linux/Mac

```bash
# Current directory only
export CLASSPATH=.

# Current directory + other locations
export CLASSPATH=.:$CLASSPATH

# Multiple locations
export CLASSPATH=.:/home/user/lib:/opt/myapp/classes
```

### CLASSPATH Examples

#### Example 1: Running from Different Directory

```
Project structure:
MyProject/
├── src/
│   └── Main.java
└── bin/
    └── Main.class
```

**Without CLASSPATH:**
```bash
cd MyProject
java bin/Main
# ❌ Error: Could not find or load main class bin.Main
```

**With CLASSPATH:**
```bash
cd MyProject
set classpath=bin;.
java Main
# ✅ Works! JVM finds Main.class in bin/
```

#### Example 2: External Libraries

```bash
# Include external JAR files
set classpath=.;lib/mysql-connector.jar;lib/gson.jar
```

### PATH vs CLASSPATH Comparison

| Aspect | PATH | CLASSPATH |
|--------|------|-----------|
| **Purpose** | Find executables | Find .class files |
| **Points to** | `.exe` files | `.class` files |
| **Used by** | Operating System | Java (JVM) |
| **Scope** | All programs | Java-specific |
| **Contains** | `javac.exe`, `java.exe` | `.class` files, JARs |
| **Example** | `C:\Java\jdk-17\bin` | `.;C:\myapp\classes` |

### Complete Workflow

```
Step 1: Write code
MyApp.java

Step 2: Compile (needs PATH)
javac MyApp.java
       ↑
    OS uses PATH to find javac.exe
       ↓
MyApp.class

Step 3: Run (needs CLASSPATH)
java MyApp
     ↑
  JVM uses CLASSPATH to find MyApp.class
```

### The `.` in PATH and CLASSPATH

```bash
set path=C:\Java\jdk-17\bin;.;%path%
#                             ↑
#                          Current directory

set classpath=.;C:\myapp\classes
#              ↑
#           Current directory
```

**Why include `.` (current directory)?**
- Allows running commands from current folder
- Standard practice for development
- Convenience during coding

---

## 📚 JAR Files

### What is a JAR File?

**JAR = Java Archive**

A JAR file is a **zipped package** containing multiple `.class` files and resources.

```
JAR File = ZIP file containing:
├── .class files
├── Resources (images, configs)
├── META-INF/
│   └── MANIFEST.MF
└── Libraries
```

### Why Use JAR Files?

1. **Distribution** - Single file instead of many .class files
2. **Compression** - Smaller file size
3. **Portability** - Easy to share and deploy
4. **Organization** - Keep related classes together
5. **Execution** - Can be made executable

### Creating JAR Files

#### Basic JAR Creation

```bash
# Create JAR from all .class files
jar cvf myapp.jar *.class

# c = create
# v = verbose (show details)
# f = file (specify filename)
```

#### Create JAR with Directory Structure

```bash
jar cvf myapp.jar com/mycompany/*.class resources/
```

#### Create Executable JAR

```bash
# Step 1: Create manifest file (manifest.txt)
Main-Class: Main

# Step 2: Create JAR with manifest
jar cvfm myapp.jar manifest.txt *.class
```

### Using JAR Files

#### Add to CLASSPATH

```bash
# Windows
set classpath=.;myapp.jar;lib/util.jar

# Linux/Mac
export CLASSPATH=.:myapp.jar:lib/util.jar
```

#### Run Executable JAR

```bash
java -jar myapp.jar
```

#### Extract JAR Contents

```bash
jar xvf myapp.jar

# x = extract
# v = verbose
# f = file
```

#### List JAR Contents

```bash
jar tvf myapp.jar

# t = table (list contents)
# v = verbose
# f = file
```

---

## 🔄 Error Handling and Re-execution

### Error Workflow

```
Write code
    ↓
Compile
    ↓
Error? ──→ Yes ──→ Fix error ──→ Recompile
    │                              ↓
    No                          Success
    ↓
Execute
```

### JVM Execution Flow

```
JVM always starts from main()
    ↓
Executes instructions
    ↓
If error occurs:
    ├─ Fix the code
    ├─ Recompile
    └─ JVM starts fresh from main() again
```

### Re-execution Pattern

#### Example: Restart on Critical Error

```java
class ErrorHandler {
    public static void main(String[] args) {
        try {
            System.out.println("Application starting...");
            
            // Simulate critical operation
            int result = performCriticalTask();
            
            System.out.println("Task completed: " + result);
            
        } catch (CriticalException e) {
            System.out.println("Critical error! Restarting...");
            
            // Restart from beginning
            main(args);
        }
    }
    
    static int performCriticalTask() throws CriticalException {
        // Task logic
        return 42;
    }
}
```

### Importance of main() as Entry Point

1. **Consistent Starting Point**
   - JVM always knows where to begin
   - Predictable execution

2. **Restart Capability**
   - Can call main() to restart
   - Useful for recovery scenarios

3. **Standard Convention**
   - Universal across Java applications
   - Easy to understand and maintain

---

## 🎯 Quick Reference

### File Naming Rules

```
public class Abc → File must be Abc.java ✅
class Xyz → File can be anything ✅
Only ONE public class per file ✅
```

### Compilation Commands

```bash
# Compile single file
javac MyApp.java

# Compile multiple files
javac File1.java File2.java

# Compile all .java files
javac *.java

# Compile with destination
javac -d bin src/*.java
```

### Execution Commands

```bash
# Run class
java MyApp

# Run with classpath
java -cp .;lib/* MyApp

# Run JAR
java -jar myapp.jar
```

### PATH and CLASSPATH

```bash
# Windows
set path=C:\Java\jdk-17\bin;.;%path%
set classpath=.;lib/*;%classpath%

# Linux/Mac
export PATH=/usr/lib/jvm/java-17/bin:$PATH
export CLASSPATH=.:lib/*:$CLASSPATH
```

---

## 🔗 Navigation

- [← Previous: Java Architecture](../03-Java-Architecture/README.md)
- [Next: Memory Management →](../05-Memory-Management/README.md)

---

## 📝 Summary

In this module, you learned:

- ✅ File naming rules for public and non-public classes
- ✅ How to use multiple classes in one file
- ✅ Multiple main() methods and their behavior
- ✅ What generates .class files
- ✅ PATH vs CLASSPATH differences
- ✅ Working with JAR files
- ✅ Error handling and re-execution

**Next Step:** Understand memory management, references, and garbage collection!

---

*Happy Learning! 🚀*