
## Prototype

This pattern specifies the kinds of objects to create using a prototypical instance, and creates new objects by copying this prototype. It’s used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects

**Problem**:

- You need to create objects in a way that allows you to specify at runtime which objects to create.
- You need to instantiate dynamically loaded classes.
- This problem arises when objects are created directly within the class that requires and uses those objects. This manner of object creation is inflexible as it commits the class to particular objects at compile-time, preventing the specification of which objects to create at runtime.

**Solution**:

- Define a Prototype object that returns a copy of itself. Then at runtime, create new objects by copying a Prototype object.

This approach provides a way to delegate the instantiation type to the objects themselves, making the system more flexible and extensible. It allows new types of objects to be added without changing the existing code, adhering to the Open/Closed Principle

![[Screenshot (221).png]]

### Without Prototype pattern

```java
abstract class Shape {
    protected String color;
    protected int size;

    // Other methods...
}

class Circle extends Shape {
    private int x, y; // coordinates

    public Circle(String color, int size, int x, int y) {
        this.color = color;
        this.size = size;
        this.x = x;
        this.y = y;
    }

    // Other methods...
}

public class GraphicEditor {
    public static void main(String[] args) {
        Circle circle = new Circle("red", 10, 5, 5);
        Circle circle2 = new Circle(circle.color, circle.size, 10, circle.y); // change coordinates
        Circle circle3 = new Circle(circle.color, circle.size, circle.x, 10); // change coordinates
        // Now we have three circles with the same color and size but different coordinates
    }
}
```


### With Prototype pattern


```java
abstract class Shape implements Cloneable {
    protected String color;
    protected int size;

    public Shape clone() throws CloneNotSupportedException {
        return (Shape) super.clone();
    }

    // Other methods...
}

class Circle extends Shape {
    private int x, y; // coordinates

    public Circle(String color, int size, int x, int y) {
        this.color = color;
        this.size = size;
        this.x = x;
        this.y = y;
    }

    public Circle clone() throws CloneNotSupportedException {
        return (Circle) super.clone();
    }
    
    public void setX(int n){
        this.x = n;
    }

    public void setY(int n){
        this.y = n;
    }
    // Other methods...
}

public class GraphicEditor {
    public static void main(String[] args) throws CloneNotSupportedException {
        Circle circle = new Circle("red", 10, 5, 5);
        Circle circle2 = circle.clone();
        circle2.setX(10); // change coordinates
        Circle circle3 = circle.clone();
        circle3.setY(10); // change coordinates
        // Now we have three circles with the same color and size but different coordinates
    }
}

```

We can also implement without the Cloneable interface

```java
abstract class Shape{
    protected String color;
    protected int size;

    public abstract Shape clone();

    // Other methods...
}

    public Circle clone(){
        return new Circle(this.color, this.size, this.x, this.y);
    }
```


![[Pasted image 20240412145130.png]]


## Singleton


**Problem**:

- It is required to ensure that only a single instance of a class is instantiated.
- It is required to provide easy access to the sole instance of a class.

**Solution**:

- The constructor for the class must be hidden by restricting it’s visibility (for example, by declaring as private).
- A public method must be provided to get a reference to the sole instance of the object (for example, as getInstance().
- Declare the instance of the class as a static field.
- The `getInstance()` method is also need to be static.


 The Singleton pattern is often criticized for introducing a global state into an application, which can make code harder to understand and debug due to hidden dependencies.
 
	 “Global state” in an application refers to data that is accessible from anywhere in the program, regardless of the flow of control. Any part of the program can potentially read from or write to the global state.

This can lead to some issues.
- **Unpredictability and Increased Complexity**
- **Tight Coupling**
- **Concurrency Issues**

The Singleton pattern is indeed an improvement over global variables. It avoids polluting the global namespace and provides a way to encapsulate and control access to the instance. It also allows for lazy initialization, where the instance is created only when it is needed.

![[Screenshot (222).png]]


*eagerly initialized Singleton*

```java
class Logger { // this is the Singleton object  
    // we define a static sharable object as a constant   
    private static  Logger INSTANCE = new Logger();  
    
    private int logCount;  
    private Logger() { // private constructor  
        logCount = 0;  
    }  
    public static Logger getInstance() { // static operation  
        return INSTANCE;  
    }  
    public synchronized void incCount() { logCount++; }  
    public int getCount() { return logCount; }  
  
}

public class LogTester {  
    public static void main(String [] args) {  
// we define a set of Logger object references  
        Logger [] log = new Logger[5];  
        for(int i=0; i<5; i++) {  
// a new Logger object is assigned to each reference  
            log[i] = Logger.getInstance();  
            log[i].incCount();  
// although we increase the logCount individually for each object it can be seen that only a single instance of the Logger object exists.  
            System.out.println("Log Count is "+log[i].getCount());  
        }  
    }  
}
```


*Lazy Initialized Singleton*

```java
public class Logger {
    private static Logger instance;
    private int logCount;

    private Logger() {
        logCount = 0;
    }

    public static synchronized Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }

    public void incCount() {
        logCount++;
    }

    public int getCount() {
        return logCount;
    }
}
```


*Double-Checked Locking Singleton*

```java
public class Logger {
    private volatile static Logger instance;
    private int logCount;

    private Logger() {
        logCount = 0;
    }

    public static Logger getInstance() {
        if (instance == null) {
            synchronized (Logger.class) {
                if (instance == null) {
                    instance = new Logger();
                }
            }
        }
        return instance;
    }

    public void incCount() {
        logCount++;
    }

    public int getCount() {
        return logCount;
    }
}
```


![[Pasted image 20240412145102.png]]


## Factory Method


**Problem:** 

1. **Compile-time object creation**: In traditional programming, we usually know at compile time what class of object we need to create. But in some cases, we might not know the exact class of the object that will be created until runtime. This is where the Factory Method pattern comes in.
2. **Subclass instantiation**: Sometimes, we want to create an instance of a class, but we want to allow subclasses to redefine which class to instantiate. This is useful when a class cannot anticipate the class of objects it needs to create.
3. **Defer instantiation to subclasses**: In some cases, a class prefers to defer instantiation of an object to its subclasses. This is useful when a class is designed to be extended by subclasses and it cannot or does not want to decide what objects to create.

**Solution:**

1. **Factory Method in an interface**: We define a method (the “factory method”) in an interface. This method is responsible for creating objects. The exact type of object it creates is not specified in the interface; instead, it’s determined by the class that implements the interface.
2. **Factory Method in a base class**: Alternatively, we can define the factory method in a base class. This method is then implemented in derived classes. The base class only needs to know that the objects it creates adhere to a certain interface or base class, not their concrete classes.
3. **Avoid direct constructor calls**: By using the Factory Method pattern, we avoid calling a constructor directly to create an object. Instead, we call the factory method, which creates the object for us. This decouples the process of creating an object from the class that uses the object, making the code more flexible and easier to maintain.

By using the Factory Method pattern, we can encapsulate the complexities of object creation, reduce code duplication, and increase the level of abstraction. This makes the code more flexible, easier to maintain, and easier to extend with new types of objects.

![[Screenshot (225).png]]


```java
// This is a Generic/Abstract Pizza class.
abstract class Pizza {
    public abstract void bake();
}

// These are Concrete Pizza classes.
// They override methods to specialize themselves.
class CheesePizza extends Pizza {
    @Override
    public void bake() { System.out.println("Baking a Cheese Pizza");     }
}

class PepperoniPizza extends Pizza {
    @Override
    public void bake() { System.out.println("Baking a Pepperoni Pizza"); }
}

// This is a Generic/Abstract PizzaStore class.
// It will bake a pizza using generic pizza classes.
// This code at compile time is generic and uses the Factory Method makePizza().
abstract class PizzaStore {
    private Pizza pizza;
    // Factory Method
    public abstract Pizza makePizza();
    public void orderPizza() {
        System.out.println("Ordering a Pizza");
        pizza = makePizza();
        pizza.bake();
    }
}

// These are Concrete PizzaStore classes.
// They override the factory method, makePizza() to specialize themselves.
class CheesePizzaStore extends PizzaStore {
    @Override
    public Pizza makePizza() {
        System.out.println("Making a Cheese Pizza");
        return new CheesePizza();
    }
}

class PepperoniPizzaStore extends PizzaStore {
    @Override
    public Pizza makePizza() {
        System.out.println("Making a Pepperoni Pizza");
        return new PepperoniPizza();
    }
}

// This test code shows that the correct pizza is baked in the specific store.
// This decision on which pizza to bake is deferred to a subclass and can be decided at runtime.
public class PizzaTestDrive {
    public static void main(String [] args) {
        PizzaStore cheeseStore = new CheesePizzaStore();
        cheeseStore.orderPizza(); // Outputs: Ordering a Pizza, Making a Cheese Pizza, Baking a Cheese Pizza

        PizzaStore pepperoniStore = new PepperoniPizzaStore();
        pepperoniStore.orderPizza(); // Outputs: Ordering a Pizza, Making a Pepperoni Pizza, Baking a Pepperoni Pizza
    }
}
```


![[Pasted image 20240412145032.png]]


## Abstract Factory 


The Abstract Factory design pattern is a creational pattern that provides a way to create families of related or dependent objects without specifying their concrete classes. It’s like a factory of factories, and it’s often used when you have multiple related products that need to be created together.


![[Screenshot (227).png]]

Improvement to previous pizza example using Abstract Factory

```java
// These are the Pizza, Box and Cutter interfaces.
interface Pizza {
    void bake();
}

interface Box {
    void pack();
}

interface Cutter {
    void cut();
}

// These are Concrete Pizza, Box and Cutter classes for Cheese Pizza.
class CheesePizza implements Pizza {
    @Override
    public void bake() { System.out.println("Baking a Cheese Pizza"); }
}

class CheesePizzaBox implements Box {
    @Override
    public void pack() { System.out.println("Packing in a Cheese Pizza Box"); }
}

class CheesePizzaCutter implements Cutter {
    @Override
    public void cut() { System.out.println("Cutting with a Cheese Pizza Cutter"); }
}

// These are Concrete Pizza, Box and Cutter classes for Pepperoni Pizza.
class PepperoniPizza implements Pizza {
    @Override
    public void bake() { System.out.println("Baking a Pepperoni Pizza"); }
}

class PepperoniPizzaBox implements Box {
    @Override
    public void pack() { System.out.println("Packing in a Pepperoni Pizza Box"); }
}

class PepperoniPizzaCutter implements Cutter {
    @Override
    public void cut() { System.out.println("Cutting with a Pepperoni Pizza Cutter"); }
}

// This is a Generic/Abstract PizzaFactory interface.
// It will create a family of pizza-related products.
interface PizzaFactory {
    Pizza makePizza();
    Box makeBox();
    Cutter makeCutter();
}

// These are Concrete PizzaFactory classes.
// They override the factory methods to specialize themselves.
class CheesePizzaFactory implements PizzaFactory {
    @Override
    public Pizza makePizza() { return new CheesePizza(); }
    @Override
    public Box makeBox() { return new CheesePizzaBox(); }
    @Override
    public Cutter makeCutter() { return new CheesePizzaCutter(); }
}

class PepperoniPizzaFactory implements PizzaFactory {
    @Override
    public Pizza makePizza() { return new PepperoniPizza(); }
    @Override
    public Box makeBox() { return new PepperoniPizzaBox(); }
    @Override
    public Cutter makeCutter() { return new PepperoniPizzaCutter(); }
}

// This test code shows that the correct pizza, box, and cutter are created in the specific store.
// This decision on which products to create is deferred to a subclass and can be decided at runtime.
public class PizzaTestDrive {
    public static void main(String [] args) {
        PizzaFactory cheeseFactory = new CheesePizzaFactory();
        cheeseFactory.makePizza().bake();
        cheeseFactory.makeBox().pack();
        cheeseFactory.makeCutter().cut();

        PizzaFactory pepperoniFactory = new PepperoniPizzaFactory();
        pepperoniFactory.makePizza().bake();
        pepperoniFactory.makeBox().pack();
        pepperoniFactory.makeCutter().cut();
    }
}
```


In here

1. **Abstract Products**: These are represented by the `Pizza`, `Box`, and `Cutter` interfaces. They declare the operations that all concrete products must implement. In this case, each product type (`Pizza`, `Box`, `Cutter`) has one operation (`bake()`, `pack()`, `cut()`).
    
2. **Concrete Products**: These are the `CheesePizza`, `PepperoniPizza`, `CheesePizzaBox`, `PepperoniPizzaBox`, `CheesePizzaCutter`, and `PepperoniPizzaCutter` classes. They are the actual objects created by the concrete factories and they implement the operations declared in the abstract product interfaces.
    
3. **Abstract Factory**: This is represented by the `PizzaFactory` interface. It declares a set of methods for creating a family of related products (`makePizza()`, `makeBox()`, `makeCutter()`), each of which returns an abstract product.
    
4. **Concrete Factories**: These are the `CheesePizzaFactory` and `PepperoniPizzaFactory` classes. They implement the methods declared in the `PizzaFactory` interface to produce concrete products. Each concrete factory corresponds to a specific variant of products and creates a family of related products. For example, `CheesePizzaFactory` creates `CheesePizza`, `CheesePizzaBox`, and `CheesePizzaCutter`, while `PepperoniPizzaFactory` creates `PepperoniPizza`, `PepperoniPizzaBox`, and `PepperoniPizzaCutter`.


![[Pasted image 20240412145006.png]]



## Builder design pattern


The Builder design pattern is a creational pattern that provides a flexible solution for constructing complex objects, especially when these objects involve a lot of steps to construct. It separates the construction of an object from its representation, allowing the same construction process to create different representations.

Here are the main components of the Builder pattern:

1. **Builder**: This is an interface that specifies methods for creating parts of a complex object.
2. **Concrete Builder**: This is a class that implements the Builder interface, provides an implementation for each of the operations, and keeps track of the product it creates.
3. **Director**: This is a class that constructs an object using the Builder interface.
4. **Product**: This is the complex object that is being built.

**Advantages**:

- **Allows for the varying of an object’s internal representation**: The Builder pattern can construct different representations of a complex object, which is useful when you need to create different flavors of an object while avoiding constructor pollution with many parameters.
- **Encapsulates code for construction and representation**: The Builder pattern encapsulates the construction logic in separate builder classes, which improves modularity and makes the code easier to read and maintain.
- **Provides control over steps of construction process**: The Builder pattern allows you to control the order and the process by which an object is constructed. You can defer part of the construction to different builder subclasses that implement the same interface.

**Disadvantages**:

- **Requires creating a separate ConcreteBuilder class for each different type of object**: For each different type of product, a concrete builder class must be created. This increases the number of classes.
- **Requires the builder classes to be mutable**: Builder classes are typically designed to be mutable as they progressively build up a complex object. This could potentially lead to issues related to mutability.
- **Data members of class are not guaranteed to be initialized**: Since the construction process is controlled by the director, there’s a risk that the product might not be fully initialized if the director forgets to call some of the build steps.
- **Dependency injection may be less supported**: The Builder pattern might complicate the code and make it harder to apply dependency injection, since it requires creating multiple new builder classes.


![[Screenshot (228).png]]


```java
// The 'Product' class
class Pizza {
    private String dough = "";
    private String sauce = "";
    private String topping = "";
    
    public void setDough(String dough) { 
        this.dough = dough; 
        System.out.println("Dough set to: " + dough);
    }
    public void setSauce(String sauce) { 
        this.sauce = sauce; 
        System.out.println("Sauce set to: " + sauce);
    }
    public void setTopping(String topping) { 
        this.topping = topping; 
        System.out.println("Topping set to: " + topping);
    }
}

// The 'Builder' abstract class
abstract class PizzaBuilder {
    protected Pizza pizza;
    
    public Pizza getPizza() { return pizza; }
    public void createNewPizzaProduct() { 
        pizza = new Pizza(); 
        System.out.println("New pizza product created");
    }
    
    public abstract void buildDough();
    public abstract void buildSauce();
    public abstract void buildTopping();
}

// 'ConcreteBuilder1'
class HawaiianPizzaBuilder extends PizzaBuilder {
    public void buildDough() { 
        pizza.setDough("cross"); 
        System.out.println("HawaiianPizzaBuilder is building dough");
    }
    public void buildSauce() { 
        pizza.setSauce("mild"); 
        System.out.println("HawaiianPizzaBuilder is building sauce");
    }
    public void buildTopping() { 
        pizza.setTopping("ham+pineapple"); 
        System.out.println("HawaiianPizzaBuilder is building topping");
    }
}

// 'ConcreteBuilder2'
class SpicyPizzaBuilder extends PizzaBuilder {
    public void buildDough() { 
        pizza.setDough("pan baked"); 
        System.out.println("SpicyPizzaBuilder is building dough");
    }
    public void buildSauce() { 
        pizza.setSauce("hot"); 
        System.out.println("SpicyPizzaBuilder is building sauce");
    }
    public void buildTopping() { 
        pizza.setTopping("pepperoni+salami"); 
        System.out.println("SpicyPizzaBuilder is building topping");
    }
}

// 'Director' class
class Cook {
    private PizzaBuilder pizzaBuilder;
    
    public void setPizzaBuilder(PizzaBuilder pb) { 
        pizzaBuilder = pb; 
        System.out.println("PizzaBuilder set in Cook");
    }
    public Pizza getPizza() { return pizzaBuilder.getPizza(); }
    
    public void constructPizza() {
        pizzaBuilder.createNewPizzaProduct();
        pizzaBuilder.buildDough();
        pizzaBuilder.buildSauce();
        pizzaBuilder.buildTopping();
    }
}

// A given type of pizza being constructed.
public class BuilderExample {
    public static void main(String[] args) {
        Cook cook = new Cook();
        PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
        PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();
        
        cook.setPizzaBuilder(hawaiianPizzaBuilder);
        cook.constructPizza();
        Pizza pizza1 = cook.getPizza();
        System.out.println("Pizza Built: " + pizza1);
        
        cook.setPizzaBuilder(spicyPizzaBuilder);
        cook.constructPizza();
        Pizza pizza2 = cook.getPizza();
        System.out.println("Pizza Built: " + pizza2);        
    }
}
```


![[Pasted image 20240413002818.png]]

[[Design Patterns]]