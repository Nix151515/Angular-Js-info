
4 pillars

1. Encapsulation: exposing only parts that are needed to the outside of the component - hiding the implementation details.
Improves security, readability + reduce the risk of errors
2. Inheritance: implementation of a "is-a" relationship between objects.
Using the "extends" keyword 

public class Cat extends Animal{
}

The children use existing (parent) classes and augmenting or overridding its methods.
Avoids duplication, reduces complexity.

3. Polymorphism
Allows objects of different classes to perfom actions with the same name using different code.
Allows for the creation of common methods and functions to be used for multiple types of objects.
Ex. : "show information" method can be used to display varied data about objects of the "car", "plane", or "ship" type.

4. Abstraction.
Abstraction is the process of hiding the internal details of an application from the outer world.
Abstraction is used to describe things in simple terms.
We display only the information necessary for the users (data abstraction) and we do not disclose functionality details (process abstraction)
Abstraction can be thought of as an expansion of encapsulation.
Abstraction is typically achieved through abstract classes, interfaces and inheritance.

Abstract classes = These are classes that cannot be instantiated directly but serve as blueprints for other classes.
 They may contain abstract methods.
 Example:

 abstract class Shape {
    // Name of the shape
    String objectName = " "; 

    // Constructor to initialize the name of the shape
    Shape(String name) {
        this.objectName = name;
    }

    // Concrete method to move the shape to a new position
    public void moveTo(int x, int y) {
        System.out.println(this.objectName + " has been moved to x = " + x + " and y = " + y);
    }

    // Abstract method to calculate the area of the shape
    abstract public double area();
 }

Abstract methods = Methods declared but not implemented, that child classes must implement (overriden basically)

Interfaces: a way to achieve abstraction, it defined the behaviour that classes must implement.
Can have only abstract methods and static & final variables

Abstract classes vs interfaces:
Abstract classes can have both abstract and concrete methods, providing a mix of required and optional implementations.
Interfaces only define method signatures (abstract methods), forcing implementing classes to provide their own logic. 



Abstraction vs encapsulation
Abstraction, as we’ve seen pertains to hiding underlying details and implementation in a program.
Encapsulation, on the other hand, describes how abstraction occurs in a program.

Abstraction is a design-level process but encapsulation is an implementation process.

 Encapsulation tells us how exactly you can implement abstraction in the program. Abstraction pertains to only displaying the essential details to the user whereas encapsulation pertains to typing up all the data members and associated member functions into a single abstracted unit.





Definitions:
- Class: user-defined blueprint that describes what a specific object will look like.
Consists of: attributes or member variables and implementation of member functions
- Object:  a single instance of a class, which contains data and methods working on that data

- Superclass: parent class
- Subclass: child class
- Composition: implementation of a "has-a" relationship
public class Person {
    //composition has-a relationship
    private Job job;
}
Achieved by using instance variables of other objects (Person.job)
- Overloading: You keep the same method name but change the number or type of parameters
This allows for other behaviour depending on what the method is called.
Overloading relates to compile-time polymorphism
Overloading can occur when you change the number, type and order of parameters.
Overloading is used to improve the readability of the code
- Overriding:  Allows a subclass or child class to provide a specific implementation of a method
The purpose of this is to change the behaviour of the inherited method
Overriding can only occur when you change the number and type.
Overriding relates to run-time polymorphism
- final: a non-access modifier used for classes, attributes and methods, which makes them non-changeable (impossible to inherit or override).
- super: refers to the superclass of a subclass.
It's used to access members (fields or methods) of the superclass, especially when those members have been overridden in the subclass.
It's also crucial for calling the superclass's constructor from the subclass constructor. 
super.methodName() -> useful when methodName is overriden
super.fieldName
super() -> calls superclass constructor, must be the first statement in the subclass constructor


Inheritance vs composition:
- Inheritance: tighly coupled, composition: loosely coupled
If the parent adds a function that is already part of a child, there will be errors -> go for composition. Use the superclass in the new class and call its methods.
Use inheritance only when you are sure that superclass will not be changed, otherwise go for composition.



SOLID principles:








Good practices:
- Identify the classes needed and their relationships
- Use abstraction to simplify understanding
- 



Resources:
https://career.softserveinc.com/en-us/stories/what-is-object-oriented-programming-oop-explaining-four-major-principles
https://dev.to/m__mdy__m/understanding-abstraction-in-oop-1abp
