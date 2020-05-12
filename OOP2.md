# OOP~~2!
401 Readings for Class 7!

## Inheritance 
  - Inheritance, together with encapsulation and polymorphism, is one of the three primary characteristics of object-oriented programming
  -  Inheritance enables you to create new classes that reuse, extend, and modify the behavior defined in other classes. 
  - For example, if you have a base class Animal, you might have one derived class that is named Mammal and another derived class that is named Reptile. A Mammal is an Animal, and a Reptile is an Animal, but each derived class represents different specializations of the base class.

### Abstract and virtual methods
  - When a base class declares a method as virtual, a derived class can override the method with its own implementation.
  - If a base class declares a member as abstract, that method must be overridden in any non-abstract class that directly inherits from that class.
  - If a derived class is itself abstract, it inherits abstract members without implementing them.

  - You can declare a class as abstract if you want to prevent direct instantiation by using the new operator. 
  - An abstract class can be used only if a new class is derived from it. An abstract class can contain one or more method signatures that themselves are declared as abstract.
  - An abstract class doesn't have to contain abstract members; however, if a class does contain an abstract member, the class itself must be declared as abstract. 
  - erived classes that aren't abstract themselves must provide the implementation for any abstract methods from an abstract base class.

### Interface
  - An interface is a reference type that defines a set of members. 
  - All classes and structs that implement that interface must implement that set of members. 

## Class Members
### Abstract Classes and Class Members
 - Classes can be declared as abstract by putting the keyword abstract before the class definition.

example

    public abstract class A
  {
      // Class members here.
  }

  - An abstract class cannot be instantiated. The purpose of an abstract class is to provide a common definition of a base class that multiple derived classes can share
  - Abstract methods have no implementation, so the method definition is followed by a semicolon instead of a normal method block. 
  -  Derived classes of the abstract class must implement all abstract methods.
  - If a virtual method is declared abstract, it is still virtual to any class inheriting from the abstract class

### Sealed Classes and Class Members
 - Classes can be declared as sealed by putting the keyword sealed before the class definition.  
 - A sealed class cannot be used as a base class
 - ecause they can never be used as a base class, some run-time optimizations can make calling sealed class members slightly faster

## Polymorphism
