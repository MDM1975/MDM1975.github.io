---
layout: post
img: ../assets/imgs/pillars-of-oop.jpg
summary: "This article explains the four fundamental principles of object-oriented programming: encapsulation, abstraction, inheritance, and polymorphism. These principles are the building blocks of OOP and provide a solid foundation for creating robust, scalable, and maintainable software."
---

##### The Four Fundamental Principles of OOP

The four fundamental principles of object-oriented programming are encapsulation, abstraction, inheritance, and polymorphism. These principles are the building blocks of OOP and provide the foundation for designing and developing robust, scalable, and maintainable software.

<pre class="mermaid">
graph LR

A[Encapsulation]
B[Abstraction]
C[Inheritance]
D[Polymorphism]

A --- B & C --- D
</pre>

##### Encapsulation

Encapsulation, a fundamental concept in object-oriented programming, is a mechanism that coalesces data and its related behaviors into a single composite unit, colloquially known as a class. This mechanism prohibits direct access to data, thereby averting inadvertent modification. As a result, encapsulation fosters data integrity and bolsters security, which is integral to robust software design.

This encapsulation principle empowers an object to conceal parts of its internal state and behaviors from other objects, revealing only a circumscribed interface to the remainder of the program. The encapsulation process renders certain elements private, making them accessible solely from within the methods of their class.

<pre class="mermaid">
classDiagram
    class BankAccount {
        - accountNumber: string
        - accountHolder: string
        - balance: double

        + deposit(amount: double): double
        + withdraw(amount: double): double
        + getBalance(): double
        + getAccountNumber(): string
        + getAccountHolder(): string
    }
</pre>

In this diagram, BankAccount is a class that encapsulates account details like accountNumber, accountHolder, and balance. These private attributes can be accessed and modified only by the class's methods. The class also has methods to deposit and withdraw funds and retrieve the account balance, account number, and account holder's name.

A slightly less restrictive mode, termed protected, allows class members to be accessed by subclasses.

<pre class="mermaid">
classDiagram
    class BankAccount {
        - accountNumber: string
        - accountHolder: string
        # balance: double

        + deposit(amount: double): double
        + withdraw(amount: double): double
        + getBalance(): double
        + getAccountNumber(): string
        + getAccountHolder(): string
    }

    class SavingsAccount {
        - interestRate: double
        + applyInterest(): double
    }

    BankAccount <|-- SavingsAccount : extends
</pre>

In this diagram, BankAccount is a class that encapsulates account details like accountNumber, accountHolder, and balance. The balance is protected and can be accessed and modified by BankAccount and its subclass SavingsAccount.

The SavingsAccount is a subclass of BankAccount and inherits its members. It also has an additional attribute interestRate and a method applyInterest() that applies interest to the account balance.

This demonstrates how protected members are encapsulated within a class yet accessible to subclasses, providing a balance between data hiding and data sharing in an object-oriented hierarchy.

##### Abstraction

In programming, objects of a program are often conceived as analogous to real-world entities. However, it is essential to note that these programmatic objects do not replicate their real-world counterparts with complete accuracy, and in fact, it is rarely expected or required for them to do so. Instead, these objects primarily model real-world entities' pertinent attributes and behaviors within a specific context, while the rest is disregarded.

Abstraction is a fundamental principle underlying this process, acting as a model of a real-world object or phenomenon restricted to a particular context. All details germane to the context are accurately represented in this model, while everything else is intentionally omitted.

Abstraction is a mechanism that effectively conceals a class's implementation details and exposes only its functionality to the end user. Consequently, this mechanism allows users to interact with the class without understanding its intricate, underlying implementation. This abstraction principle facilitates code reusability and maintainability, enhancing software's overall quality and robustness.

Here's an illustration using a Vehicle abstract class and its concrete classes Car and Motorcycle:

<pre class="mermaid">
classDiagram
    class Vehicle {
        + currentSpeed(): integer
        + fuelCapacity(): integer
    }

    class Car {
        + currentSpeed(): integer
        + fuelCapacity(): integer
        + numberOfDoors(): integer
    }

    class Motorcycle {
        + currentSpeed(): integer
        + fuelCapacity(): integer
        + hasSideCar(): boolean
    }

    Vehicle <|-- Car : extends
    Vehicle <|-- Motorcycle : extends
</pre>

In this diagram, Vehicle represents the abstraction of a real-world concept. It defines the currentSpeed() and fuelCapacity() methods that must be implemented by any concrete class that extends Vehicle. These methods represent the necessary attributes of a vehicle in this context.

The Car and Motorcycle classes extend Vehicle, thereby inheriting its properties and behaviors. They represent more specific instances of vehicles and thus contain additional methods—numberOfDoors() for Car and hasSideCar() for Motorcycle.

##### Inheritance

Inheritance, an integral concept in the object-oriented programming paradigm, confers the capability to construct new classes based on existing ones. The cardinal advantage of inheritance resides in the promotion of code reuse. When the requirement arises to create a class with minor variations from an extant one, there is no compulsion to duplicate code. Instead, one can extend the existing class and integrate the additional functionality into the following subclass, which inherits fields and methods from the superclass.

A significant ramification of employing inheritance is the manifestation of identical interfaces in subclasses as their superclass. This necessitates that if a method is declared in the superclass, it cannot be hidden in any subclasses. Furthermore, all abstract methods must be implemented, regardless of their relevancy or applicability to the specific subclass.

In most programming languages, a subclass can extend only a single superclass. Contrastingly, any class holds the ability to implement multiple interfaces simultaneously. However, as mentioned earlier, it is worth noting that if a superclass implements an interface, all of its respective subclasses are also required. This characteristic illuminates the intricate and complex relationships that define and drive object-oriented programming, underscoring the importance of careful design and thoughtful implementation.

<pre class="mermaid">
classDiagram
    class Animal {
        - name: string
        - age: integer
        - weight: double
        + getName(): string
        + getAge(): integer
        + getWeight(): double
        # makeSound(): string
    }

    class Dog { + makeSound(): string }

    class Cat { + makeSound(): string }

    Animal <|-- Dog : extends

    Animal <|-- Cat : extends
</pre>

##### Polymorphism

Polymorphism is a fundamental concept in object-oriented programming. It allows a program to identify the actual class of an object and execute its corresponding implementation, even when the object's type is unclear in the current context. Another way to think about polymorphism is an object's ability to act like a different kind of entity, usually a class it extends or an interface it implements.

Polymorphism also allows the same interface to represent many types or classes, each with its unique implementation. This feature makes it easier to design more flexible and adaptable code. For example, a method designed to work with a superclass or interface can interact smoothly with any of its subclasses or implement classes without needing specific knowledge of their details.

There are two primary ways to implement polymorphism: static or compile-time polymorphism and dynamic or runtime polymorphism. Static polymorphism is usually achieved through function overloading and determined at compile time. On the other hand, dynamic polymorphism relies on virtual methods and is determined at runtime.

Overall, polymorphism plays a crucial role in improving the modularity and readability of code, promoting reusability, and enabling developers to design systems that can adapt to changing requirements. It embodies the essence of abstract thinking in programming, allowing developers to create more extensible and adaptable software.

<pre class="mermaid">
classDiagram
    class FlyBehavior { + fly(): void }

    class Bird { + fly(): void }

    class Airplane { + fly(): void }

    FlyBehavior <|.. Bird: implements

    FlyBehavior <|.. Airplane: implements
</pre>
