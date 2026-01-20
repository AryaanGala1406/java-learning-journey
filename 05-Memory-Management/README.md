# 05 - Memory Management

> Understanding reference variables, pointers, heap memory, and garbage collection in Java

---

## 📚 Table of Contents

1. [Object Creation and Initialization](#object-creation-and-initialization)
2. [Reference Variables](#reference-variables)
3. [Pointers in Java](#pointers-in-java)
4. [Memory Management](#memory-management)
5. [Garbage Collection](#garbage-collection)

---

## 🎯 Object Creation and Initialization

### The Problem: Uninitialized Reference

#### ❌ Incorrect Approach

```java
class Abc {
    int x;
}

public class Test {
    public static void main(String[] args) {
        Abc a1;  // Only declaration, no object created
        System.out.println(a1.x);  // ❌ ERROR!
    }
}
```

**Error:**
```
Error: variable a1 might not have been initialized
```

**Why Error?**
```
a1 is just a reference variable
    ↓
No object has been created
    ↓
No memory allocated in heap
    ↓
Cannot access properties of non-existent object
```

### Memory State - Incorrect Approach

```
┌─────────────────────────────┐
│  Stack Memory               │
│  ┌───────────────────────┐  │
│  │ a1: ???               │  │ Reference declared
│  │ (uninitialized)       │  │ but not pointing
│  └───────────────────────┘  │ to any object
└─────────────────────────────┘

┌─────────────────────────────┐
│  Heap Memory                │
│                             │
│  (Empty - No object)        │
│                             │
└─────────────────────────────┘
```

### ✅ Correct Approach

```java
class Abc {
    int x;
}

public class Test {
    public static void main(String[] args) {
        Abc a1 = new Abc();  // Object created and assigned
        System.out.println(a1.x);  // ✅ Output: 0
        // Default value for int is 0
    }
}
```

### Memory State - Correct Approach

```
┌─────────────────────────────┐
│  Stack Memory               │
│  ┌───────────────────────┐  │
│  │ a1: 0x1A2B           │──┼──┐
│  │ (reference)           │  │  │
│  └───────────────────────┘  │  │
└─────────────────────────────┘  │
                                 │
┌─────────────────────────────┐  │
│  Heap Memory                │  │
│  ┌───────────────────────┐  │  │
│  │ Abc Object @ 0x1A2B   │◄─┘
│  │ ┌───────────────────┐ │  │
│  │ │ x = 0             │ │  │
│  │ └───────────────────┘ │  │
│  └───────────────────────┘  │
└─────────────────────────────┘
```

### Default Values

When an object is created, instance variables get default values:

| Data Type | Default Value |
|-----------|---------------|
| `byte`    | 0 |
| `short`   | 0 |
| `int`     | 0 |
| `long`    | 0L |
| `float`   | 0.0f |
| `double`  | 0.0d |
| `boolean` | false |
| `char`    | '\u0000' (null character) |
| Reference types | null |

**Example:**
```java
class Person {
    int age;           // Default: 0
    String name;       // Default: null
    boolean active;    // Default: false
    double salary;     // Default: 0.0
}

Person p = new Person();
System.out.println(p.age);     // 0
System.out.println(p.name);    // null
System.out.println(p.active);  // false
System.out.println(p.salary);  // 0.0
```

---

## 🔗 Reference Variables

### What is a Reference Variable?

A **reference variable** stores the **memory address** of an object.

```
Reference Variable = Pointer to Object's Memory Location
```

### Understanding References

```java
class Car {
    String brand;
    int speed;
}

public class Demo {
    public static void main(String[] args) {
        Car c1 = new Car();
        //  ↑      ↑
        //  │      └── Creates object in heap
        //  └── Reference variable stores address
    }
}
```

### Memory Diagram

```
Stack Memory:                Heap Memory:
┌──────────────┐            ┌─────────────────┐
│              │            │                 │
│  c1: 0x1A2B  │───────────→│  Car Object     │
│  (reference) │            │  @0x1A2B        │
│              │            │  brand: null    │
└──────────────┘            │  speed: 0       │
                            └─────────────────┘
```

### Multiple References to Same Object

```java
class Demo {
    public static void main(String[] args) {
        Car c1 = new Car();
        Car c2 = c1;  // c2 now points to same object
        
        c1.speed = 100;
        System.out.println(c2.speed);  // 100
        // Both reference same object!
    }
}
```

**Memory:**
```
Stack Memory:                Heap Memory:
┌──────────────┐            ┌─────────────────┐
│  c1: 0x1A2B  │───────────→│  Car Object     │
│              │            │  @0x1A2B        │
│  c2: 0x1A2B  │───────────→│  brand: null    │
│              │            │  speed: 100     │
└──────────────┘            └─────────────────┘
                Both point to same object!
```

### Multiple Objects

```java
class Demo {
    public static void main(String[] args) {
        Car c1 = new Car();  // First object
        Car c2 = new Car();  // Second object
        Car c3 = new Car();  // Third object
        
        c1.speed = 100;
        c2.speed = 120;
        c3.speed = 80;
        
        // Each has independent data
    }
}
```

**Memory:**
```
Stack Memory:                Heap Memory:
┌──────────────┐            ┌─────────────────┐
│  c1: 0x1A2B  │───────────→│  Car @ 0x1A2B   │
│              │            │  speed: 100     │
│  c2: 0x3C4D  │──────┐     └─────────────────┘
│              │      │     ┌─────────────────┐
│  c3: 0x5E6F  │──┐   └────→│  Car @ 0x3C4D   │
│              │  │         │  speed: 120     │
└──────────────┘  │         └─────────────────┘
                  │         ┌─────────────────┐
                  └────────→│  Car @ 0x5E6F   │
                            │  speed: 80      │
                            └─────────────────┘
```

---

## 🔍 Pointers in Java

### Do Java Have Pointers?

**Short Answer:** Yes, but they're called "reference variables"

**Long Answer:** Java has pointers, but with restrictions for safety

### Java References vs C/C++ Pointers

```
C/C++ Pointers:
• Full pointer arithmetic
• Can manipulate memory addresses
• Dangerous if misused
• Explicit pointer syntax

Java References:
• Cannot do arithmetic
• Cannot manipulate addresses directly
• Safe and managed
• Hidden pointer syntax
```

### Reference Assignment (✅ Allowed)

```java
class Demo {
    public static void main(String[] args) {
        Abc a1 = new Abc();
        Abc a2 = new Abc();
        
        a1 = a2;  // ✅ ALLOWED
        // a1 now points to same object as a2
    }
}
```

**Memory Changes:**

**Before Assignment:**
```
a1 → Object1 @ 0x1A2B
a2 → Object2 @ 0x3C4D
```

**After `a1 = a2`:**
```
a1 → Object2 @ 0x3C4D
a2 → Object2 @ 0x3C4D
Object1 @ 0x1A2B → (Eligible for garbage collection)
```

### Pointer Arithmetic (❌ NOT Allowed)

```java
class Demo {
    public static void main(String[] args) {
        Abc a1 = new Abc();
        
        a1++;     // ❌ Compilation Error
        a1--;     // ❌ Compilation Error
        a1 = a1 + 1;  // ❌ Compilation Error
        a1 = a1 - 1;  // ❌ Compilation Error
        
        // Cannot perform arithmetic on references!
    }
}
```

**Error Message:**
```
Error: bad operand types for binary operator '+'
  first type:  Abc
  second type: int
```

### C vs Java Pointer Comparison

#### C Language - Full Pointer Control

```c
int* ptr = malloc(sizeof(int));
*ptr = 10;

ptr++;     // ✅ Allowed - moves to next memory location
ptr--;     // ✅ Allowed - moves to previous location
*ptr = 20; // ✅ Allowed - access/modify memory

free(ptr); // Manual memory management required
```

#### Java - Restricted References

```java
Integer obj = new Integer(10);

obj = another;  // ✅ Allowed - reassign reference
// obj++;       // ❌ Not allowed - no arithmetic
// *obj;        // ❌ No dereference operator
// obj = null;  // ✅ Allowed - set to null

// Garbage collector handles memory automatically
```

### Comparison Table

| Feature | C/C++ Pointers | Java References |
|---------|----------------|-----------------|
| **Arithmetic** | ✅ Allowed (`ptr++`) | ❌ Not allowed |
| **Direct memory access** | ✅ Allowed | ❌ Not allowed |
| **Null pointer** | Can crash | NullPointerException (handled) |
| **Memory management** | Manual (`free()`) | Automatic (GC) |
| **Reassignment** | ✅ Allowed | ✅ Allowed |
| **Type safety** | ❌ Weak | ✅ Strong |
| **Dereferencing** | `*ptr` | Automatic |

---

## 🧠 Memory Management

### JRE's Role in Pointer Management

```
┌─────────────────────────────────────────┐
│  Java Developer                         │
│  ↓                                      │
│  Uses reference variables (safe API)    │
│  ↓                                      │
│  JRE (Java Runtime Environment)         │
│  ↓                                      │
│  Manages actual pointers internally     │
│  ↓                                      │
│  Operating System & Hardware            │
└─────────────────────────────────────────┘
```

### What JRE Handles Internally

1. **Memory Allocation**
   - Allocates heap memory for objects
   - Manages memory addresses

2. **Pointer Safety**
   - Prevents invalid memory access
   - No buffer overflows
   - No dangling pointers

3. **Garbage Collection**
   - Automatic memory cleanup
   - Frees unreferenced objects

4. **Type Safety**
   - Ensures type compatibility
   - Prevents illegal casts

### Memory Areas

```
┌─────────────────────────────────────────┐
│         Method Area                     │
│  • Class metadata                       │
│  • Static variables                     │
│  • Constant pool                        │
│  • Method bytecode                      │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│           Heap                          │
│  • All objects                          │
│  • Instance variables                   │
│  • Arrays                               │
│  • Shared among all threads             │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│       Stack (per thread)                │
│  • Method call frames                   │
│  • Local variables                      │
│  • Reference variables                  │
│  • Each thread has its own stack        │
└─────────────────────────────────────────┘
```

### Example: Memory Allocation

```java
class Employee {
    static int count = 0;  // Method Area (static)
    
    int id;                // Heap (instance variable)
    String name;           // Heap (instance variable)
    
    public Employee(int id, String name) {
        this.id = id;      // Stack → Heap
        this.name = name;  // Stack → Heap
        count++;
    }
    
    public void display() {
        int salary = 50000;  // Stack (local variable)
        System.out.println(name + ": " + salary);
    }
}

public class Demo {
    public static void main(String[] args) {
        Employee e1 = new Employee(1, "Alice");
        //       ↑                  ↑
        //    Stack (ref)      Heap (object)
    }
}
```

**Memory Layout:**
```
Method Area:
┌───────────────────┐
│ Employee.class    │
│ count: 1          │ ← Static variable
└───────────────────┘

Stack (main thread):
┌───────────────────┐
│ main() frame      │
│  e1: 0x1A2B       │ ← Reference
└───────────────────┘

Heap:
┌───────────────────┐
│ Employee @ 0x1A2B │
│  id: 1            │
│  name: "Alice"    │
└───────────────────┘
```

---

## 🗑️ Garbage Collection

### What is Garbage Collection?

**Garbage Collection** = Automatic memory management

```
Object created → Used → No longer referenced → GC frees memory
```

### Real-Life Analogy

```
Real Life:
    Person born
    ↓
    Lives and interacts
    ↓
    Person dies (no longer exists)
    ↓
    Natural processes reclaim resources
    ↓
    Space available for new life

Java:
    Object created
    ↓
    Used in program
    ↓
    No references (object "dies")
    ↓
    Garbage Collector reclaims memory
    ↓
    Memory available for new objects
```

### When Object Becomes Eligible for GC

An object is eligible for garbage collection when:
1. **No references point to it**
2. **All references are null**
3. **References go out of scope**

### Example 1: Setting Reference to Null

```java
class Demo {
    public static void main(String[] args) {
        Person p1 = new Person("Alice");
        Person p2 = new Person("Bob");
        
        p1 = null;  // Person object "Alice" eligible for GC
        p2 = null;  // Person object "Bob" eligible for GC
        
        // Garbage collector will free memory
    }
}
```

**Memory State:**

**Before `p1 = null`:**
```
p1 → Person("Alice")  ← Referenced (Safe)
p2 → Person("Bob")    ← Referenced (Safe)
```

**After `p1 = null`:**
```
p1 → null
Person("Alice") ← No references (Eligible for GC) 🗑️
p2 → Person("Bob")  ← Still referenced (Safe)
```

### Example 2: Reassignment

```java
class Demo {
    public static void main(String[] args) {
        Car c1 = new Car("Toyota");   // Object 1
        Car c2 = new Car("Honda");    // Object 2
        
        c1 = c2;  // Object 1 becomes eligible for GC
        
        // Now both c1 and c2 point to "Honda"
        // "Toyota" has no references → GC eligible
    }
}
```

**Memory Changes:**

**Before Reassignment:**
```
c1 → Car("Toyota")  ← 1 reference
c2 → Car("Honda")   ← 1 reference
```

**After `c1 = c2`:**
```
c1 → Car("Honda")   ← 2 references
c2 → Car("Honda")   ← 2 references
Car("Toyota") ← 0 references (GC eligible) 🗑️
```

### Example 3: Reference Going Out of Scope

```java
class Demo {
    public static void main(String[] args) {
        createCar();  // Local reference
        
        // After method returns, Car object eligible for GC
        System.gc();  // Suggest GC (not guaranteed)
    }
    
    static void createCar() {
        Car c = new Car("Tesla");
        System.out.println(c);
    }  // c goes out of scope here → object eligible for GC
}
```

### Garbage Collection Process

```
1. Mark Phase
   ↓
   GC identifies unreachable objects
   ↓
2. Sweep Phase
   ↓
   GC removes unreachable objects
   ↓
3. Compact Phase (optional)
   ↓
   GC defragments memory
```

### Requesting Garbage Collection

```java
// Suggest JVM to run GC (not guaranteed)
System.gc();

// OR
Runtime.getRuntime().gc();
```

⚠️ **Important:** 
- `System.gc()` is just a **suggestion**
- JVM decides when to actually run GC
- Cannot force immediate garbage collection

### Benefits of Automatic GC

✅ **No Memory Leaks** - Automatic cleanup  
✅ **No Dangling Pointers** - Safe references  
✅ **Developer Productivity** - Focus on logic, not memory  
✅ **Safety** - Prevents common C/C++ errors  

### Drawbacks

❌ **Performance Overhead** - GC takes CPU time  
❌ **Pause Times** - Can pause application briefly  
❌ **Non-deterministic** - Don't know exactly when GC runs  

---

## 🎯 Key Takeaways

### Essential Points

1. **Always initialize objects**
   ```java
   Abc a1 = new Abc();  // ✅ Correct
   Abc a1;              // ❌ Uninitialized
   ```

2. **Reference variables store addresses**
   - Not the object itself
   - Just like pointers, but safer

3. **Java has pointers (references)**
   - Cannot do pointer arithmetic
   - JRE manages internally
   - Safe and type-checked

4. **Multiple references can point to same object**
   ```java
   Abc a1 = new Abc();
   Abc a2 = a1;  // Same object
   ```

5. **Garbage collection is automatic**
   - No manual `free()` or `delete`
   - JVM handles memory cleanup

### Memory Management Rules

```
✅ DO:
• Always initialize objects before use
• Set references to null when done
• Let GC handle memory cleanup
• Use proper scoping

❌ DON'T:
• Try to do pointer arithmetic
• Assume GC runs immediately
• Keep unnecessary references
• Manually manage memory
```

---

## 🔗 Navigation

- [← Previous: Compilation & Execution](../04-Compilation-and-Execution/README.md)
- [Next: Practice Programs →](../06-Practice-Programs/README.md)

---

## 📝 Summary

In this module, you learned:

- ✅ How to properly create and initialize objects
- ✅ Understanding reference variables
- ✅ Pointers in Java vs C/C++
- ✅ Memory areas and management
- ✅ Garbage collection mechanism
- ✅ When objects become eligible for GC

**Next Step:** Apply your knowledge with practice programs!

---

*Happy Learning! 🧠*