# 06 - Practice Programs

> Hands-on Java programs to reinforce concepts learned

---

## 📚 Table of Contents

1. [Program 1: Employee Salary Management](#program-1-employee-salary-management)
2. [Program 2: Room Area Calculator](#program-2-room-area-calculator)
3. [Program 3: Multiple Main Methods Demo](#program-3-multiple-main-methods-demo)
4. [Program 4: Reference Variables Demo](#program-4-reference-variables-demo)
5. [Program 5: Garbage Collection Demo](#program-5-garbage-collection-demo)
6. [Additional Exercises](#additional-exercises)

---

## 💼 Program 1: Employee Salary Management

### Problem Statement
Write a program to manage employee salaries with the ability to raise salaries by a given percentage.

### Code

```java
class Employee {
    // Instance variables
    private int id;
    private String name;
    private double salary;
    
    // Constructor
    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }
    
    // Method to raise salary by percentage
    public void raiseSalary(double percentage) {
        if (percentage > 0) {
            double increment = salary * percentage / 100;
            salary += increment;
            System.out.println("Salary raised by " + percentage + "%");
            System.out.println("Increment amount: $" + increment);
        } else {
            System.out.println("Invalid percentage!");
        }
    }
    
    // Method to display employee details
    public void display() {
        System.out.println("─────────────────────────");
        System.out.println("Employee ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Salary: $" + String.format("%.2f", salary));
        System.out.println("─────────────────────────");
    }
    
    // Getters
    public int getId() {
        return id;
    }
    
    public String getName() {
        return name;
    }
    
    public double getSalary() {
        return salary;
    }
}

public class EmployeeDemo {
    public static void main(String[] args) {
        System.out.println("===== Employee Salary Management =====\n");
        
        // Create employee objects
        Employee emp1 = new Employee(101, "Alice Johnson", 50000);
        Employee emp2 = new Employee(102, "Bob Smith", 60000);
        Employee emp3 = new Employee(103, "Charlie Brown", 45000);
        
        // Display original details
        System.out.println("BEFORE SALARY RAISE:");
        emp1.display();
        emp2.display();
        emp3.display();
        
        System.out.println("\n===== Applying Salary Raises =====\n");
        
        // Raise salaries
        emp1.raiseSalary(10);  // 10% raise
        emp2.raiseSalary(15);  // 15% raise
        emp3.raiseSalary(12);  // 12% raise
        
        // Display updated details
        System.out.println("\nAFTER SALARY RAISE:");
        emp1.display();
        emp2.display();
        emp3.display();
    }
}
```

### Output

```
===== Employee Salary Management =====

BEFORE SALARY RAISE:
─────────────────────────
Employee ID: 101
Name: Alice Johnson
Salary: $50000.00
─────────────────────────
─────────────────────────
Employee ID: 102
Name: Bob Smith
Salary: $60000.00
─────────────────────────
─────────────────────────
Employee ID: 103
Name: Charlie Brown
Salary: $45000.00
─────────────────────────

===== Applying Salary Raises =====

Salary raised by 10.0%
Increment amount: $5000.0
Salary raised by 15.0%
Increment amount: $9000.0
Salary raised by 12.0%
Increment amount: $5400.0

AFTER SALARY RAISE:
─────────────────────────
Employee ID: 101
Name: Alice Johnson
Salary: $55000.00
─────────────────────────
─────────────────────────
Employee ID: 102
Name: Bob Smith
Salary: $69000.00
─────────────────────────
─────────────────────────
Employee ID: 103
Name: Charlie Brown
Salary: $50400.00
─────────────────────────
```

### Concepts Demonstrated

✅ **Class and Object Creation**  
✅ **Encapsulation** (private variables, public methods)  
✅ **Constructor Usage**  
✅ **Instance Variables**  
✅ **Method Implementation**  
✅ **Object State Management**

---

## 📐 Program 2: Room Area Calculator

### Problem Statement
Create a program to calculate the area and perimeter of different rooms.

### Code

```java
class Room {
    // Instance variables
    private double length;
    private double width;
    private String roomName;
    
    // Constructor
    public Room(String roomName, double length, double width) {
        this.roomName = roomName;
        this.length = length;
        this.width = width;
    }
    
    // Method to calculate area
    public double calculateArea() {
        return length * width;
    }
    
    // Method to calculate perimeter
    public double calculatePerimeter() {
        return 2 * (length + width);
    }
    
    // Method to display room details
    public void displayDetails() {
        System.out.println("\n╔════════════════════════════════════╗");
        System.out.println("  Room: " + roomName);
        System.out.println("╠════════════════════════════════════╣");
        System.out.println("  Dimensions:");
        System.out.println("  • Length: " + length + " meters");
        System.out.println("  • Width: " + width + " meters");
        System.out.println("  ────────────────────────────────");
        System.out.println("  Calculations:");
        System.out.println("  • Area: " + calculateArea() + " sq meters");
        System.out.println("  • Perimeter: " + calculatePerimeter() + " meters");
        System.out.println("╚════════════════════════════════════╝");
    }
    
    // Method to compare with another room
    public void compareWith(Room other) {
        double thisArea = this.calculateArea();
        double otherArea = other.calculateArea();
        
        System.out.println("\n📊 Comparison: " + this.roomName + " vs " + other.roomName);
        System.out.println("   " + this.roomName + " area: " + thisArea + " sq m");
        System.out.println("   " + other.roomName + " area: " + otherArea + " sq m");
        
        if (thisArea > otherArea) {
            double diff = thisArea - otherArea;
            System.out.println("   ➜ " + this.roomName + " is larger by " + diff + " sq m");
        } else if (thisArea < otherArea) {
            double diff = otherArea - thisArea;
            System.out.println("   ➜ " + other.roomName + " is larger by " + diff + " sq m");
        } else {
            System.out.println("   ➜ Both rooms have the same area!");
        }
    }
}

public class RoomDemo {
    public static void main(String[] args) {
        System.out.println("╔════════════════════════════════════════╗");
        System.out.println("║   ROOM AREA CALCULATOR SYSTEM         ║");
        System.out.println("╚════════════════════════════════════════╝");
        
        // Create room objects
        Room bedroom = new Room("Bedroom", 15.5, 12.0);
        Room livingRoom = new Room("Living Room", 20.0, 18.5);
        Room kitchen = new Room("Kitchen", 10.0, 8.5);
        Room bathroom = new Room("Bathroom", 6.0, 5.5);
        
        // Display all room details
        bedroom.displayDetails();
        livingRoom.displayDetails();
        kitchen.displayDetails();
        bathroom.displayDetails();
        
        // Compare rooms
        System.out.println("\n" + "=".repeat(40));
        System.out.println("ROOM COMPARISONS");
        System.out.println("=".repeat(40));
        
        bedroom.compareWith(livingRoom);
        kitchen.compareWith(bathroom);
        
        // Calculate total area
        double totalArea = bedroom.calculateArea() + 
                          livingRoom.calculateArea() + 
                          kitchen.calculateArea() + 
                          bathroom.calculateArea();
        
        System.out.println("\n" + "=".repeat(40));
        System.out.println("📐 Total House Area: " + totalArea + " sq meters");
        System.out.println("=".repeat(40));
    }
}
```

### Output

```
╔════════════════════════════════════════╗
║   ROOM AREA CALCULATOR SYSTEM         ║
╚════════════════════════════════════════╝

╔════════════════════════════════════╗
  Room: Bedroom
╠════════════════════════════════════╣
  Dimensions:
  • Length: 15.5 meters
  • Width: 12.0 meters
  ────────────────────────────────
  Calculations:
  • Area: 186.0 sq meters
  • Perimeter: 55.0 meters
╚════════════════════════════════════╝

╔════════════════════════════════════╗
  Room: Living Room
╠════════════════════════════════════╣
  Dimensions:
  • Length: 20.0 meters
  • Width: 18.5 meters
  ────────────────────────────────
  Calculations:
  • Area: 370.0 sq meters
  • Perimeter: 77.0 meters
╚════════════════════════════════════╝

╔════════════════════════════════════╗
  Room: Kitchen
╠════════════════════════════════════╣
  Dimensions:
  • Length: 10.0 meters
  • Width: 8.5 meters
  ────────────────────────────────
  Calculations:
  • Area: 85.0 sq meters
  • Perimeter: 37.0 meters
╚════════════════════════════════════╝

╔════════════════════════════════════╗
  Room: Bathroom
╠════════════════════════════════════╣
  Dimensions:
  • Length: 6.0 meters
  • Width: 5.5 meters
  ────────────────────────────────
  Calculations:
  • Area: 33.0 sq meters
  • Perimeter: 23.0 meters
╚════════════════════════════════════╝

========================================
ROOM COMPARISONS
========================================

📊 Comparison: Bedroom vs Living Room
   Bedroom area: 186.0 sq m
   Living Room area: 370.0 sq m
   ➜ Living Room is larger by 184.0 sq m

📊 Comparison: Kitchen vs Bathroom
   Kitchen area: 85.0 sq m
   Bathroom area: 33.0 sq m
   ➜ Kitchen is larger by 52.0 sq m

========================================
📐 Total House Area: 674.0 sq meters
========================================
```

### Concepts Demonstrated

✅ **Object Creation and Initialization**  
✅ **Method Return Values**  
✅ **Object Interaction** (comparing objects)  
✅ **Data Encapsulation**  
✅ **Formatted Output**

---

## 🔀 Program 3: Multiple Main Methods Demo

### Problem Statement
Demonstrate how multiple classes can have main() methods and how they can call each other.

### Code

```java
class Application1 {
    public static void main(String[] args) {
        System.out.println("┌─────────────────────────────────┐");
        System.out.println("│  Application 1 Started          │");
        System.out.println("└─────────────────────────────────┘");
        
        System.out.println("\n→ Calling Application 2...\n");
        Application2.main(args);
        
        System.out.println("\n← Back to Application 1");
        System.out.println("Application 1 Complete!");
    }
}

class Application2 {
    public static void main(String[] args) {
        System.out.println("┌─────────────────────────────────┐");
        System.out.println("│  Application 2 Started          │");
        System.out.println("└─────────────────────────────────┘");
        
        System.out.println("\n→ Calling Application 3...\n");
        Application3.main(args);
        
        System.out.println("\n← Back to Application 2");
        System.out.println("Application 2 Complete!");
    }
}

class Application3 {
    public static void main(String[] args) {
        System.out.println("┌─────────────────────────────────┐");
        System.out.println("│  Application 3 Started          │");
        System.out.println("└─────────────────────────────────┘");
        
        System.out.println("Performing final tasks...");
        System.out.println("Application 3 Complete!");
    }
}

public class MultiMainDemo {
    public static void main(String[] args) {
        System.out.println("╔═════════════════════════════════════╗");
        System.out.println("║  MULTIPLE MAIN METHODS DEMO        ║");
        System.out.println("╚═════════════════════════════════════╝\n");
        
        System.out.println("Starting chain of main() calls...\n");
        Application1.main(args);
        
        System.out.println("\n╔═════════════════════════════════════╗");
        System.out.println("║  All Applications Completed!       ║");
        System.out.println("╚═════════════════════════════════════╝");
    }
}
```

### Output

```
╔═════════════════════════════════════╗
║  MULTIPLE MAIN METHODS DEMO        ║
╚═════════════════════════════════════╝

Starting chain of main() calls...

┌─────────────────────────────────┐
│  Application 1 Started          │
└─────────────────────────────────┘

→ Calling Application 2...

┌─────────────────────────────────┐
│  Application 2 Started          │
└─────────────────────────────────┘

→ Calling Application 3...

┌─────────────────────────────────┐
│  Application 3 Started          │
└─────────────────────────────────┘
Performing final tasks...
Application 3 Complete!

← Back to Application 2
Application 2 Complete!

← Back to Application 1
Application 1 Complete!

╔═════════════════════════════════════╗
║  All Applications Completed!       ║
╚═════════════════════════════════════╝
```

### Concepts Demonstrated

✅ **Multiple main() Methods**  
✅ **Static Method Calls**  
✅ **Method Call Chain**  
✅ **main() as Entry Point**  
✅ **Execution Flow Control**

---

## 🔗 Program 4: Reference Variables Demo

### Problem Statement
Demonstrate how reference variables work, including multiple references to the same object.

### Code

```java
class Car {
    String brand;
    String model;
    int speed;
    
    public Car(String brand, String model) {
        this.brand = brand;
        this.model = model;
        this.speed = 0;
    }
    
    public void accelerate(int increment) {
        speed += increment;
        System.out.println(brand + " " + model + " accelerating...");
        System.out.println("Current speed: " + speed + " km/h");
    }
    
    public void displayInfo() {
        System.out.println("─────────────────────────");
        System.out.println("Brand: " + brand);
        System.out.println("Model: " + model);
        System.out.println("Speed: " + speed + " km/h");
        System.out.println("─────────────────────────");
    }
}

public class ReferenceDemo {
    public static void main(String[] args) {
        System.out.println("╔════════════════════════════════╗");
        System.out.println("║  REFERENCE VARIABLES DEMO     ║");
        System.out.println("╚════════════════════════════════╝\n");
        
        // Create first car
        System.out.println("1️⃣  Creating car1...");
        Car car1 = new Car("Toyota", "Camry");
        car1.displayInfo();
        
        // Create second reference to same object
        System.out.println("\n2️⃣  Creating car2 = car1 (same object)...");
        Car car2 = car1;  // Both point to same object
        
        System.out.println("\ncar1 info:");
        car1.displayInfo();
        System.out.println("car2 info:");
        car2.displayInfo();
        
        // Modify through car1
        System.out.println("\n3️⃣  Accelerating through car1...");
        car1.accelerate(50);
        
        // Check both references
        System.out.println("\n4️⃣  Checking both references:");
        System.out.println("car1 speed: " + car1.speed);
        System.out.println("car2 speed: " + car2.speed);
        System.out.println("➜ Both show same speed (same object!)");
        
        // Create completely new object
        System.out.println("\n5️⃣  Creating new object for car2...");
        car2 = new Car("Honda", "Civic");
        
        System.out.println("\ncar1 info:");
        car1.displayInfo();
        System.out.println("car2 info:");
        car2.displayInfo();
        
        System.out.println("➜ Now they're different objects!");
        
        // Demonstrate null reference
        System.out.println("\n6️⃣  Setting car1 to null...");
        car1 = null;
        System.out.println("car1 = " + car1);
        System.out.println("Original Toyota object eligible for GC!");
    }
}
```

### Output

```
╔════════════════════════════════╗
║  REFERENCE VARIABLES DEMO     ║
╚════════════════════════════════╝

1️⃣  Creating car1...
─────────────────────────
Brand: Toyota
Model: Camry
Speed: 0 km/h
─────────────────────────

2️⃣  Creating car2 = car1 (same object)...

car1 info:
─────────────────────────
Brand: Toyota
Model: Camry
Speed: 0 km/h
─────────────────────────
car2 info:
─────────────────────────
Brand: Toyota
Model: Camry
Speed: 0 km/h
─────────────────────────

3️⃣  Accelerating through car1...
Toyota Camry accelerating...
Current speed: 50 km/h

4️⃣  Checking both references:
car1 speed: 50
car2 speed: 50
➜ Both show same speed (same object!)

5️⃣  Creating new object for car2...

car1 info:
─────────────────────────
Brand: Toyota
Model: Camry
Speed: 50 km/h
─────────────────────────
car2 info:
─────────────────────────
Brand: Honda
Model: Civic
Speed: 0 km/h
─────────────────────────
➜ Now they're different objects!

6️⃣  Setting car1 to null...
car1 = null
Original Toyota object eligible for GC!
```

### Concepts Demonstrated

✅ **Reference Variables**  
✅ **Multiple References to Same Object**  
✅ **Object Independence**  
✅ **Null References**  
✅ **Garbage Collection Eligibility**

---

## 🗑️ Program 5: Garbage Collection Demo

### Problem Statement
Demonstrate how objects become eligible for garbage collection.

### Code

```java
class Student {
    String name;
    int rollNo;
    
    public Student(String name, int rollNo) {
        this.name = name;
        this.rollNo = rollNo;
        System.out.println("✅ Student created: " + name + " (Roll: " + rollNo + ")");
    }
    
    // This method is called by GC before object is destroyed
    protected void finalize() {
        System.out.println("🗑️  Garbage Collecting: " + name + " (Roll: " + rollNo + ")");
    }
}

public class GarbageCollectionDemo {
    public static void main(String[] args) {
        System.out.println("╔════════════════════════════════════╗");
        System.out.println("║  GARBAGE COLLECTION DEMO          ║");
        System.out.println("╚════════════════════════════════════╝\n");
        
        // Scenario 1: Setting reference to null
        System.out.println("📍 Scenario 1: Setting reference to null");
        System.out.println("─".repeat(40));
        Student s1 = new Student("Alice", 101);
        Student s2 = new Student("Bob", 102);
        
        s1 = null;  // Alice becomes eligible for GC
        System.out.println("➜ s1 set to null (Alice eligible for GC)\n");
        
        // Scenario 2: Reassigning reference
        System.out.println("📍 Scenario 2: Reassigning reference");
        System.out.println("─".repeat(40));
        Student s3 = new Student("Charlie", 103);
        Student s4 = new Student("David", 104);
        
        s3 = s4;  // Charlie becomes eligible for GC
        System.out.println("➜ s3 = s4 (Charlie eligible for GC)\n");
        
        // Scenario 3: Anonymous object
        System.out.println("📍 Scenario 3: Anonymous object");
        System.out.println("─".repeat(40));
        new Student("Eve", 105);  // Immediately eligible for GC
        System.out.println("➜ Anonymous object (Eve eligible for GC)\n");
        
        // Scenario 4: Object going out of scope
        System.out.println("📍 Scenario 4: Object going out of scope");
        System.out.println("─".repeat(40));
        {
            Student temp = new Student("Frank", 106);
            System.out.println("➜ temp is in inner scope");
        }  // temp goes out of scope
        System.out.println("➜ temp out of scope (Frank eligible for GC)\n");
        
        // Request garbage collection
        System.out.println("📍 Requesting Garbage Collection...");
        System.out.println("─".repeat(40));
        System.gc();  // Suggest GC to run
        
        // Give GC time to run
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("\n✅ Demo completed!");
        System.out.println("Note: Actual GC timing is non-deterministic");
    }
}
```

### Output

```
╔════════════════════════════════════╗
║  GARBAGE COLLECTION DEMO          ║
╚════════════════════════════════════╝

📍 Scenario 1: Setting reference to null
────────────────────────────────────────
✅ Student created: Alice (Roll: 101)
✅ Student created: Bob (Roll: 102)
➜ s1 set to null (Alice eligible for GC)

📍 Scenario 2: Reassigning reference
────────────────────────────────────────
✅ Student created: Charlie (Roll: 103)
✅ Student created: David (Roll: 104)
➜ s3 = s4 (Charlie eligible for GC)

📍 Scenario 3: Anonymous object
────────────────────────────────────────
✅ Student created: Eve (Roll: 105)
➜ Anonymous object (Eve eligible for GC)

📍 Scenario 4: Object going out of scope
────────────────────────────────────────
✅ Student created: Frank (Roll: 106)
➜ temp is in inner scope
➜ temp out of scope (Frank eligible for GC)

📍 Requesting Garbage Collection...
────────────────────────────────────────
🗑️  Garbage Collecting: Frank (Roll: 106)
🗑️  Garbage Collecting: Eve (Roll: 105)
🗑️  Garbage Collecting: Charlie (Roll: 103)
🗑️  Garbage Collecting: Alice (Roll: 101)

✅ Demo completed!
Note: Actual GC timing is non-deterministic
```

### Concepts Demonstrated

✅ **Object Creation and Destruction**  
✅ **GC Eligibility Scenarios**  
✅ **finalize() Method**  
✅ **System.gc() Usage**  
✅ **Non-deterministic GC**

---

## 📝 Additional Exercises

### Exercise 1: Bank Account System

Create a `BankAccount` class with:
- Account number, holder name, balance
- Methods: deposit(), withdraw(), checkBalance()
- Demonstrate creating multiple accounts

### Exercise 2: Student Grade Calculator

Create a `Student` class with:
- Name, roll number, marks in 3 subjects
- Methods: calculateTotal(), calculateAverage(), displayGrade()
- Grade logic: 90+ = A, 80-89 = B, etc.

### Exercise 3: Library Book Management

Create a `Book` class with:
- Title, author, ISBN, availability
- Methods: issueBook(), returnBook(), displayInfo()
- Track multiple books

### Exercise 4: Temperature Converter

Create a `Temperature` class with:
- Value and unit (Celsius/Fahrenheit)
- Methods: toCelsius(), toFahrenheit(), toKelvin()
- Conversion demonstrations

---

## 🎯 Key Takeaways

From these practice programs, you've learned:

✅ **Object-Oriented Design**
- Creating classes with proper encapsulation
- Using constructors effectively
- Implementing methods with clear purposes

✅ **Object Management**
- Creating and initializing objects
- Managing object references
- Understanding object lifecycle

✅ **Memory Concepts**
- Reference variables behavior
- Garbage collection scenarios
- Memory management best practices

✅ **Java Fundamentals**
- Multiple main() methods usage
- Static method calls
- Execution flow control

---

## 🔗 Navigation

- [← Previous: Memory Management](../05-Memory-Management/README.md)
- [Back to Main README](../README.md)

---

## 📝 Summary

These practice programs reinforce:

- ✅ Class and object creation
- ✅ Encapsulation and data hiding
- ✅ Reference variables and pointers
- ✅ Memory management and GC
- ✅ Multiple main() methods
- ✅ Object interaction and comparison

**Keep practicing to master Java fundamentals!**

---

*Happy Coding! 💻*