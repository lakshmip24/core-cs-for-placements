# Object-Oriented Programming (OOP) Notes

## 1. Why OOP? (Procedural vs Object-Oriented)

**Procedural programming** divides a program into functions. Data sits in variables, and functions operate on that data. As programs grow, data and the functions that use it drift apart, and it becomes hard to track which function changes what.

**OOP** groups related data and functions into one unit called an **object**.

| Term | Meaning | Example (BankAccount) |
|---|---|---|
| **Class** | Blueprint for objects | `BankAccount` |
| **Object** | An instance of a class | `myAccount` |
| **Property** (attribute) | Data stored in an object | `balance` |
| **Method** | Function that belongs to an object | `deposit()`, `withdraw()` |

**How OOP solves the procedural problem:** the data and the code that changes it live together, so the object controls its own state.

---

## 2. The Four Pillars

### 2.1 Encapsulation: wrapping data and methods into one unit

Hide the data inside the class, and allow changes only through methods that check the rules.

**Java**
```java
public class BankAccount {
    private double balance;                       // hidden from outside

    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be positive");
        balance += amount;
    }

    public double getBalance() { return balance; }
}
```

**Python**
```python
class BankAccount:
    def __init__(self):
        self.__balance = 0            # double underscore: name-mangled, treated as private

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")
        self.__balance += amount

    def get_balance(self):
        return self.__balance
```

**Interview answer:** "Encapsulation means the balance is private. It can only change through `deposit` and `withdraw`, which validate the input. This prevents invalid states, like a negative deposit."

---

### 2.2 Abstraction: hiding complexity from the user

Show only what the user needs to know (*what* it does), and hide how it works (*how* it does it).

**Java** (using an interface)
```java
interface PaymentMethod {
    void pay(double amount);
}

class UpiPayment implements PaymentMethod {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " via UPI");   // complex details hidden here
    }
}

// Usage: the caller doesn't know how payment works internally
PaymentMethod p = new UpiPayment();
p.pay(500);
```

**Python** (using `abc`)
```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class UpiPayment(PaymentMethod):
    def pay(self, amount):
        print(f"Paid {amount} via UPI")

p = UpiPayment()
p.pay(500)
```

**Interview answer:** "Abstraction lets the caller use `pay()` without knowing how UPI, card, or wallet payments work. I can add a new payment method without changing the code that calls it."

---

### 2.3 Inheritance: parent-child relationship

A child class gets the properties and methods of a parent class and can add its own.

**Java**
```java
class Vehicle {
    void start() { System.out.println("Starting"); }
}

class Car extends Vehicle {       // Car inherits from Vehicle
    int doors = 4;                // Car-specific property
}

Car c = new Car();
c.start();                        // inherited from Vehicle
```

**Python**
```python
class Vehicle:
    def start(self):
        print("Starting")

class Car(Vehicle):               # Car inherits from Vehicle
    doors = 4

c = Car()
c.start()                         # inherited from Vehicle
```

**Interview answer:** "`Car` reuses `start()` from `Vehicle` without rewriting it. It only adds what is specific to a car, like the number of doors. This reduces duplicate code."

---

### 2.4 Polymorphism: many forms

The same method name behaves differently depending on the object or the arguments.

There are two types:

- **Compile-time polymorphism (method overloading):** same method name, different parameters. Decided before the program runs.
- **Run-time polymorphism (method overriding):** a child class replaces a parent's method. Decided while the program runs.

**Compile-time: Java (overloading)**
```java
class Calculator {
    int add(int a, int b)       { return a + b; }
    double add(double a, double b) { return a + b; }
}
```

**Compile-time: Python** (Python does not support true overloading. Use default arguments instead.)
```python
class Calculator:
    def add(self, a, b=0, c=0):
        return a + b + c
```

**Run-time: Java (overriding)**
```java
class Shape {
    double area() { return 0; }
}

class Circle extends Shape {
    double r = 2;
    @Override
    double area() { return Math.PI * r * r; }
}

Shape s = new Circle();
System.out.println(s.area());     // Circle's version runs
```

**Run-time: Python (overriding)**
```python
class Shape:
    def area(self):
        return 0

class Circle(Shape):
    def __init__(self, r):
        self.r = r
    def area(self):
        return 3.14 * self.r ** 2

for shape in [Circle(2)]:
    print(shape.area())           # Circle's version runs
```

**Interview answer:** "In a list of shapes, I can call `area()` on each one. Each shape runs its own formula, so the calling code doesn't need to check the type."

---

## 3. Java vs Python: Which Supports What?

| Concept | Java | Python |
|---|---|---|
| Encapsulation | Yes. Strict access modifiers (`private`, `protected`, `public`) | Yes, by convention. `_name` means protected, `__name` is name-mangled. No strict enforcement |
| Abstraction | Yes. `abstract` classes and `interface` | Yes. `abc` module (`ABC`, `@abstractmethod`) |
| Inheritance | Single inheritance for classes. Multiple via interfaces | Yes, including multiple inheritance |
| Compile-time polymorphism (overloading) | Yes | No. Use default or variable arguments |
| Run-time polymorphism (overriding) | Yes | Yes. Also supports duck typing |

**Summary:** Both languages support all four pillars. Java enforces them more strictly and checks overloading at compile time. Python is more flexible and relies on conventions.

---

## 4. Quick Revision

- **Object** = data (properties) + behavior (methods)
- **Encapsulation** = hide data, control access through methods
- **Abstraction** = show what, hide how
- **Inheritance** = child reuses and extends parent
- **Polymorphism** = same method name, different behavior (overloading at compile time, overriding at run time)

---

## 5. Likely Interview Questions

1. What is the difference between procedural and object-oriented programming?
2. What is the difference between encapsulation and abstraction?
3. Why doesn't Python support method overloading?
4. What is the difference between overloading and overriding?
5. Can a Java class inherit from two classes? (No. It can implement two interfaces.)
