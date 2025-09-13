# Design Patterns

A design pattern is a standard, reusable solution to a common problem in software design. Blueprint that tells you how to solve a problem in a clean, flexible, and maintainable way — but it’s not actual code, more like a structured approach.


**WHY =>** Reusability, Maintainability, Communication{Gives developers a common vocabulary (“let’s use Singleton here”)}, Best practices{Encapsulates years of industry experience.}


## Types of Design Patterns (GOF Classification)

The famous Gang of Four (GoF) book groups them into 3 families:

### 1. Creational Patterns (object creation)

Examples:

- Singleton → Only one instance of a class (e.g., DB connection).

- Factory → Creates objects without exposing the creation logic.

- Builder → Step-by-step object construction.

### 2.Structural Patterns (object composition)

Examples:

- Adapter → Makes incompatible interfaces work together.

- Decorator → Dynamically adds behavior to objects (like React HOC).

- Proxy → Placeholder for another object (e.g., lazy loading).

### 3. Behavioral Patterns (object interaction/communication)

Examples:

- Observer → One-to-many dependency (like EventEmitter, Redux subscribers).

- Strategy → Define different algorithms and switch at runtime.

- Command → Encapsulates a request as an object (undo/redo systems)









