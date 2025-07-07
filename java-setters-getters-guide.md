# Java Setters and Getters Guide

## What are Setters and Getters?

Setters and getters are methods used in Java to access and modify the values of private fields in a class. They are fundamental to the concept of **encapsulation** in object-oriented programming.

- **Getter (Accessor)**: A method that retrieves the value of a private field
- **Setter (Mutator)**: A method that sets or updates the value of a private field

## Why Use Setters and Getters?

1. **Encapsulation**: Hide internal implementation details
2. **Data Validation**: Control what values can be assigned to fields
3. **Data Protection**: Prevent direct access to sensitive data
4. **Flexibility**: Easy to modify internal logic without changing the interface

## Basic Syntax

### Naming Conventions
- Getters: `get` + FieldName (camelCase)
- Setters: `set` + FieldName (camelCase)
- For boolean fields: `is` + FieldName instead of `get`

### Basic Example

```java
public class Person {
    // Private fields (encapsulated)
    private String name;
    private int age;
    private boolean isStudent;
    
    // Getter for name
    public String getName() {
        return name;
    }
    
    // Setter for name
    public void setName(String name) {
        this.name = name;
    }
    
    // Getter for age
    public int getAge() {
        return age;
    }
    
    // Setter for age with validation
    public void setAge(int age) {
        if (age >= 0 && age <= 150) {
            this.age = age;
        } else {
            System.out.println("Invalid age!");
        }
    }
    
    // Getter for boolean (using 'is' prefix)
    public boolean isStudent() {
        return isStudent;
    }
    
    // Setter for boolean
    public void setStudent(boolean student) {
        this.isStudent = student;
    }
}
```

## Advanced Examples

### 1. With Constructors

```java
public class Student {
    private String studentId;
    private String name;
    private double gpa;
    
    // Constructor
    public Student(String studentId, String name, double gpa) {
        this.studentId = studentId;
        this.name = name;
        setGpa(gpa); // Using setter for validation
    }
    
    // Getters
    public String getStudentId() {
        return studentId;
    }
    
    public String getName() {
        return name;
    }
    
    public double getGpa() {
        return gpa;
    }
    
    // Setters
    public void setStudentId(String studentId) {
        if (studentId != null && !studentId.trim().isEmpty()) {
            this.studentId = studentId;
        }
    }
    
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;
        }
    }
    
    public void setGpa(double gpa) {
        if (gpa >= 0.0 && gpa <= 4.0) {
            this.gpa = gpa;
        } else {
            throw new IllegalArgumentException("GPA must be between 0.0 and 4.0");
        }
    }
}
```

### 2. Read-Only Fields (Only Getters)

```java
public class BankAccount {
    private final String accountNumber; // final field
    private double balance;
    
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }
    
    // Only getter for account number (read-only)
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public double getBalance() {
        return balance;
    }
    
    // Custom methods instead of direct setter
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
}
```

### 3. Complex Data Types

```java
import java.util.List;
import java.util.ArrayList;
import java.util.Date;

public class Course {
    private String courseName;
    private List<String> students;
    private Date startDate;
    
    public Course() {
        this.students = new ArrayList<>();
    }
    
    // String getter/setter
    public String getCourseName() {
        return courseName;
    }
    
    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }
    
    // List getter - return defensive copy
    public List<String> getStudents() {
        return new ArrayList<>(students);
    }
    
    // List setter - create defensive copy
    public void setStudents(List<String> students) {
        this.students = new ArrayList<>(students);
    }
    
    // Date getter - return defensive copy
    public Date getStartDate() {
        return startDate != null ? new Date(startDate.getTime()) : null;
    }
    
    // Date setter - create defensive copy
    public void setStartDate(Date startDate) {
        this.startDate = startDate != null ? new Date(startDate.getTime()) : null;
    }
    
    // Helper methods for list manipulation
    public void addStudent(String student) {
        if (student != null && !student.trim().isEmpty()) {
            students.add(student);
        }
    }
    
    public void removeStudent(String student) {
        students.remove(student);
    }
}
```

## Usage Example

```java
public class Main {
    public static void main(String[] args) {
        // Creating a Person object
        Person person = new Person();
        
        // Using setters to set values
        person.setName("John Doe");
        person.setAge(25);
        person.setStudent(true);
        
        // Using getters to retrieve values
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
        System.out.println("Is Student: " + person.isStudent());
        
        // Trying to set invalid age
        person.setAge(-5); // This will print "Invalid age!"
        System.out.println("Age after invalid input: " + person.getAge()); // Still 25
    }
}
```

## Best Practices

1. **Always make fields private** unless there's a specific reason not to
2. **Use validation** in setters to ensure data integrity
3. **Use meaningful names** following Java naming conventions
4. **Return defensive copies** for mutable objects to prevent external modification
5. **Consider using builders** for objects with many fields
6. **Don't create getters/setters for every field** - only create them when needed
7. **Use IDE generation** tools to avoid manual typing (most IDEs can auto-generate them)

## Common Patterns

### Builder Pattern (Alternative to many setters)
```java
public class PersonBuilder {
    private String name;
    private int age;
    private boolean isStudent;
    
    public PersonBuilder setName(String name) {
        this.name = name;
        return this;
    }
    
    public PersonBuilder setAge(int age) {
        this.age = age;
        return this;
    }
    
    public PersonBuilder setStudent(boolean student) {
        this.isStudent = student;
        return this;
    }
    
    public Person build() {
        Person person = new Person();
        person.setName(this.name);
        person.setAge(this.age);
        person.setStudent(this.isStudent);
        return person;
    }
}

// Usage:
Person person = new PersonBuilder()
    .setName("Alice")
    .setAge(30)
    .setStudent(false)
    .build();
```

## IDE Tips

Most modern IDEs (IntelliJ IDEA, Eclipse, VS Code) can automatically generate getters and setters:
- **IntelliJ IDEA**: Right-click → Generate → Getter and Setter
- **Eclipse**: Right-click → Source → Generate Getters and Setters
- **VS Code**: Use Java extensions for code generation

This saves time and ensures consistent formatting!