
## Adapter Pattern


The Adapter pattern is a structural design pattern that allows objects with incompatible interfaces to collaborate. It’s often used when you’re integrating new components into an existing system and you don’t want to refactor all the code to match the new component’s interface.

- **Problem**: You have a class (`Adaptee`) with a useful functionality, but its interface isn’t compatible with the rest of your code. The `Adaptee` class provides some useful behavior, but its interface is incompatible with the existing client code. The client cannot use this class directly because the interfaces do not match.
    
- **Solution**: Create a new class (`Adapter`) that contains an instance of the `Adaptee` class. This `Adapter` class should have a method that follows the expected interface. This method should then translate the method call to the `Adaptee`’s method. The `Adapter` class translates the interface of the `Adaptee` class into an interface that the client code can use.

- **Key Idea**: The main idea behind the Adapter pattern is to enable classes with incompatible interfaces to work together. It’s about creating an intermediary abstraction that translates, or maps, the old component’s interface to the new system’s interface. The Adapter acts as a wrapper between two objects. It catches calls for one object and transforms them to format and interface recognizable by the second object.

![[Screenshot (229).png]]

```java
// Existing class (Adaptee)
class Temperature {
    private double temperature;

    public Temperature(double temperature) {
        this.temperature = temperature;
    }

    public double getTemperature() {
        return this.temperature;
    }
}

// Target interface
interface TemperatureInCelsius {
    double getTemperatureInCelsius();
}

// Adapter class
class TemperatureAdapter implements TemperatureInCelsius {
    private Temperature temperature;

    public TemperatureAdapter(Temperature temperature) {
        this.temperature = temperature;
    }

    @Override
    public double getTemperatureInCelsius() {
        return (temperature.getTemperature() - 32) * 5/9;
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        Temperature temperature = new Temperature(100); // Fahrenheit
        TemperatureInCelsius adapter = new TemperatureAdapter(temperature);

        System.out.println("Temperature in Celsius: " + adapter.getTemperatureInCelsius());
    }
}
```

In this example, the client code can use the `Temperature` class via the `TemperatureAdapter`, even though their interfaces are not compatible. The Adapter makes them compatible by providing the interface that the client expects while using the functionality of the `Temperature` class.


![[Pasted image 20240422134458.png]]


## Bridge Pattern

This pattern is particularly useful when you want to avoid a permanent binding between an abstraction and its implementation. This might be necessary, for example, when the implementation must be selected or switched at run-time.

- **Problem**: There’s a need to define an abstraction and its implementation in a way that allows them to be extended independently. Also, it’s required to avoid a compile-time binding between an abstraction and its implementation, so that an implementation can be selected at run-time.
    
- **Solution**: The Bridge pattern suggests moving the implementation into a separate class hierarchy. Then, linking the original classes with the new hierarchy through a bridge interface (also known as the Abstraction). More specifically, you create two parallel or separate hierarchies - one for abstractions or interfaces, and another for implementations. The abstraction object controls or interacts with the different implementation objects.

Bridge pattern is very powerful when you need to support a variety of implementations or switch between them at run-time.

- When we use **subclassing** to implement an abstract class in different ways, the implementation is indeed bound to the abstraction at compile-time. This means we can’t change the implementation at run-time.
    
- However, the **Bridge** design pattern allows us to avoid this limitation. It enables us to configure an Abstraction with an Implementor object at run-time. This means we can switch out the implementation dynamically while the program is running.

It’s important not to confuse the Bridge pattern with the Adapter pattern. While both patterns are about structuring classes and objects, they have different purposes. The Adapter pattern allows an existing class to be used through a different interface without changing its source code, effectively making old classes work with new systems. On the other hand, the Bridge pattern is about separating an abstraction from its implementation so that they can vary independently.

The Bridge pattern uses **encapsulation** (hiding the details of implementation), **aggregation** (using objects of other classes as members), and **inheritance** (creating subclass hierarchies) to separate responsibilities into different classes.


![[Screenshot (232).png]]

```java
// Implementor
interface SoundBehavior {
    void makeSound();
}

// Concrete Implementors
class WoofBehavior implements SoundBehavior {
    public void makeSound() {
        System.out.println("Woof!");
    }
}

class MeowBehavior implements SoundBehavior {
    public void makeSound() {
        System.out.println("Meow!");
    }
}

// Abstraction
abstract class Animal {
    protected SoundBehavior soundBehavior;

    public Animal(SoundBehavior soundBehavior) {
        this.soundBehavior = soundBehavior;
    }

    public void performSound() {
        soundBehavior.makeSound();
    }

    // other methods...
}

// Refined Abstractions
class Dog extends Animal {
    public Dog(SoundBehavior soundBehavior) {
        super(soundBehavior);
    }

    // other methods...
}

class Cat extends Animal {
    public Cat(SoundBehavior soundBehavior) {
        super(soundBehavior);
    }

    // other methods...
}

// Client code
public class Main {
    public static void main(String[] args) {
        Animal myPet = new Dog(new WoofBehavior());
        myPet.performSound();  // Outputs: "Woof!"

        myPet = new Cat(new MeowBehavior());
        myPet.performSound();  // Outputs: "Meow!"
    }
}
```

In this example, the `Animal` class (the **Abstraction**) contains a reference to the `SoundBehavior` interface (the **Implementor**). The `Dog` and `Cat` classes (the **Refined Abstractions**) inherit from `Animal` and can be instantiated with any class that implements `SoundBehavior`.
This allows you to change the sound behavior of `myPet` at run-time by assigning it a new `Animal` object with a different `SoundBehavior` implementation.

![[Pasted image 20240424111518.png]]



## Composite Pattern

- The problem is about representing a **hierarchy** of entities where each entity could be a full entity (composed of other entities) or a part entity (which is not further divisible). This is a common scenario in many real-world systems. For example, in a file system, a directory can contain other directories or files. Here, the directory is a full entity, and the file is a part entity.

- The solution to this problem is the **Composite Design Pattern**. This pattern allows you to compose objects into tree structures and then work with these structures as if they were individual objects. The first step in this pattern is to define a unified **Component interface** that both part (Leaf) objects and whole (Composite) objects implement. This interface defines the operations that are common to both simple and complex entities.


**Partitioning Design Pattern**: The Composite pattern is a type of partitioning design pattern. It helps in partitioning or dividing the object structure into simple and composite objects which can be treated in the same way.

**Part-Whole Hierarchies**: The intent of a composite is to compose objects into tree structures to represent part-whole hierarchies. This means it allows you to build complex objects by recursively composing them from simpler ones.

**When to Use Composite**: The Composite pattern should be used when clients should ignore the difference between compositions of objects (Composites) and individual objects (Leaves). If the client code needs to treat composites and leaves differently, then the Composite pattern may not be the best choice.

![[Screenshot (252).png]]

```java
// This is the abstract class for use by Clients  
abstract class Plant {  
    private String id;  
    Plant(String id) {  
        this.id = id;  
    }
    public String getId() {  
        return id;  
    }  
    abstract public void report();  
    abstract public boolean install(Plant p);  
}  
// This is the leaf node of the plant hierarchy tree  
class LeafPlant extends Plant {  
    LeafPlant(String id) {  
        super(id);  
    }  
    public void report() {  
        System.out.println("Reporting status of "+getId());  
    }  
    public boolean install(Plant p) {  
        System.out.println("Leaf should never call install()");  
        return false;  
    } }  
// This is a Plant node consisting of several sub plant nodes  
class CompositePlant extends Plant {  
    private int spcount;  
    private Plant [] splist;  
    private int spptr;  
    CompositePlant(String id, int spcount) {  
        super(id);  
        this.spcount = spcount;  
        splist = new Plant[spcount];  
        spptr = 0; }  
    // continued ...  
  
    public boolean install(Plant subplant) {  
        if(spptr<spcount) {  
            splist[spptr++] = subplant;  
            return true;  
        }  
        else  
            return false;  
    }  
    public void report() {  
        for(int i=0; i<spptr; i++)  
            splist[i].report();  
        System.out.println("Reporting status of "+getId());  
    } }  
// Example binary tree with 3 levels and 4 leaf nodes  
public class PlantStatusMonitor {  
    public static void main(String [] args) {  
        Plant p1 = new LeafPlant("P1");  
        Plant p2 = new LeafPlant("P2");  
        Plant p3 = new LeafPlant("P3");  
        Plant p4 = new LeafPlant("P4");  
        Plant p5 = new CompositePlant("P5",2);  
        p5.install(p1); p5.install(p2);  
        Plant p6 = new CompositePlant("P6",2);  
        p6.install(p3); p6.install(p4);  
        Plant p7 = new CompositePlant("P7",2);  
        p7.install(p5);  p7.install(p6);  
        p7.report();  
    }  
}
```


![[Pasted image 20240501134447.png]]


## Decorator Pattern

- The need is to add and remove responsibilities from an object dynamically at runtime. This is a common requirement in many real-world applications where an object’s behavior needs to be extended or modified based on certain conditions. Subclassing is not a flexible solution for extending functionality because it’s bound at compile-time and cannot be changed at runtime.

- The solution is to use the Decorator Design Pattern. This involves defining Decorator objects that implement the same interface as the object they are supposed to extend (the Component). Decorators are flexible because you can stack them by wrapping a Decorator with another Decorator, each adding its own behavior.

**Dynamic Behavior Modification**: The Decorator pattern allows behavior to be added to or removed from an individual object dynamically at runtime.

**Single Responsibility Principle**: The Decorator pattern is useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern.

**Similarity to Chain of Responsibility**: The Decorator pattern is structurally nearly identical to the Chain of Responsibility pattern. The key difference is that in a Chain of Responsibility, exactly one class in the chain handles the request, while in the Decorator pattern, all decorators and the wrapped object handle the request.

![[Screenshot (253).png]]


```java
// The Component interface
interface Sandwich {
    double getPrice();
    String getIngredients();
}

// Concrete Component
class BasicSandwich implements Sandwich {
    public double getPrice() {
        return 3.00;
    }

    public String getIngredients() {
        return "Bread";
    }
}

// Base Decorator
abstract class SandwichDecorator implements Sandwich {
    protected final Sandwich decoratedSandwich;

    public SandwichDecorator(Sandwich s) {
        this.decoratedSandwich = s;
    }

    public double getPrice() {
        return decoratedSandwich.getPrice();
    }

    public String getIngredients() {
        return decoratedSandwich.getIngredients();
    }
}

// Concrete Decorators
class WithCheese extends SandwichDecorator {
    public WithCheese(Sandwich s) {
        super(s);
    }

    public double getPrice() {
        return super.getPrice() + 0.50;
    }

    public String getIngredients() {
        return super.getIngredients() + ", Cheese";
    }
}

class WithHam extends SandwichDecorator {
    public WithHam(Sandwich s) {
        super(s);
    }

    public double getPrice() {
        return super.getPrice() + 1.00;
    }

    public String getIngredients() {
        return super.getIngredients() + ", Ham";
    }
}

public class SandwichMaker {
    public static void main(String[] args) {
        // Create a new sandwich and decorate it with cheese and ham
        Sandwich sandwich1 = new WithCheese(new WithHam(new BasicSandwich()));
        // Print out the ingredients and total cost of the sandwich
        System.out.println(sandwich1.getIngredients() + " costs $" + sandwich1.getPrice());
        
        Sandwich sandwich2 = new WithCheese(new BasicSandwich());
        // Print out the ingredients and total cost of the sandwich
        System.out.println(sandwich2.getIngredients() + " costs $" + sandwich2.getPrice());       
        
        
    }
}
```

![[Pasted image 20240501160711.png]]


## Façade Pattern


1. **Problem**:
    
    - A complex subsystem can have many different interfaces, making it difficult to use. Clients that directly interact with these interfaces can become tightly coupled to the subsystem, making the client code hard to implement, change, test, and reuse.
    - The client has a high dependency on the subsystem due to direct references to its objects. This results in tight coupling, which is generally undesirable because it reduces flexibility and reusability.
    
2. **Solution**:
    
    - The solution is to introduce a Facade object that provides a simplified interface to the complex subsystem. The Facade encapsulates the complexities of the subsystem and exposes a simple interface for clients to use.
    - This reduces the dependencies of outside code on the inner workings of the subsystem, thereby making the client code easier to manage.
    - The Facade does not prevent the client from accessing the subsystem directly if it needs to. It just provides a simpler interface for common tasks.
    - Facade doesn’t do all the work itself but delegates most of the work to the subsystem.


![[Screenshot (256).png]]

```java
class CPU {
    public void freeze() { ... }
    public void jump(long position) { ... }
    public void execute() { ... }
}

class Memory {
    public void load(long position, byte[] data) { ... }
}

class HardDrive {
    public byte[] read(long lba, int size) { ... }
}
```

```java
class ComputerFacade {
    private final CPU processor;
    private final Memory ram;
    private final HardDrive hd;

    public ComputerFacade() {
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() {
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }
}
```

```java
class User {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();
        computer.start();
    }
}
```

In this example, `ComputerFacade` is the facade that simplifies the interface of the `CPU`, `Memory`, and `HardDrive` classes. The user doesn’t need to understand how these components work internally; they just need to interact with the `ComputerFacade`

![[Pasted image 20240503204548.png]]

## Flyweight Pattern

1. **Problem**:
    - The need is to support a large number of objects efficiently. Creating a large number of similar objects can consume a lot of memory and slow down your application.
    - At the same time, it’s required to avoid creating a large number of objects to save system resources.

2. **Solution**:
    
    - The solution is to use the Flyweight Design Pattern. This involves creating a `Flyweight` object that stores the shared state (intrinsic state) that’s common to many objects. This state is invariant (it doesn’t change across instances of a Flyweight).
    - The Flyweight object provides an interface through which the variant state (extrinsic state) can be passed in. This is the state that can vary across different instances of a Flyweight.

By separating the intrinsic and extrinsic state, the Flyweight pattern allows for efficient sharing of objects(context independent) that have some shared state (intrinsic), while still allowing individual objects(context-dependent) to have their own unique state (extrinsic). They cannot be shared and must be passed in. This can significantly reduce memory usage and improve performance when dealing with large numbers of objects.

![[Screenshot (257).png]]

```java
import java.util.HashMap;
import java.util.Map;

// The Flyweight class
class Character {
    private char character;
    private String font;
    private int size;
    private String color;

    public Character(char character, String font, int size, String color) {
        this.character = character;
        this.font = font;
        this.size = size;
        this.color = color;
    }

    public void print() {
        System.out.println("Printing character " + character + " in font " + font + " with size " + size + " and color " + color);
    }
}

// The Flyweight Factory
class CharacterFactory {
    private static Map<String, Character> characters = new HashMap<>();

    public static Character getCharacter(char character, String font, int size, String color) {
        String key = "" + character + font + size + color;
        if (!characters.containsKey(key)) {
            characters.put(key, new Character(character, font, size, color));
        }
        return characters.get(key);
    }
}

// The Client class
class TextFormatter {
    public static void main(String[] args) {
        CharacterFactory factory = new CharacterFactory();

        // Use factory to get flyweight objects
        Character a1 = factory.getCharacter('A', "Times New Roman", 12, "red");
        Character a2 = factory.getCharacter('A', "Times New Roman", 12, "red");

        // a1 and a2 are the same object
        System.out.println(a1 == a2);  // prints "true"

        a1.print();  // prints "Printing character A in font Times New Roman with size 12 and color red"
    }
}

```

![[Pasted image 20240506172740.png]]


## Proxy Pattern

1. **Problem:**
    
    - **Control Access to an Object:** Sometimes, you may want to control how and when clients access a particular object. This could be because the object is resource-intensive, and you want to manage its usage, or you might need to secure it by adding an authentication layer.
    - **Provide Additional Functionality:** You might want to add some functionality that is not part of the object’s main responsibility but is related to its usage, like logging, caching, etc.

2. **Solution:**
    
    - **Define a Separate Proxy Object:** The Proxy Design Pattern suggests creating a new proxy class with the same interface as the original service object. When a proxy receives a request, it passes the request to the service object.
    - **Substitute for Another Object (Subject):** The proxy often creates and controls the lifecycle of the service object. Clients can only interact with the service object through the proxy.
    - **Implements Additional Functionality:** The proxy can perform additional actions like accessing the service object only if the client’s credentials are correct, keeping track of the requests, logging the history, etc.


**Substitute for a Subject:** A Proxy must implement the same interface as the Subject. This is the principle of **interface compatibility**.

**Sensitive Access Control Management:** A Proxy can also add a layer of security to the original object by protecting it from the outer world.

![[Screenshot (260).png]]

```java
// The 'Subject' interface
public interface Internet {
    void connectTo(String serverHost) throws Exception;
}

// The 'RealSubject' class
public class RealInternet implements Internet {
    @Override
    public void connectTo(String serverHost) {
        System.out.println("Connecting to "+ serverHost);
    }
}

// The 'Proxy' class
public class ProxyInternet implements Internet {
    private Internet internet = new RealInternet();
    private static List<String> bannedSites;

    static {
        bannedSites = new ArrayList<String>();
        bannedSites.add("abc.com");
        bannedSites.add("def.com");
        bannedSites.add("ijk.com");
        bannedSites.add("lnm.com");
    }

    @Override
    public void connectTo(String serverHost) throws Exception {
        if(bannedSites.contains(serverHost.toLowerCase())) {
            throw new Exception("Access Denied");
        }
        internet.connectTo(serverHost);
    }
}

// In the main method
public static void main (String[] args) {
    Internet internet = new ProxyInternet();
    try {
        internet.connectTo("google.com");
        internet.connectTo("abc.com");
    } catch (Exception e) {
        System.out.println(e.getMessage());
    }
}

```

![[Pasted image 20240506195316.png]]

[[Design Patterns]]