# 07 - Constructors and this Keyword

> Deep dive into constructors, the this keyword, constructor chaining, and best practices

---

## 📚 Table of Contents

1. [How to Write Programs - Best Approach](#how-to-write-programs---best-approach)
2. [Constructor Basics](#constructor-basics)
3. [Default Constructor](#default-constructor)
4. [The this Keyword](#the-this-keyword)
5. [Constructor Chaining](#constructor-chaining)
6. [Constructor vs Method](#constructor-vs-method)
7. [Best Practices](#best-practices)

---

## 📝 How to Write Programs - Best Approach

### The Noun-Verb-Entity Rule

When designing Java programs, use this simple mapping:

| English Grammar | Programming Concept | Java Representation |
|----------------|---------------------|---------------------|
| **Noun** (Person, Place, Thing) | Object/Entity | **Class Name** |
| **Verb** (Action, Behavior) | Behavior | **Method Name** |
| **Entity** (Elements needed) | Properties | **Variables** |

### Example 1: Increase Employee Salary

**Identify components:**
- **Noun**: Employee → **Class name**
- **Verb**: raiseSalary → **Method name**
- **Entity**: salary → **Variable**

```java
class Employee {
    float salary;  // Entity (variable)
    
    // Verb (method - action)
    public void raiseSalary(int amount) {
        salary = salary + amount;
    }
}
```

### Example 2: Find Area of Room

**Identify components:**
- **Noun**: Room → **Class name**
- **Verb**: findArea → **Method name**
- **Entities**: length, breadth → **Variables**

```java
class Room {
    int length;   // Entity
    int breadth;  // Entity
    
    // Verb (method - action)
    public void findArea() {
        System.out.println(length * breadth);
    }
}
```

---

## 🏗️ Class Relationships: IS-A vs HAS-A

### When to Create Separate Classes?

When you have multiple nouns/classes, check their relationship:

```
Two Types of Relationships:
    ├── IS-A Relationship  → Use INHERITANCE (extends)
    └── HAS-A Relationship → Use COMPOSITION (object as field)
```

### IS-A Relationship (Inheritance)

**Rule:** If Class A **IS-A** Class B, use `extends`

```java
class Human {
    String name;
    int age;
}

class Employee extends Human {
    // Employee IS-A Human
    float salary;
}
```

**Example Hierarchy:**
```
        Human
          ↑
          |
      Employee
    (IS-A Human)
```

### HAS-A Relationship (Composition)

**Rule:** If Class A **HAS-A** Class B, use object declaration

```java
class Address {
    String city;
    String state;
    int pincode;
}

class Employee {
    // Employee HAS-A Address
    String name;
    float salary;
    Address address;  // HAS-A relationship
}
```

**Example Structure:**
```
Employee
    ├── name (String)
    ├── salary (float)
    └── address (Address object)
            ├── city
            ├── state
            └── pincode
```

### Complete Example

```java
class Address {
    String city;
    String state;
    int pincode;
    
    Address(String city, String state, int pincode) {
        this.city = city;
        this.state = state;
        this.pincode = pincode;
    }
}

class Human {
    String name;
    int age;
    
    Human(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Employee extends Human {  // IS-A relationship
    float salary;
    Address address;  // HAS-A relationship
    
    Employee(String name, int age, float salary, Address address) {
        super(name, age);  // Call parent constructor
        this.salary = salary;
        this.address = address;
    }
    
    void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Salary: " + salary);
        System.out.println("City: " + address.city);
    }
}
```

---

## 📋 Where to Write the main() Method?

### Business Class vs Main Class

#### ❌ Bad Practice: main() in Business Class

```java
class Employee {
    float salary;
    
    void raiseSalary(int amount) {
        salary += amount;
    }
    
    // ❌ Avoid this!
    public static void main(String[] args) {
        Employee e = new Employee();
        // execution code...
    }
}
```

**Problems:**
- Reduces reusability
- Mixes business logic with execution logic
- Harder to maintain

#### ✅ Good Practice: Separate Main Class

```java
// Business class (no main method)
class Employee {
    float salary;
    
    void raiseSalary(int amount) {
        salary += amount;
    }
}

// Main class (execution only)
public class Main {
    public static void main(String[] args) {
        Employee e = new Employee();
        e.salary = 50000;
        e.raiseSalary(5000);
    }
}
```

**Benefits:**
- ✅ Clear separation of concerns
- ✅ Business class is reusable
- ✅ Easier to maintain and test
- ✅ Main class should be `public` for direct execution

### Best Practice Rule

```
Business Class:
    ├── Properties (variables)
    ├── Methods (behaviors)
    └── ❌ NO main() method

Main Class:
    ├── public class (for easy execution)
    └── ✅ main() method only
```

---

## 🔧 Constructor Basics

### What is a Constructor?

A **constructor** is a special method that:
- Has the **same name as the class**
- Has **no return type** (not even `void`)
- Is called **automatically** when object is created
- Used to **initialize** object state

### Memory Allocation with Constructors

```java
class Room {
    int length;
    int width;
}

public class Main {
    public static void main(String[] args) {
        Room r1;          // Step 1: Reference variable created
        r1 = new Room();  // Step 2: Object created in heap
    }
}
```

**What happens:**

**Step 1: `Room r1;`**
```
Stack Memory:
┌──────────────┐
│ r1: ???      │  Reference declared but uninitialized
└──────────────┘

Heap Memory:
(empty)
```

**Step 2: `r1 = new Room();`**
```
Stack Memory:           Heap Memory:
┌──────────────┐       ┌─────────────────┐
│ r1: 0x1A2B   │──────→│ Room @ 0x1A2B   │
└──────────────┘       │ length: 0       │ ← Default values
                       │ width: 0        │    by constructor
                       └─────────────────┘
```

### What `new` Keyword Does

```
new Room();
    ↓
1. Allocates heap memory for instance variables
2. Calls constructor
3. Returns memory address
```

**Memory allocated:**
- Only for **instance variables**
- Methods are stored separately (shared among all objects)
- Each object gets its own copy of variables

### Example: Memory Calculation

```java
class Room {
    int length;   // 4 bytes
    int width;    // 4 bytes
    
    public void area() {
        // Method stored separately
    }
}

// new Room() allocates: 4 + 4 = 8 bytes
```

**For 100 objects:**
- Variables: 100 copies (100 × 8 bytes)
- Methods: 1 copy (shared)

---

## 🎯 Default Constructor

### Who Creates the Default Constructor?

**Answer:** The **Java Compiler (javac)**

### When is it Created?

```
If NO constructor is defined
    ↓
Compiler automatically adds default constructor
    ↓
If ANY constructor is defined
    ↓
Compiler does NOT add default constructor
```

### Seeing the Default Constructor

Use **`javap`** tool to view class structure:

```bash
javac Room.java
javap Room
```

**Output:**
```
class Room {
  int length;
  int width;
  Room();           ← Default constructor (added by compiler)
  void area();
}
```

### Example: No Constructor Defined

```java
class Room {
    int length;
    int width;
    
    void area() {
        System.out.println(length * width);
    }
}

public class Main {
    public static void main(String[] args) {
        Room r1 = new Room();  // ✅ Works! Default constructor exists
        r1.area();  // Output: 0 (default values)
    }
}
```

### Example: Constructor Defined

```java
class Room {
    int length;
    int width;
    
    // Parameterized constructor defined
    Room(int l, int w) {
        length = l;
        width = w;
    }
    
    void area() {
        System.out.println(length * width);
    }
}

public class Main {
    public static void main(String[] args) {
        Room r1 = new Room();      // ❌ ERROR! No default constructor
        Room r2 = new Room(10, 20); // ✅ Works!
    }
}
```

**Error:**
```
error: constructor Room in class Room cannot be applied to given types
  required: int, int
  found: no arguments
```

### Default Values Assigned by Constructor

| Data Type | Default Value |
|-----------|---------------|
| `int`, `long`, `short`, `byte` | `0` |
| `float`, `double` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` (null character) |
| Object references (String, etc.) | `null` |

**Important:** Java has **no concept of garbage values** (unlike C/C++)

---

## 🔑 The this Keyword

### What is `this`?

**`this`** is a reference to the **current object**

### Uses of `this`

1. **Distinguish between instance variables and parameters**
2. **Call methods of current object**
3. **Call other constructors (constructor chaining)**
4. **Return current object**

### Use Case 1: Parameter vs Instance Variable

**Problem:**
```java
class Room {
    int length;
    
    Room(int length) {
        length = length;  // ❌ Ambiguous! Which length?
    }
}
```

**Solution:**
```java
class Room {
    int length;
    
    Room(int length) {
        this.length = length;  // ✅ Clear!
        // this.length → instance variable
        // length → parameter
    }
}
```

### Use Case 2: Method Call

```java
class Room {
    int length;
    int width;
    
    Room(int length, int width) {
        this.length = length;
        this.width = width;
        this.display();  // Call method using this
    }
    
    void display() {
        System.out.println("Room created: " + this.length + "x" + this.width);
    }
}
```

### Use Case 3: Return Current Object

```java
class Room {
    int length;
    
    Room setLength(int length) {
        this.length = length;
        return this;  // Return current object
    }
    
    void display() {
        System.out.println("Length: " + length);
    }
}

// Usage - Method chaining
Room r = new Room();
r.setLength(10).display();  // Chaining methods
```

---

## 🔗 Constructor Chaining

### What is Constructor Chaining?

**Constructor chaining** = Calling one constructor from another constructor

### Syntax: `this()`

```java
this();           // Call no-argument constructor
this(args);       // Call parameterized constructor
```

### Rules for `this()`

1. ⚠️ **Must be the FIRST statement** in constructor
2. ✅ Can only call ONE constructor
3. ❌ Cannot create circular chain

### Example: Complete Chain

```java
class Room {
    int length;
    int width;
    
    // Constructor 1: No arguments
    Room() {
        System.out.println("------------");
    }
    
    // Constructor 2: One argument
    Room(String label) {
        this();  // ✅ Calls Constructor 1
        System.out.print("Area: ");
    }
    
    // Constructor 3: Two arguments
    Room(int length, int width) {
        this("Room Label");  // ✅ Calls Constructor 2
        this.length = length;
        this.width = width;
        this.area();
    }
    
    void area() {
        System.out.println(length * width);
    }
}

public class Main {
    public static void main(String[] args) {
        Room r1 = new Room(20, 30);
    }
}
```

**Output:**
```
------------
Area: 600
```

**Execution Flow:**
```
new Room(20, 30)
    ↓
Room(int, int) called
    ↓
this("Room Label") → Room(String)
    ↓
this() → Room()
    ↓
Print "------------"
    ↓
Return to Room(String)
    ↓
Print "Area: "
    ↓
Return to Room(int, int)
    ↓
Set length = 20, width = 30
    ↓
Call this.area()
    ↓
Print "600"
```

### Visual Representation

```
Room(int, int)
    │
    └─→ this("abc") ─→ Room(String)
                           │
                           └─→ this() ─→ Room()
                                            │
                                            └─→ Execute
                                            ↓
                                         Return
                           ↓
                        Return
    ↓
Execute rest of constructor
```

### Common Mistakes

#### ❌ Error: Not First Statement

```java
Room(int length, int width) {
    this.length = length;
    this();  // ❌ ERROR! Must be first statement
}
```

**Error:**
```
error: call to this must be first statement in constructor
```

#### ❌ Error: Circular Chain

```java
Room() {
    this(10);  // Calls Room(int)
}

Room(int x) {
    this();  // ❌ ERROR! Calls Room() → Infinite loop!
}
```

---

## ⚖️ Constructor vs Method

### Comprehensive Comparison

| Aspect | Constructor | Method |
|--------|-------------|--------|
| **Name** | Must be same as class name | Can have any valid name |
| **Return Type** | ❌ No return type (not even void) | ✅ Must have return type |
| **Invocation** | Automatically called at object creation | Called explicitly on object |
| **Inheritance** | ❌ Not inherited | ✅ Inherited by subclass |
| **Overriding** | ❌ Cannot be overridden | ✅ Can be overridden |
| **Overloading** | ✅ Can be overloaded | ✅ Can be overloaded |
| **Access Modifiers** | ✅ public, private, protected | ✅ public, private, protected |
| **Non-access Modifiers** | ❌ final, static NOT allowed | ✅ final, static, abstract allowed |
| **Calls per Object** | Called once per object | Can be called multiple times |
| **Purpose** | Initialize object state | Perform operations |

### Detailed Examples

#### Name Rule

```java
class Room {
    // ✅ Constructor - same name as class
    Room() {
        System.out.println("Constructor");
    }
    
    // ✅ Method - any name
    void display() {
        System.out.println("Method");
    }
}
```

#### Return Type

```java
class Room {
    // ✅ Constructor - NO return type
    Room() {
        // Initialize
    }
    
    // ❌ This is NOT a constructor (it's a method)
    void Room() {
        // Because it has return type void
    }
    
    // ✅ Method - must have return type
    int getArea() {
        return 100;
    }
}
```

#### Invocation

```java
class Room {
    Room() {
        System.out.println("Constructor called");
    }
    
    void display() {
        System.out.println("Method called");
    }
}

public class Main {
    public static void main(String[] args) {
        Room r = new Room();  // Constructor called automatically
        r.display();          // Method called explicitly
    }
}
```

**Output:**
```
Constructor called
Method called
```

#### Inheritance

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
    
    void show() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("Child constructor");
    }
    
    // ✅ Methods are inherited and can be overridden
    @Override
    void show() {
        System.out.println("Child method");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.show();
        
        // Note: Constructors are NOT inherited
        // Child doesn't inherit Parent() constructor
        // But Parent() is called automatically
    }
}
```

**Output:**
```
Parent constructor
Child constructor
Child method
```

#### Non-Access Modifiers

```java
class Room {
    // ❌ ERROR - final not allowed in constructor
    // final Room() { }
    
    // ❌ ERROR - static not allowed in constructor
    // static Room() { }
    
    // ✅ Methods can have non-access modifiers
    final void display() {
        System.out.println("Final method");
    }
    
    static void info() {
        System.out.println("Static method");
    }
}
```

---

## 🎯 Similarities Between Constructors and Methods

Despite their differences, constructors and methods share some features:

### 1. Both Can Be Overloaded

```java
class Room {
    // Constructor overloading
    Room() { }
    Room(int l) { }
    Room(int l, int w) { }
    
    // Method overloading
    void display() { }
    void display(int x) { }
    void display(int x, int y) { }
}
```

### 2. Both Support Access Modifiers

```java
class Room {
    // public constructor
    public Room() { }
    
    // private constructor (singleton pattern)
    private Room(int x) { }
    
    // public method
    public void show() { }
    
    // private method
    private void calculate() { }
}
```

### 3. Both Can Contain Logic

```java
class Student {
    int age;
    
    // Constructor with logic
    Student(int age) {
        if (age < 0) {
            System.out.println("Invalid age");
            return;  // ✅ Early exit
        }
        this.age = age;
        System.out.println("Age set successfully");
    }
    
    // Method with logic
    void updateAge(int newAge) {
        if (newAge < age) {
            System.out.println("Cannot decrease age");
            return;  // ✅ Early exit
        }
        age = newAge;
    }
}
```

**Example Output:**
```java
Student s1 = new Student(-5);  // Invalid age
Student s2 = new Student(20);  // Age set successfully
```

### 4. Both Can Have return Statement

```java
class Example {
    // Constructor with return
    Example(int value) {
        if (value < 0) {
            System.out.println("Invalid value");
            return;  // ✅ Exits constructor (no value returned)
        }
        System.out.println("Valid value: " + value);
    }
    
    // Method with return
    int process(int value) {
        if (value < 0) {
            return -1;  // ✅ Returns value
        }
        return value * 2;
    }
}
```

**Key Difference:**
- Constructor's `return` = exits constructor (no value)
- Method's `return` = exits method and returns value

---

## 📋 Best Practices

### 1. Separation of Concerns

```java
// ✅ Good: Business class without main
class Employee {
    private String name;
    private double salary;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    public void raiseSalary(double percentage) {
        salary += salary * percentage / 100;
    }
}

// ✅ Good: Separate main class
public class Main {
    public static void main(String[] args) {
        Employee emp = new Employee("John", 50000);
        emp.raiseSalary(10);
    }
}
```

### 2. Always Provide Default Constructor When Needed

```java
class Room {
    int length;
    int width;
    
    // If you provide parameterized constructor,
    // also provide default constructor if needed
    Room() {
        this(10, 10);  // Default dimensions
    }
    
    Room(int length, int width) {
        this.length = length;
        this.width = width;
    }
}
```

### 3. Use this for Clarity

```java
class Student {
    String name;
    int age;
    
    // ✅ Clear: using this
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // ❌ Confusing: different parameter names
    Student(String n, int a) {
        name = n;
        age = a;
    }
}
```

### 4. Constructor Chaining for Code Reuse

```java
class Rectangle {
    int length;
    int width;
    
    Rectangle() {
        this(0, 0);  // Use constructor chaining
    }
    
    Rectangle(int side) {
        this(side, side);  // Square
    }
    
    Rectangle(int length, int width) {
        this.length = length;
        this.width = width;
        validateDimensions();  // Common logic in one place
    }
    
    private void validateDimensions() {
        if (length < 0 || width < 0) {
            throw new IllegalArgumentException("Negative dimensions");
        }
    }
}
```

---

## 🎯 Quick Reference

### Constructor Quick Facts

```
✅ Same name as class
✅ No return type
✅ Called automatically at object creation
✅ Can be overloaded
✅ Can have access modifiers (public, private, protected)
❌ Cannot be inherited
❌ Cannot be overridden
❌ Cannot have non-access modifiers (final, static, abstract)
```

### this Keyword Quick Reference

```java
this.variable     // Access instance variable
this.method()     // Call instance method
this()            // Call constructor (must be first statement)
return this;      // Return current object
```

### Memory Allocation

```
new Keyword:
    ├── Allocates heap memory for instance variables
    ├── Calls constructor
    └── Returns memory address

Constructor:
    ├── Initializes instance variables
    └── Executes initialization logic
```

---

## 🔗 Navigation

- [← Previous: Practice Programs Day 1](../06-Practice-Programs/README.md)
- [Next: Practice Programs Day 2 →](../08-Practice-Programs-Day2/README.md)
- [Back to Main README](../README.md)

---

## 📝 Summary

In this module, you learned:

- ✅ How to approach writing Java programs (Noun-Verb-Entity)
- ✅ IS-A vs HAS-A relationships
- ✅ Business class vs Main class separation
- ✅ Constructor basics and default constructors
- ✅ The `this` keyword and its uses
- ✅ Constructor chaining with `this()`
- ✅ Constructor vs Method differences
- ✅ Best practices for constructors

**Next Step:** Practice with Day 2 programs to solidify these concepts!

---

*Happy Learning! 🚀*