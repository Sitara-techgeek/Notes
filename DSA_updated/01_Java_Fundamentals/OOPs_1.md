# OOPs 1

> **📅 Written:** 24 Sep 2026

### Definition

**Object-Oriented Programming (OOP) is a paradigm that organizes code around "objects" — bundles of data (fields) and behavior (methods) — modeled after real-world entities, built on four core pillars: Encapsulation, Abstraction, Inheritance, and Polymorphism.**

**In one sentence:**
> Instead of writing a pile of loose functions and variables, you model your program as a collection of objects that know their own data and know how to act on it.

*(This note covers Classes, Objects, Encapsulation, Constructors, and the `static` keyword — Inheritance and Polymorphism are covered in OOPs 2.)*

---

## Class vs Object

- **Class** — a blueprint/template; defines what fields and methods objects of this type will have
- **Object** — an actual instance created from that blueprint, with its own copy of the data

```java
class Car {
    String model;
    int speed;

    void accelerate() {
        speed += 10;
    }
}

Car myCar = new Car();     // object created from the Car class
myCar.model = "Tesla";
myCar.accelerate();         // speed becomes 10
```

```
Class Car (blueprint)         Objects (instances)
┌─────────────┐               myCar:  model="Tesla", speed=10
│ model       │      ---->    yourCar: model="Toyota", speed=20
│ speed       │               (each object has its OWN copy of the fields)
│ accelerate()│
└─────────────┘
```

---

## The Four Pillars of OOP — Overview

| Pillar | What it means |
|---|---|
| **Encapsulation** | Bundling data + methods together, restricting direct access to internal state |
| **Abstraction** | Hiding complex implementation details, exposing only what's necessary |
| **Inheritance** | A class acquiring properties/behavior from another class *(covered in OOPs 2)* |
| **Polymorphism** | The same interface behaving differently based on the actual object type *(covered in OOPs 2)* |

---

## 1. Encapsulation

### Definition

Bundling data (fields) and the methods that operate on it into a single unit (a class), while restricting direct external access to the internal state — typically via `private` fields and `public` getter/setter methods.

```java
class BankAccount {
    private double balance;   // hidden from outside direct access

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {           // validation lives HERE, guaranteed on every deposit
            balance += amount;
        }
    }
}
```

```
Without encapsulation:              With encapsulation:
account.balance = -500;  ❌          account.deposit(-500);  -> rejected by validation ✅
(anyone can corrupt state           (the class CONTROLS how its own state changes)
 directly, bypassing any rules)
```

> **Interview point ⭐:** Encapsulation isn't really about "hiding data for secrecy" — it's about **guaranteeing an object's internal state can only change through validated, controlled paths**, which is what makes large codebases maintainable and bug-resistant.

---

## Access Modifiers ⭐

| Modifier | Same Class | Same Package | Subclass (different package) | Everywhere |
|---|---|---|---|---|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *default* (no modifier) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

```java
public class Example {
    private int a;      // only within this class
    int b;                // default: only within this package
    protected int c;       // package + subclasses anywhere
    public int d;            // accessible from anywhere
}
```

---

## 2. Abstraction

### Definition

Exposing only the necessary details of an object's behavior, while hiding the complexity of *how* it's implemented underneath.

```java
class Calculator {
    public int add(int a, int b) {
        return performAddition(a, b);   // caller doesn't need to know HOW this works
    }

    private int performAddition(int a, int b) {
        // complex internal logic could live here
        return a + b;
    }
}
```

> A real-world analogy: driving a car requires knowing "press the accelerator to go faster" — not the internal combustion mechanics happening under the hood. That's abstraction: a simple interface hiding complex internals.

*(Abstraction is explored further with `abstract` classes and `interface` in OOPs 2, alongside Inheritance and Polymorphism.)*

---

## Constructors

### Definition

A special method, automatically called when an object is created, used to initialize its fields. Has the **same name as the class**, and **no return type** (not even `void`).

```java
class Car {
    String model;
    int speed;

    // Constructor
    Car(String model) {
        this.model = model;   // 'this' distinguishes the field from the parameter
        this.speed = 0;
    }
}

Car myCar = new Car("Tesla");   // constructor runs automatically
```

### The `this` Keyword

Refers to the **current object instance** — most commonly used to disambiguate a field from a constructor/method parameter with the same name.

```java
class Point {
    int x, y;
    Point(int x, int y) {
        this.x = x;   // this.x = the FIELD, x = the PARAMETER
        this.y = y;
    }
}
```

### Constructor Overloading

Multiple constructors with different parameter lists — Java picks the right one based on the arguments passed.

```java
class Car {
    String model;
    int speed;

    Car() {                          // default constructor
        this("Unknown");             // calls the other constructor below
    }

    Car(String model) {
        this.model = model;
        this.speed = 0;
    }
}
```

> **Interview point ⭐:** If you don't define **any** constructor, Java automatically provides a no-argument **default constructor**. The moment you define even one constructor yourself, that automatic default disappears — you'd need to write your own no-arg one explicitly if you still want it.

---

## The `static` Keyword

### Definition

Marks a field or method as belonging to the **class itself**, not to any individual object — shared across all instances, and accessible without creating an object.

```java
class Counter {
    static int count = 0;    // shared across ALL instances

    Counter() {
        count++;             // every new object increments the SAME shared count
    }
}

new Counter();
new Counter();
new Counter();
System.out.println(Counter.count);   // 3 — accessed via the CLASS, not an instance
```

```
Instance fields:                  Static fields:
each object has its OWN copy      ONE copy, shared by every object

obj1: {name: "A"}                 Counter.count = 3  <- one shared value
obj2: {name: "B"}                      ↑         ↑         ↑
obj3: {name: "C"}                   obj1      obj2      obj3  (all see the SAME count)
```

> **Interview point ⭐:** `main` is `static` precisely because the JVM needs to call it **before** any object of the class exists — there's nothing to instantiate yet, so it must belong to the class itself, not an instance.

### Static Methods

```java
class MathUtils {
    static int square(int x) {
        return x * x;
    }
}

int result = MathUtils.square(5);   // called on the CLASS, no object needed
```

> **Watch out ⭐:** A `static` method **cannot** directly access instance (non-static) fields or methods — since it doesn't belong to any particular object, there's no `this` for it to refer to.

---

## Quick Revision

- **Class** → blueprint; **Object** → instance with its own copy of the fields.
- **Encapsulation** → `private` fields + public getters/setters, controls how state changes.
- **Access modifiers** → `private` < default < `protected` < `public`, increasing visibility.
- **Abstraction** → exposing simple interfaces, hiding implementation complexity.
- **Constructor** → same name as class, no return type, runs on object creation; `this` disambiguates fields from parameters.
- **`static`** → belongs to the class, shared across all instances, accessible without an object; can't access instance members directly.

---

## Interview Definition ⭐

> **Encapsulation bundles an object's data with the methods that operate on it, restricting direct access via access modifiers so state can only change through validated paths, while `static` members belong to the class itself rather than any instance, shared across all objects and accessible without instantiation — together with abstraction (hiding implementation complexity behind simple interfaces), these form the foundation OOP is built on before introducing inheritance and polymorphism.**

---

## Must-Solve LeetCode Problems

*(OOP concepts are typically tested via design questions rather than algorithmic ones — the most relevant LeetCode problems are the "Design ___" family, which directly exercises encapsulation and clean class structure.)*

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Design HashMap](https://leetcode.com/problems/design-hashmap/) | Easy | Encapsulating internal state behind a clean public API |
| 2 | [Design Parking System](https://leetcode.com/problems/design-parking-system/) | Easy | Basic class modeling with private state |
| 3 | [Min Stack](https://leetcode.com/problems/min-stack/) | Medium | Encapsulated state with auxiliary tracking, all hidden behind a simple interface |
| 4 | [Design Twitter](https://leetcode.com/problems/design-twitter/) | Medium | Multiple classes/objects collaborating, good encapsulation practice |
