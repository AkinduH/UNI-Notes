> [!NOTE]
> What is the difference between stack and heap memory? How are 
> they managed? 
> 
> Stack is used for storing primitive types (numbers, boolean and 
> character) and variables that store references to objects in the heap. 
> Variables stored in the stack are immediately cleared when they go out 
> of scope (eg when a method finishes execution). Objects stored in the 
> heap get removed later on when they’re no longer references. This is 
> done by Java’s garbage collector. 

> [!NOTE]
> What is encapsulation? 
> 
> Encapsulation is the first principle of object-oriented programming. 
> It suggests that we should bundle the data and operations on the data 
> inside a single unit (class). 

> [!NOTE]
> What is abstraction? 
> 
> Abstraction is the second principle of object-oriented programming. 
> It suggests that we should reduce complexity by hiding the unnecessary 
> implementation details. 


> [!NOTE]
> What is coupling? 
> 
>  Coupling represents the level of dependency between software 
> entities (eg classes). The more our classes are dependent on each other, 
> the harder it is to change them. Changing one class may result in several 
> cascading and breaking changes. 

> [!NOTE]
> What are constructors? 
> 
> Constructors are called when we instantiate our class. We use them 
> to initialize our objects. Initialization means putting an object into an 
> early or initial state (eg giving it initial values). 

> [!NOTE]
> What is method overloading? 
> 
> Method overloading means declaring a method with the same name 
> but with different signatures. The number, type and order of its 
> parameters will be different. 

> [!NOTE]
> What are static methods? 
> 
> Since _static_ variables belong to a class, we can access them directly using the class name. So, we don’t need any object reference.
> We can only declare _static_ variables at the class level.
> We can access _static_ fields without object initialization.
> static variables are stored in the heap memory.
> 
> ```
> public class Car {
>     private String name;
>     private String engine;
>     
>     public static int numberOfCars;
>     
>     public Car(String name, String engine) {
>         this.name = name;
>         this.engine = engine;
>         numberOfCars++;
>     }
> 
>     // getters and setters
> }
> ```


> [!NOTE]
> What is a default constructor?
> 
> A constructor without any parameters. If we don’t create it, the Java 
> compiler will automatically add one to our classes.
>  constructors don’t have a return type, not even void. 


> [!NOTE]
> What is the super keyword? 
> 
> The super keyword is a reference to the base or parent class. We can 
> use it to access the members (fields and methods) or call the 
> constructors of the base class. In contrast, the this keyword returns a 
> reference to the current object.


> [!NOTE]
> How accessible is a field or method if it’s declared without an access 
> modifier? 
> 
> Member will be public in the package but private outside of 
> the package. 
> 
> > [!NOTE]
> >  Members marked with protected or private access modifiers are 
> > only accessible inside of a class. Protected members are inherited and 
> > are accessible by child (derived) classes. Private members are not. 


> [!NOTE]
> **Upcasting** is the process of converting a subclass reference into a superclass reference.
> 
> ```java
>         Dog dog = new Dog();
>         Animal animal = dog; // Upcasting
>         animal.eat();
> ```
> 
> 
> **Downcasting**, on the other hand, is the process of converting a superclass reference back into a subclass reference.
> 
> ```java
>         Animal animal = new Dog(); // Upcasting
>         Dog dog = (Dog) animal; // Downcasting
>         dog.bark();
> ```
> It’s always important to ensure that the actual object is of the target class or a subclass of the target class before downcasting.


> [!NOTE]
> Four principles of object-oriented programming
> 
> - Encapsulation: bundling the data and operations on the data inside 
> a single unit (class). 
> 
> - Abstraction: reducing complexity by hiding unnecessary details 
> (metaphor: the implementation detail of a remote control is hidden 
> from us. We only work with its public interface.) 
> 
> - Inheritance: a mechanism for reusing code. 
> 
> - Polymorphism: a mechanism that allows an object to take many 
> forms and behave differently. This will help us build extensible 
> applications


> [!NOTE]
> Can we have an abstract class without any abstract methods? 
> 
> Yes! An abstract class does not need abstract methods. But if we 
> mark a method as abstract, we should mark the class as abstract as well.

> [!NOTE]
>  When do we use final classes?
> 
> Final classes cannot be inherited. We use them when we’ve made 
> certain assumptions about a class and we want to prevent other classes 
> extending our class and break those assumptions. 

