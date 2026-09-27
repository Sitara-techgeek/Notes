# OOPs 2

> **📅 Written:** 24 Sep 2026

### Definition

**Inheritance lets a class acquire the fields and methods of another class, forming an "is-a" relationship. Polymorphism lets the same method call behave differently depending on the actual object type — together they're what make OOP genuinely powerful beyond just bundling data with methods.**

**In one sentence:**
> Inheritance is "borrow and extend the family's traits"; polymorphism is "one instruction, different results, depending on who's actually receiving it."

*(Builds on Classes, Objects, and Encapsulation from OOPs 1.)*

---

## Inheritance

### Definition

A class (**subclass/child**) inherits fields and methods from another class (**superclass/parent**), and can add its own or override the inherited ones.

```java
class Animal {
    String name;
    void eat() {
        System.out.println(name + " is eating");
    }
}

class Dog extends Animal {      // Dog IS-A Animal
    void bark() {
        System.out.println(name + " is barking");
    }
}

Dog d = new Dog();
d.name = "Rex";    // inherited field
d.eat();            // inherited method
d.bark();            // Dog's own method
```

```
       Animal
      /   |   \
   Dog   Cat   Bird     <- each "IS-A" Animal, inherits name/eat()
```

### The `super` Keyword

Refers to the **parent class** — used to call the parent's constructor or an overridden method.

```java
class Animal {
    String name;
    Animal(String name) {
        this.name = name;
    }
    void eat() {
        System.out.println("Animal eating");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);          // calls Animal's constructor
    }
    @Override
    void eat() {
        super.eat();          // calls Animal's original eat() first
        System.out.println("Dog eating specifically");
    }
}
```

> **Interview point ⭐:** A subclass constructor **implicitly calls `super()`** (the parent's no-arg constructor) as its very first action, even if you don't write it — unless you explicitly call a different parent constructor yourself via `super(args)`.

### Single Inheritance Only (for Classes)

Java classes can extend only **one** superclass (no multiple inheritance of classes, unlike C++) — this avoids the classic "diamond problem" ambiguity. Multiple inheritance of *behavior* is achieved instead through **interfaces** (see below).

---

## Polymorphism

### Definition

The ability for the same method call to produce different behavior depending on the actual runtime type of the object — comes in two forms: **Compile-time** (overloading) and **Runtime** (overriding).

### 1. Compile-Time Polymorphism — Method Overloading

Multiple methods with the **same name** but **different parameter lists** (different number/type of parameters), resolved at compile time based on the arguments passed.

```java
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c) { return a + b + c; }
}
```
→ The compiler decides *which* `add` to call based on the argument types/count — happens before the program even runs.

### 2. Runtime Polymorphism — Method Overriding

A subclass provides its **own implementation** of a method already defined in its superclass, with the **same signature**. Which version runs is decided at **runtime**, based on the actual object type — not the reference type.

```java
class Animal {
    void makeSound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}

Animal a1 = new Dog();
Animal a2 = new Cat();
a1.makeSound();   // "Bark"  - decided at RUNTIME by the actual object type
a2.makeSound();   // "Meow"
```

```
Reference type: Animal                Actual object type decides behavior:
Animal a1 = new Dog();  -> a1.makeSound() calls Dog's version -> "Bark"
Animal a2 = new Cat();  -> a2.makeSound() calls Cat's version -> "Meow"
```

> **Interview point ⭐:** This is the essence of polymorphism's power — you can write code against the **superclass type** (`Animal a = ...`) and it automatically calls the **correct subclass behavior**, without needing `if/else` chains checking the object's actual type. This is what lets you add new subclasses later without touching existing code.

### Overloading vs Overriding ⭐

| Overloading | Overriding |
|---|---|
| Same class (or subclass with different signature) | Subclass, same signature as parent |
| Different parameter list | Exact same parameter list and return type |
| Resolved at **compile time** | Resolved at **runtime** |
| Not true polymorphism in the "dynamic dispatch" sense | The core mechanism behind runtime polymorphism |

---

## Abstract Classes

### Definition

A class that **cannot be instantiated directly**, and may contain both fully-implemented methods and **abstract methods** (declared but with no body) — subclasses are forced to implement the abstract ones.

```java
abstract class Shape {
    abstract double area();          // no body - MUST be implemented by subclasses

    void printArea() {               // fully implemented, inherited as-is
        System.out.println("Area: " + area());
    }
}

class Circle extends Shape {
    double radius;
    Circle(double radius) { this.radius = radius; }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}

Shape s = new Circle(5);
s.printArea();   // uses Circle's area() implementation

Shape s2 = new Shape();   // ❌ COMPILE ERROR - cannot instantiate an abstract class
```

---

## Interfaces

### Definition

A fully abstract type (traditionally) that defines a **contract** — a set of methods a class must implement, without dictating *how*. A class can implement **multiple** interfaces, which is how Java achieves multiple inheritance of behavior.

```java
interface Movable {
    void move();          // implicitly public abstract
}

interface Soundable {
    void makeSound();
}

class Dog implements Movable, Soundable {   // multiple interfaces - totally fine!
    @Override
    public void move() {
        System.out.println("Dog runs");
    }
    @Override
    public void makeSound() {
        System.out.println("Bark");
    }
}
```

### Default Methods (Java 8+)

Interfaces can now include methods **with** a body, using `default` — allows adding new methods to an interface without breaking every existing class that implements it.

```java
interface Greetable {
    default void greet() {
        System.out.println("Hello!");
    }
}
```

---

## Abstract Class vs Interface ⭐

| Abstract Class | Interface |
|---|---|
| `extends` — single inheritance only | `implements` — a class can implement **multiple** |
| Can have both abstract and concrete methods, plus fields | Traditionally only abstract methods; now also `default`/`static` methods (no instance fields) |
| Use when classes share **significant common code** | Use when defining a **capability/contract** across unrelated classes |
| "IS-A" relationship, closely related classes | "CAN-DO" relationship, possibly unrelated classes |

```java
// Example distinction:
abstract class Vehicle { ... }      // Car, Bike, Truck are all closely related "types of Vehicle"
interface Flyable { void fly(); }    // Both Bird AND Airplane can "fly" - unrelated otherwise
```

> **Interview point ⭐:** A very common design question: "when would you use an abstract class vs an interface?" — answer: abstract class when subclasses share substantial common implementation and are naturally related (IS-A); interface when you need to guarantee a capability across otherwise-unrelated classes (CAN-DO), or need multiple inheritance of behavior.

---

## Quick Revision

- **Inheritance** → `extends`, single inheritance for classes, `super` accesses the parent.
- **Overloading** → same name, different parameters, resolved at **compile time**.
- **Overriding** → same signature in a subclass, resolved at **runtime** — the core of true polymorphism.
- **Abstract class** → cannot instantiate, mix of implemented + abstract methods, single inheritance.
- **Interface** → contract of methods, a class can implement **multiple**, `default` methods allowed since Java 8.
- **Abstract class** → closely related classes sharing code (IS-A). **Interface** → unrelated classes sharing a capability (CAN-DO).

---

## Interview Definition ⭐

> **Inheritance lets a subclass acquire and extend a superclass's fields and methods via `extends`, while polymorphism allows the same method call to resolve differently — overloading at compile time based on parameters, and overriding at runtime based on the object's actual type — letting code written against a superclass or interface automatically invoke the correct subclass behavior; abstract classes suit closely related types sharing implementation, while interfaces define a capability contract implementable across multiple, otherwise unrelated classes.**

---

## Must-Solve LeetCode Problems

*(Like OOPs 1, this is best practiced through "Design ___" problems, which force real inheritance/polymorphism decisions.)*

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Design a Stack With Increment Operation](https://leetcode.com/problems/design-a-stack-with-increment-operation/) | Medium | Extending/overriding standard behavior on top of a base structure |
| 2 | [Design Underground System](https://leetcode.com/problems/design-underground-system/) | Medium | Multiple collaborating classes with clean interfaces |
| 3 | [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | Medium | Class design with encapsulated internal node structure |
| 4 | [LRU Cache](https://leetcode.com/problems/lru-cache/) | Medium | Strong exercise in designing a class's public contract vs private internals |
