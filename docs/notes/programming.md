# Programming

## OOP
**Object Oriented programming** (OOP) is a programming paradigm that relies on the concept of classes and objects. It is used to structure a software program into simple, reusable pieces of code blueprints (usually called classes), which are used to create individual instances of objects.

**Benefits**

- OOP models complex things as reproducible, simple structures
- Reusable, OOP objects can be used across programs
- Allows for class-specific behavior through polymorphism
- Easier to debug, classes often contain all applicable information to them
- Secure, protects information through encapsulation



## Principles

### Inheritance
Inheritance allows classes to inherit features of other classes.  
Inheritance supports reusability.

### Encapsulation
Encapsulation means containing all important information inside an object, and only exposing selected information to the outside world.

**Benefits**

- **Adds security** - Only public methods and attributes are accessible from the outside
- **Protects against common mistakes** - Only public fields & methods accessible, so developers don’t accidentally change something dangerous
- **Protects IP** - Code is hidden in a class, only public methods are accessible by the outside developers
- **Supportable** - Most code undergoes updates and improvements
- **Hides complexity** - No one can see what’s behind the object’s curtain!

### Abstraction
Abstraction means that the user interacts with only selected attributes and methods of an object.  
Abstraction is using simple classes to represent complexity.

**Benefits**

- Simple, high level user interfaces
- Complex code is hidden
- Security
- Easier software maintenance
- Code updates rarely change abstraction

### Polymorphism
Polymorphism means designing objects to share behaviors. Using inheritance, objects can override shared parent behaviors, with specific child behaviors. Polymorphism allows the same method to execute different behaviors in two ways: method overriding and method overloading.

**Method Overriding**  
In method overriding, a child class can provide a different implementation than its parent class.

**Method Overloading**  
Methods or functions may have the same name, but a different number of parameters passed into the method call. Different results may occur depending on the number of parameters passed in.

**Benefits**

- Objects of different types can be passed through the same interface
