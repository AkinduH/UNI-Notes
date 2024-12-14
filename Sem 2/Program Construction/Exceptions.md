![[Pasted image 20240526192925.png]]

- The compiler enforces the handling of checked exceptions, ensuring that they are either caught or declared.
- It does not check for unchecked exceptions or errors, as these are considered the responsibility of the programmer.


Exception Handling and Call Stack

1. Exception Thrown: An exceptional condition occurs during program execution, triggering the creation of an exception object with error details.
    
2. Call Stack Unwinding: The JVM receives the exception and begins searching backward through the call stack for a matching handler.
    
3. Exception Handler Match: Each method in the call stack is checked for a `try-catch` block that matches the exception type or its superclass.
    
4. Exception Handling: If a match is found, execution jumps to the corresponding `catch` block where the error can be managed.
    
5. Unhandled Exceptions: If no matching handler is found, the JVM terminates the program and prints a stack trace for debugging.


Multiple Exceptions in Java:
1. Polymorphism: Exceptions inherit from a hierarchy, allowing them to be treated as their own type or any superclass.
   
2. Declaration: Methods can declare they might throw a general exception type, covering potential specific subtypes.
   
3. Catching: A `catch` block using a supertype will catch any thrown exception that's its subclass.
   
4. Granularity: Avoid catching `Exception`, as it's too broad. Use multiple `catch` blocks for specific types to tailor handling
   
5. Multiple catch blocks must be ordered from smallest to biggest

```java
try {
    // Code that might throw FileNotFoundException, IOException, or any other Exception
} catch (FileNotFoundException e) {
    // Handle FileNotFoundException specifically
    System.out.println("File eka nane");
} catch (IOException e) {
    // Handle other IOExceptions
} catch (Exception e) {
    // Handle all other Exceptions
    System.out.println("Hutto loku exception ekak");
}
```

More details on streams 

- **Byte Arrays:** `ByteArrayInputStream`, `ByteArrayOutputStream` for reading/writing raw byte data (e.g., images).
- **Character Arrays:** `CharArrayReader`, `CharArrayWriter` for working with character data.
- **Strings:** `StringReader`, `StringWriter` for processing text directly in String format.
- **Remote I/O:** `InputStream`, `OutputStream` (and subclasses) for network communication.

**Filters**

Filters wrap another input/output stream, modifying the data after it is read or before it is written.

**Buffering:**
- `BufferedInputStream`, `BufferedOutputStream`: Read/write data in larger blocks for efficiency.
- `BufferedReader`, `BufferedWriter`: Read/write lines of text efficiently.

**Print Streams:**
- **Purpose:** Provide convenient methods for writing formatted text output.
- **Classes:**
    - `PrintStream`: Writes to a byte output stream (e.g., `System.out`).
    - `PrintWriter`: Writes to a character output stream or writer.
- **Methods:**
    - `print()`: Writes a String representation of a value without a newline.
    - `println()`: Writes a String representation of a value followed by a newline.

**Closing Streams**

```java
FileReader r = null; // Initialize the reader as null
try {
    r = new FileReader("somefile.txt"); // Open the file
    // Read from the file
} catch (IOException e) { 
    // Handle potential IOExceptions (e.g., file not found)
} finally {
    if (r != null) { // Check if the reader was successfully opened
        try {
            r.close(); // Attempt to close the reader
        } catch (Exception ignoreMe) {
            // If closing fails, it's usually not recoverable, but you might log it.
        }
    }
}
```

**Important Note:**

Since Java 7, a more concise and safer way to handle resources like streams is using the try-with-resources statement:
In this approach, the resource (`r`) is automatically closed when the `try` block completes, even if exceptions occur.
```java
try (FileReader r = new FileReader("somefile.txt")) {
    // Read from the file
} catch (IOException e) {
    // Handle exception
}
```


> [!Scanner vs BufferedReader]
> - **Scanner**:
>     
>     - **Easier to use for parsing**: It has built-in methods like `nextInt()`, `nextDouble()`, `nextLine()`, etc., which directly parse the data into the required types.
>     - **Automatic Parsing**: The `Scanner` handles the conversion from string to various primitive types automatically.
>     - **Token-based**: Can read tokens based on delimiters (default is whitespace), making it easy to process structured data.
> - **BufferedReader**:
>     
>     - **Manual Parsing Required**: Reads lines of text, and you must manually convert these lines into other data types using methods like `Integer.parseInt()`, `Double.parseDouble()`, etc.
>     - **Line-based**: Reads the entire line as a string, giving you more control but also requiring more effort to parse the data.
>     - **Efficient for Large Files**: BufferedReader reads data in larger chunks and can be more efficient for large files.

### **Throwing Exceptions**

Approach 1: Throw Early (Preferred)
```java
public static void readData(String filename) throws FileNotFoundException, NumberFormatException {
    File inFile = new File(filename);

    if (!inFile.exists()) {
        throw new FileNotFoundException("File not found: " + filename);
    }

    Scanner in = new Scanner(inFile);
    while (in.hasNext()) {
        if (!in.hasNextInt()) {
            throw new NumberFormatException("Invalid number format in file: " + filename);
        }
        int number = in.nextInt();
        // Process the number...
    }
    in.close();
}

public static void main(String[] args) {
    try {
        readData("numbers.txt");
    } catch (FileNotFoundException e) {
        System.err.println("Error:" + e.getMessage());
    } catch (NumberFormatException e) {
        System.err.println("Error:" + e.getMessage());
    }
}
```

Approach 2: No Explicit Throws (Less Ideal)
```java
public void readData(String filename) {
    try {
        File inFile = new File(filename);
        Scanner in = new Scanner(inFile);
        while (in.hasNext()) {
            int number = in.nextInt(); // Potential NumberFormatException
            // Process the number...
        }
    } catch (FileNotFoundException e) {
        System.err.println("Error: File not found: " + filename);
    } catch (NumberFormatException e) {
        System.err.println("Error: Invalid number format in file: " + filename);
    }
}
```

| Feature            | Throw Early (Preferred)  | No Explicit Throws (Less Ideal)    |
| ------------------ | ------------------------ | ---------------------------------- |
| Error Detection    | Immediate                | Delayed until the problematic line |
| Code Clarity       | Higher                   | Lower                              |
| Exception Handling | Separate from main logic | Mixed with main logic              |

**Throw Early, Catch Late:**

- **Principle:** This principle advocates for throwing an exception as soon as an error condition is detected within a method, rather than attempting to handle it immediately. The exception can then be caught and handled at a higher level of the program where more context is available for appropriate error recovery.


### **Create Custom Exceptions**

Example: `InsufficientFundsException`
```java
public class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException() {} // Constructor with no arguments
    public InsufficientFundsException(String message) { // Constructor with message
        super(message);
    }
}
```

**Usage:**
```java
if (amount > balance) {
    throw new InsufficientFundsException("Withdrawal of " + amount + " exceeds balance of " + balance);
}
```

**Catching and Retrieving the Message:**
```java
try {
    // Code that might throw InsufficientFundsException
} catch (InsufficientFundsException e) {
    System.err.println("Error: " + e.getMessage()); // Prints the custom message
}
```

[[Concurrency]]