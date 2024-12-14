
## Chain of Responsibility Pattern

**Purpose**

- Decouple the sender of a request from its receiver.
- Allow multiple objects a chance to handle a request.

**Structure**

- **Sender:** Initiates the request.
- **Handlers:** Series of objects, each responsible for a specific type of request.
- **Chain:** The order in which handlers are connected.

**How it Works**

1. **Sender** passes the request to the first handler in the chain.
2. Each **handler** examines the request:
    - **If it can handle it:** Processes the request and the chain terminates.
    - **If it cannot handle it:** Passes the request to the next handler in the chain.
3. This continues until the request is handled or the end of the chain is reached.

**Benefits**

- **Flexibility:** Easily add, remove, or reorder handlers at runtime.
- **Loose Coupling:** Sender and receivers are loosely coupled.
- **Open/Closed Principle:** New handlers can be added without modifying existing code.

**Comparison with Decorator Pattern**

|Feature|Chain of Responsibility|Decorator|
|---|---|---|
|Purpose|Delegate responsibility|Extend behavior|
|Handling|One handler|All handlers|
|Example|Error handling|Logging, caching, etc.|

**Key Considerations**

- **Termination:** Decide how to handle requests that reach the end of the chain.
- **Chain Length:** Avoid overly long chains.
- **Request Type:** Determine the representation of requests.

![[Screenshot (306).png]]


```java
//Handler Interface:

interface Handler {
    void setNextHandler(Handler handler);  // Link to the next handler in the chain
    void handleRequest(int amount);        // Method to process the request
}


//Concrete Handlers:

class Dispenser50 implements Handler {
    private Handler next;

    public void setNextHandler(Handler handler) {
        this.next = handler;
    }

    public void handleRequest(int amount) {
        if (amount >= 50) {
            int num = amount / 50;
            System.out.println("Dispensing " + num + " 50$ note(s)");
            int remaining = amount % 50;
            if (remaining != 0) this.next.handleRequest(remaining);
        } else {
            this.next.handleRequest(amount);
        }
    }
}
// Similar classes for Dispenser20, Dispenser10, and Dispenser1


//ATM Class (Chain Setup and Usage):

class ATMDispenserChain {
    private Handler chain;

    public ATMDispenserChain() {
        // Create the chain of responsibility: $50 -> $20 -> $10 -> $1
        this.chain = new Dispenser50();
        Handler chain20 = new Dispenser20();
        Handler chain10 = new Dispenser10();
        Handler chain1 = new Dispenser1();
        chain.setNextHandler(chain20);
        chain20.setNextHandler(chain10);
        chain10.setNextHandler(chain1);
    }

    public void dispense(int amount) {
        if (amount > 0) {
            chain.handleRequest(amount);
        } else {
            System.out.println("Invalid amount");
        }
    }
}


//Usage:

public class Main {
    public static void main(String[] args) {
        ATMDispenserChain atmDispenser = new ATMDispenserChain();
        atmDispenser.dispense(289); // Example: Dispense $289
    }
}
```

```
Output:

Dispensing 5 50$ note(s)
Dispensing 1 20$ note(s)
Dispensing 1 10$ note(s)
Dispensing 1 5$ note(s)
Dispensing 4 1$ note(s)
```


![[Pasted image 20240611140059.png]]


## Command

**Core Idea**

- **Encapsulate actions as objects:** Treat requests or operations as self-contained objects. This allows for greater flexibility and decoupling.

**Key Components**

1. **Command:**
    - **Interface:** Defines methods like `execute()` and, optionally, `undo()`.
    - **Concrete Commands:** Implement the interface, encapsulating specific actions and parameters.
    - **Stores receiver:** Holds a reference to the object performing the action.
    - **Additional Functionality (Optional):**
        - `undo()`: Reverses the effects of the command.
        - `queue()`: Allows commands to be scheduled for later execution.
        - `log()`: Records changes for auditing or recovery.

```java
// Command interface
interface Command {
    void execute();
}

// Concrete command: Turn on a light
class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) { // Store the receiver
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();  // Delegate to the receiver
    }
}

// Concrete command: Turn off a light
class LightOffCommand implements Command {
    private Light light;

    // ... (similar constructor and execute() method as LightOnCommand)
}
```

2. **Receiver:**
    - **Performs the work:** Contains the actual logic for carrying out the command.

```java
class Light {
    public void turnOn() {
        System.out.println("Light is on");
    }

    public void turnOff() {
        System.out.println("Light is off");
    }
}
```

3. **Invoker:**
    - **Triggers execution:** Holds references to Command objects and initiates their execution by calling `execute()`.
    - **Optional history:** Can maintain a history of commands for undo/redo functionality.

```java
class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();  // Trigger the command
    }
}
```

4. **Client:**
    - **Sets up the system:** Creates concrete command objects and associates them with receivers.
    - **Decides what to do:** Determines which commands to execute and when.

```java
public class Main {
    public static void main(String[] args) {
        // Create receiver and commands
        Light livingRoomLight = new Light();
        LightOnCommand lightOn = new LightOnCommand(livingRoomLight); // Associate command with receiver
        LightOffCommand lightOff = new LightOffCommand(livingRoomLight);

        // Create invoker
        RemoteControl remote = new RemoteControl();

        // Client decides to turn on the light
        remote.setCommand(lightOn); 
        remote.pressButton(); 
        
        // Client decides to turn off the light
        remote.setCommand(lightOff);
        remote.pressButton();
    }
}
```


**Interaction Flow**

1. **Client:** Creates a command object and sets its receiver.
2. **Client:** Gives the command object to the invoker.
3. **Invoker:** Holds the command until it's time to execute it.
4. **Invoker:** Calls `execute()` on the command.
5. **Command:** Calls the appropriate method on the receiver to perform the action.

**Benefits**

- **Decoupling:** Separates the object that requests an action from the object that performs it.
- **Flexibility:** Commands can be parameterized, queued, logged, or combined for complex operations.
- **Extensibility:** New commands can be easily added without changing existing code.
- **Undo/Redo Support:** Simplifies the implementation of undo/redo functionality.

![[Screenshot (307).png]]

![[Pasted image 20240613125737.png]]

## Interpreter

Purpose:

- Define and interpret custom languages (domain-specific languages or mini-languages).
- Represent and evaluate expressions within those languages.

Components:

1. Expression Hierarchy:
    
    - Abstract Expression (interface/base class): Defines the `interpret()` method and any common logic.
    - Terminal Expressions: Represent basic elements of the language (numbers, literals).
    - Non-terminal Expressions: Represent complex structures (operators, functions).
2. Abstract Syntax Tree (AST):
    
    - Represents the syntactic structure of an expression.
    - Nodes are instances of Expression classes.
    - Built by a parser (not part of the core pattern).
3. Context (Optional):
    
    - Stores information relevant to interpretation (e.g., variable values).

Interpretation Process:

1. Parsing: An external parser builds the AST from the input expression.
2. Interpretation: Recursively call `interpret()` on each node of the AST, passing the context if needed.
    - Terminal expressions return their values.
    - Non-terminal expressions combine the results of their children.

Advantages:

- Extensibility: Easy to add or modify grammar rules by creating new Expression classes.
- Flexibility: Dynamically change interpretation behavior at runtime.
- Modularity: Clear separation of concerns for each expression type.

Disadvantages:

- Complexity: Large grammars can lead to many classes and potential maintenance challenges.
- Performance: Interpreted code is generally slower than compiled code.

When to Use:

- Domain-Specific Languages (DSLs): To create custom languages for specific problems.
- Simple Grammars: When the language syntax is not too complex.
- Flexibility and Extensibility: When you anticipate the need to evolve the language.

Alternatives:

- Parser Generators (ANTLR, YACC): For more complex grammars, these tools can automate parser and interpreter generation.
- Compilation: If performance is critical, consider compiling the language instead of interpreting it.

1. Expression Interface:

```java
interface Expression {
    int interpret(Map<String, Integer> context); // Context for variable values
}
```

2. Terminal Expressions:

```java
class Number implements Expression {
    private int value;

    public Number(int value) {
        this.value = value;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        return value;
    }
}

class Variable implements Expression {
    private String name;

    public Variable(String name) {
        this.name = name;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        return context.getOrDefault(name, 0); // 0 if variable not found
    }
}
```

3. Non-Terminal Expressions:

```java
class SquareExpression implements Expression {
    private Expression expression;

    public SquareExpression(Expression expression) {
        this.expression = expression;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        int value = expression.interpret(context);
        if (value % 2 == 0) { // Check if even
            return value * value;
        } else {
            return 0; // Ignore odd numbers
        }
    }
}

class SumExpression implements Expression {
    private List<Expression> expressions;

    public SumExpression(List<Expression> expressions) {
        this.expressions = expressions;
    }

    @Override
    public int interpret(Map<String, Integer> context) {
        int sum = 0;
        for (Expression expr : expressions) {
            sum += expr.interpret(context);
        }
        return sum;
    }
}
```

4. Client Code:

```java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> context = new HashMap<>();
        context.put("a", 2);
        context.put("b", 5); // Odd number, will be ignored

        List<Expression> expressions = new ArrayList<>();
        expressions.add(new SquareExpression(new Number(4)));
        expressions.add(new SquareExpression(new Variable("a")));
        expressions.add(new SquareExpression(new Number(6)));
        expressions.add(new SquareExpression(new Variable("b"))); 

        Expression expr = new SumExpression(expressions);

        int result = expr.interpret(context);
        System.out.println("Result: " + result); // Output: Result: 56
    }
}
```

![[Pasted image 20240613162213.png]]


## Iterator

Purpose:

- Provide a way to access elements of a collection sequentially without exposing its underlying representation.
- Enable different traversal algorithms (forward, backward, filtered) without modifying the collection's interface.

Key Components:

- Iterator: An interface defining methods for traversal:
    
    - `hasNext()`: Checks if there are more elements to iterate over.
    - `next()`: Returns the next element and moves the iterator's position forward.
- Concrete Iterator: A class implementing the Iterator interface:
    
    - Encapsulates the traversal logic specific to a particular collection type.
    - Maintains the current position during iteration.
- Collection: Provides a method `iterator()` to obtain an iterator instance.
    

How It Works:

1. Client obtains an iterator from the collection using the `iterator()` method.
2. Client uses the iterator's `hasNext()` and `next()` methods to traverse the collection:
    - While `hasNext()` is true, the `next()` method retrieves the current element and advances the iterator.

Benefits:

- Decoupling: Separates traversal logic from the collection's internal implementation.
- Flexibility: Allows for different traversal algorithms without modifying the collection.
- Extensibility: New traversal methods can be added by creating new iterators.
- Multiple Traversals: Supports multiple independent iterations over the same collection.

Real-World Examples:

- Java Collections Framework: The Iterator interface is the basis for iterating over most collection classes (e.g., ArrayList, HashSet, TreeSet).
- Custom Data Structures: You can implement iterators for your own data structures to provide consistent and flexible traversal.

Note:

- The Iterator pattern is a behavioral pattern that focuses on how objects interact rather than their structure.
- It promotes loose coupling and adherence to the Open/Closed Principle.


![[Screenshot (308).png]]


**1. Iterator Interface:**

```java
import java.util.Iterator;

public interface Iterator {
    boolean hasNext();  // Check if there are more elements
    Object next();      // Return the next element and move the iterator forward
}
```


**2. Concrete Collection (NotificationCollection):**

```java
public class NotificationCollection implements Iterable { 
    private Notification[] notificationList;
    private int index;

    public NotificationCollection() {
        notificationList = new Notification[5];
        index = 0;
    }

    public void addItem(String str) {
        Notification notification = new Notification(str);
        if (index < notificationList.length) {
            notificationList[index++] = notification;
        } else {
            System.out.println("Notification List is full!!");
        }
    }

    @Override
    public Iterator iterator() {
        return new NotificationIterator(this); // Create a specific iterator for this collection
    }
}
```


**3. Concrete Iterator (NotificationIterator):**

```java
public class NotificationIterator implements Iterator {
    private NotificationCollection collection;
    private int pos;

    public NotificationIterator(NotificationCollection collection) {
        this.collection = collection;
        pos = 0;
    }

    @Override
    public boolean hasNext() {
        return pos < collection.notificationList.length && collection.notificationList[pos] != null;
    }

    @Override
    public Object next() {
        return collection.notificationList[pos++];
    }
}
```


**4. Notification Class:**

```java
public class Notification {
    private String notification;

    public Notification(String notification) {
        this.notification = notification;
    }

    public String getNotification() {
        return notification;
    }
}
```


**5. Client Code:**

```java
public class Main {
    public static void main(String[] args) {
        NotificationCollection nc = new NotificationCollection();
        nc.addItem("Notification 1");
        nc.addItem("Notification 2");
        nc.addItem("Notification 3");

        Iterator iterator = nc.iterator();

        while (iterator.hasNext()) {
            Notification n = (Notification) iterator.next();
            System.out.println(n.getNotification());
        }
    }
}
```

![[Pasted image 20240613181838.png]]


## Mediator

Purpose:

- Manage complex interactions between objects.
- Decouple objects by centralizing communication through a mediator object.

Components:

- Mediator:
    - Defines an interface for communication between colleagues.
    - Coordinates interactions and message passing between colleagues.
- Concrete Mediator:
    - Implements the mediator interface.
    - Contains the logic to manage and coordinate communication between colleagues.
- Colleagues:
    - Interact with each other only through the mediator.
    - Unaware of the existence of other colleagues.

How It Works:

1. Colleagues register with the mediator.
2. Colleagues send messages to the mediator, not directly to each other.
3. The mediator receives messages, processes them (if necessary), and forwards them to the appropriate colleagues.

Benefits:

- Reduced coupling: Colleagues are loosely coupled and only depend on the mediator interface.
- Simplified communication: Centralized communication logic makes the system easier to understand and maintain.
- Flexibility: Changes to individual colleagues or their interactions can be made in the mediator without affecting other colleagues.
- Single Responsibility Principle: Each object focuses on its core responsibility, and the mediator handles communication.
- Improved testability: Isolating communication logic in the mediator makes it easier to write unit tests.

When to Use:

- Complex Interactions: When objects have many-to-many relationships that make the system difficult to understand or change.
- Tight Coupling: If changes in one object ripple through the system, impacting multiple other objects.
- Centralized Control: When you need a central point to manage communication and behavior in a complex system.

Key Considerations:

- Mediator can become a bottleneck or God Object if not designed carefully.
- The mediator interface should be flexible enough to accommodate different types of messages and interactions.



**1. Mediator Interface:**

```java
interface ChatMediator {
    void sendMessage(String msg, User user);
    void addUser(User user);
}
```


**2. Concrete Mediator (ChatRoom):**

```java
class ChatRoom implements ChatMediator {
    private List<User> users;

    public ChatRoom() {
        users = new ArrayList<>();
    }

    @Override
    public void sendMessage(String msg, User user) {
        for (User u : users) {
            // message should not be received by the user sending it
            if (u != user) {
                u.receive(msg);
            }
        }
    }

    @Override
    public void addUser(User user) {
        users.add(user);
    }
}
```


**3. Colleague (User):**

```java
class User {
    private String name;
    private ChatMediator mediator;

    public User(String name, ChatMediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }

    public void send(String msg) {
        System.out.println(this.name + ": Sending Message=" + msg);
        mediator.sendMessage(msg, this);
    }

    public void receive(String msg) {
        System.out.println(this.name + ": Received Message:" + msg);
    }
}
```


**4. Client:**

```java
public class Main {
    public static void main(String[] args) {
        ChatMediator mediator = new ChatRoom();
        User user1 = new User("Tom", mediator);
        User user2 = new User("Alice", mediator);
        User user3 = new User("Bob", mediator);
        mediator.addUser(user1);
        mediator.addUser(user2);
        mediator.addUser(user3);

        user1.send("Hi All");
        user2.send("Hello!");
    }
}
```

![[Pasted image 20240613202025.png]]

## Memento

 Purpose:

- Captures and externalizes an object's internal state so that the object can be restored to this state later.
- Provides a way to undo or rollback changes to an object.

 Components:

- **Originator:** The object whose state needs to be saved.
- **Memento:** Stores a snapshot of the originator's state.
- **Caretaker:** Manages memento objects but cannot access their contents.

 How It Works:

1. The originator creates a memento object, capturing its current internal state.
2. The originator passes the memento to the caretaker.
3. The caretaker stores the memento.
4. When needed, the caretaker passes the memento back to the originator.
5. The originator uses the memento to restore its internal state.

 Benefits:

- Protects encapsulation by preventing direct access to an object's internal state.
- Provides a simple mechanism for undoing or rolling back changes.
- Can be used for implementing features like undo/redo, version control, and state restoration.

 When to Use:

- When you need to capture and restore an object's internal state.
- When direct access to the object's state would violate encapsulation.
- When you need to implement undo/redo functionality.

![[Screenshot (366).png]]

###### Memento

```java
public class TextEditorMemento {
    private final String content;

    public TextEditorMemento(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}
```

###### Originator

```java
public class TextEditor {
    private StringBuilder content;

    public TextEditor() {
        content = new StringBuilder();
    }

    public void type(String words) {
        content.append(words);
    }

    public TextEditorMemento save() {
        return new TextEditorMemento(content.toString());
    }

    public void restore(TextEditorMemento memento) {
        content = new StringBuilder(memento.getContent());
    }

    public String getContent() {
        return content.toString();
    }
}
```

###### Caretaker

```java
import java.util.Stack;

public class History {
    private Stack<TextEditorMemento> mementos = new Stack<>();

    public void push(TextEditorMemento memento) {
        mementos.push(memento);
    }

    public TextEditorMemento pop() {
        if (mementos.isEmpty()) {
            return null;
        }
        return mementos.pop();
    }
}
```

###### Usage

```java
public class Main {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        History history = new History();

        editor.type("This is the first sentence.");
        history.push(editor.save());

        editor.type(" This is the second sentence.");
        history.push(editor.save());

        editor.type(" This is the third sentence.");

        System.out.println(editor.getContent());

        editor.restore(history.pop());
        System.out.println(editor.getContent());
    }
}
```

![[Pasted image 20240724133256.png]]


## Observer

Problem:

- Multiple objects depend on a single object.
- Updating dependent objects when the main object changes requires loose coupling.

Solution:

- Define a one-to-many dependency between objects.
- Subject (main object) maintains a list of observers.
- Observers register with the subject to receive notifications.
- Subject notifies observers when its state changes.

Components:

- **Subject:** The object being observed.
    - Maintains a list of observers.
    - Notifies observers when its state changes.
- **Observer:** Depends on the subject.
    - Updates itself when notified by the subject.

How it works:

1. Observers register with the subject.
2. Subject undergoes a state change.
3. Subject notifies all registered observers.
4. Observers update themselves based on the notification.

Benefits:

- Loose coupling between subject and observers.
- Supports dynamic addition/removal of observers.
- Encapsulates the notification mechanism.

When to use:

- One-to-many dependencies between objects.
- Objects need to be notified of state changes in other objects.
- Dynamic number of observers.


![[Screenshot (367).png]]

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update(String message);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    private String message;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void setState(String message) {
        this.message = message;
        notifyObservers();
    }
}

class ConcreteObserver implements Observer {
    @Override
    public void update(String message) {
        System.out.println("Observer received message: " + message);
    }
}

public class Main {
    public static void main(String[] args) {
        Subject subject = new Subject();

        Observer observer1 = new ConcreteObserver();
        Observer observer2 = new ConcreteObserver();

        subject.addObserver(observer1);
        subject.addObserver(observer2);

        subject.setState("Hello, observers!");

        subject.removeObserver(observer2);

        subject.setState("Another message");
    }
}
```

![[Pasted image 20240724144606.png]]

## State

Problem:

- An object's behavior needs to change based on its internal state.
- Tight coupling between state and behavior makes it difficult to add new states and behaviors.

Solution:

- Encapsulate state-specific behaviors into separate state objects.
- The main object delegates behavior to the current state object.

Components:

- **Context:** The main object that delegates behavior to the current state.
- **State:** The interface defining the state-specific behavior.
- **ConcreteState:** Concrete implementations of the State interface.

How it works:

1. The context maintains a reference to the current state object.
2. The context delegates requests to the current state object.
3. The current state object handles the request based on its state-specific logic.
4. The state can change the context's current state object.

Benefits:

- Improves code maintainability and extensibility.
- Allows for easy addition of new states and behaviors.
- Encapsulates state-specific logic.

When to use:

- When an object's behavior depends on its state.
- When the number of states is likely to change.
- When you want to isolate state-specific logic.

![[Screenshot (368).png]]

```java
interface MobileState {
    void powerOn();
    void powerOff();
    void receiveCall();
}

class OnState implements MobileState {
    @Override
    public void powerOn() {
        System.out.println("Mobile is already on.");
    }

    @Override
    public void powerOff() {
        System.out.println("Powering off...");
    }

    @Override
    public void receiveCall() {
        System.out.println("Receiving call...");
    }
}

class OffState implements MobileState {
    @Override
    public void powerOn() {
        System.out.println("Powering on...");
    }

    @Override
    public void powerOff() {
        System.out.println("Mobile is already off.");
    }

    @Override
    public void receiveCall() {
        System.out.println("Mobile is off. Cannot receive call.");
    }
}

class Mobile {
    private MobileState state;

    public Mobile() {
        state = new OffState();
    }

    public void setState(MobileState state) {
        this.state = state;
    }

    public void powerOn() {
        state.powerOn();
    }

    public void powerOff() {
        state.powerOff();
    }

    public void receiveCall() {
        state.receiveCall();
    }
}
```

![[Pasted image 20240724161043.png]]

## Strategy

Problem:

- An object needs to use different algorithms to perform a task.
- The algorithm choice depends on runtime conditions.
- Tight coupling between the object and algorithms limits flexibility.

Solution:

- Encapsulate algorithms as separate strategy objects.
- The context object delegates the task to the appropriate strategy.

Components:

- **Context:** The object that uses the strategy.
- **Strategy:** The interface defining the algorithm.
- **ConcreteStrategy:** Concrete implementations of the strategy.

How it works:

1. The context holds a reference to a strategy object.
2. The context delegates the task to the current strategy.
3. The strategy executes the algorithm.
4. The context can change the strategy at runtime based on conditions.

Benefits:

- Improves code flexibility and maintainability.
- Allows for easy addition of new algorithms.
- Promotes loose coupling between the context and algorithms.

When to use:

- When you need to choose an algorithm at runtime.
- When you want to isolate algorithm implementations.
- When different algorithms provide the same functionality.

Key considerations:

- An important concept being highlighted by the strategy design pattern is that the behaviour of an object should not be inherited but should be encapsulated using objects that define behaviour (that is, interfaces).

![[Screenshot (369).png]]

```java
interface CompressionStrategy {
    void compress(Image image);
}

class JpegCompression implements CompressionStrategy {
    @Override
    public void compress(Image image) {
        // Implement JPEG compression logic
        System.out.println("Compressing image using JPEG");
    }
}

class PngCompression implements CompressionStrategy {
    @Override
    public void compress(Image image) {
        // Implement PNG compression logic
        System.out.println("Compressing image using PNG");
    }
}

class ImageCompressor {
    private CompressionStrategy strategy;

    public ImageCompressor(CompressionStrategy strategy) {
        this.strategy = strategy;
    }

    public void setCompressionStrategy(CompressionStrategy strategy) {
        this.strategy = strategy;
    }

    public void compress(Image image) {
        strategy.compress(image);
    }
}

class Image {
    // Image data
}

public class Main {
    public static void main(String[] args) {
        Image image = new Image();

        // Compress as JPEG
        ImageCompressor compressor = new ImageCompressor(new JpegCompression());
        compressor.compress(image);

        // Compress as PNG
        compressor.setCompressionStrategy(new PngCompression());
        compressor.compress(image);
    }
}
```

![[Pasted image 20240724175058.png]]

![[Screenshot (370).png]]

## Template

Summary:

- **Defines a skeleton of an algorithm in a superclass.**
- **Subclasses provide concrete implementations for specific steps.**
- **Ensures algorithm structure is maintained while allowing customization.**

Key Points:

- **Template method:** The overarching algorithm defined in the base class.
- **Hook methods:** Optional steps in the template method that subclasses can override.
- **Abstract methods:** Required steps that subclasses must implement.
- **Enforces algorithm structure:** Prevents subclasses from overriding the template method.
- **Promotes code reuse and extensibility.**
- The template method essentially provide a skeletal implementation of an operation or an algorithm with the **invariant parts** properly defined in the method while the **variant parts** are left for customization or refinement.

![[Screenshot (371).png]]

```java
abstract class AbstractCoffee {
  final void prepareCoffee() {
    boilWater();
    brewCoffee();
    pourInCup();
    addSugarAndMilk();
  }

  abstract void brewCoffee();

  void boilWater() {
    System.out.println("Boiling water");
  }

  void pourInCup() {
    System.out.println("Pouring into cup");
  }

  void addSugarAndMilk() {
    System.out.println("Adding sugar and milk");
  }
}

class CoffeeWithMilk extends AbstractCoffee {
  @Override
  void brewCoffee() {
    System.out.println("Brewing coffee with milk");
  }
}

class CoffeeWithCream extends AbstractCoffee {
  @Override
  void brewCoffee() {
    System.out.println("Brewing coffee with cream");
  }
}
```

![[Pasted image 20240724205418.png]]

## Visitor

The Problem:

- **Inflexibility:** Existing object structures often lack the necessary operations.
- **Inheritance Limitations:** Extending objects to add new operations can lead to tight coupling and violation of the Open-Closed Principle.

The Visitor Pattern Solution:

- **Separation of Concerns:** Separates the algorithm (Visitor) from the data structure (Element).
- **Dynamic Dispatch:** Allows for new operations without modifying existing classes.
- **Flexibility:** Enables the introduction of new operations by creating new Visitor classes.

Key Concepts:

- **Visitor:** Encapsulates the new operation.
- **Element:** The object structure to be visited.
- **Accept Method:** Defines the interface for accepting visitors in the Element.

![[Screenshot (372).png]]

```java
interface AbstractVisitor {
    public void doSpecialOp(AbstractElement ae);
}

class Visitor implements AbstractVisitor {
    public void doSpecialOp(AbstractElement ae) {
        System.out.print("Visitor is doing the special operation on ");
        System.out.println(((Element)ae).getData());
    }
}

interface AbstractElement {
    public void accept(AbstractVisitor av);
}

class Element implements AbstractElement {
    private String data;

    public Element(String data) {
        this.data = data;
    }

    public String getData() {
        return data;
    }

    public void accept(AbstractVisitor av) {
        av.doSpecialOp(this);
    }
}

class Client {
    private Element e;
    private Visitor v;

    public void action(String data) {
        e = new Element(data);
        v = new Visitor();
        e.accept(v);
    }
}

public class VisitorTester {
    public static void main(String [] args) {
        Client c = new Client();
        c.action("data sent to element by client");
    }
}
```

![[Pasted image 20240724214001.png]]