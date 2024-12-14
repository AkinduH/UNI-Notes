Input Stream

- An input stream is a conduit that funnels data from an external source into your Java program. This source could be a file, the keyboard, or even network input.
- In Java, `System.in` is a standard input stream that typically represents the keyboard.

```java
BufferedReader stdin = new BufferedReader(new InputStreamReader(System.in));
String line = stdin.readLine(); // Read a line of text from the keyboard
```

The `BufferedReader` allows you to read entire lines of text at once, which is often more practical than reading single characters.

**Output Stream**

- An output stream acts as a channel through which your Java program sends data to an external destination. This could be a file on your computer's disk, the console (your screen), or a network connection.
- `System.out` is the standard output stream in Java, often representing the console.


**Byte-Oriented Streams**

- **Core Purpose:** Designed to handle raw binary data, which means they deal with data as a sequence of individual bytes. Each byte represents an 8-bit value (0-255).
- **Data Types:** Suitable for working with various forms of data, including:
    - **Primitive data types:** `int`, `long`, `float`, `double`, etc.
    - **Raw bytes:** Image files, audio files, executable programs

**Character-Oriented Streams**

- **Core Purpose:** Primarily designed to handle textual data, where data is represented as a sequence of characters.
- **Data Types:** Ideal for handling text files, strings, and other data that consists of human-readable characters.

![[Pasted image 20240525180252.png]]

**Processing vs. Ordinary Streams:**
- **Ordinary Stream:** Directly connected to the data source or destination (e.g., `FileInputStream`, `FileOutputStream`).
- **Processing Stream:** Wraps another stream, adding functionality or transforming the data (e.g., `BufferedInputStream`, `DataInputStream`).


### Text File Output (Writing to a File)

- **FileWriter:**
    - **Purpose:** Establishes a direct connection to a file for writing text data.
    - **Limitation:** It can be less efficient for writing large amounts of text because it often writes to the file character by character.
    - **High Overhead:** Each byte requires a separate disk operation, involving seek time, rotational latency, and data transfer time.
    
- **BufferedWriter:**
    - **Purpose:** Enhances `FileWriter` by adding a buffer. This buffer stores characters temporarily, writing them to the file in larger chunks, improving performance.
    - **Benefits:**
        - Significantly improves writing speed, especially for larger files.
        - Provides convenient methods like `newLine()` to insert line breaks.
        - **Reduced Disk Operations:** Instead of reading/writing one byte at a time, the buffer fills up with a larger block of data (e.g., 512 bytes, 4KB). This reduces the number of disk operations significantly.

```java
import java.io.*;

class WriteAFile {
    public static void main(String[] args) {
        try {
            FileWriter fileWriter = new FileWriter("Foo.txt");
            //If the file doesn't exist, it will be created.
            BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);  
            bufferedWriter.write("hello foo!");
            bufferedWriter.newLine();
            bufferedWriter.write("This is a sample text file.");
            bufferedWriter.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
```

**Text File Input (Reading from a File)**

- **FileReader:**
    - **Purpose:** Establishes a connection to a file for reading text data.
    - **Limitation:** Reading character by character can be slow, especially for larger files.
- **BufferedReader:**
    - **Purpose:** Improves `FileReader` by adding buffering. It reads larger blocks of data from the file and stores them in the buffer, allowing you to read characters or lines more efficiently.
    - **Benefits:**
        - Makes reading much faster, especially for large files.
        - Provides `readLine()` for easy line-by-line processing.


```java
import java.io.*;

class ReadAFile {
    public static void main(String[] args) {
        try {
	        FileReader fileReader = new FileReader("Foo.txt");
	        BufferedReader bufferedReader = new BufferedReader(fileReader);
	        String line = null;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }
            bufferedReader.close();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
}
```


---

### Serialization

```java
FileOutputStream fileStream = new FileOutputStream("MyGame.ser");
//FileOutputStream object is designed to write raw bytes to a file.
ObjectOutputStream os = new ObjectOutputStream(fileStream);
//ObjectOutputStream is specifically designed to write objects to a file.
os.writeObject(characterOne);
os.writeObject(characterTwo);
os.writeObject(characterThree);
//turning the object's state into a byte stream.
os.close();
fileStream.close();
```

**Serializable vs. Not Serializable**

- **Serializable Types:**
    - **Primitive Types:** All primitive types in Java (`int`, `double`, `boolean`, etc.) are automatically serializable.
    - **Many Built-in Classes:** Many standard Java classes are designed to be serializable, such as:
        - `String`
        - `URL`
        - `Date`
        - `Point`
        - `Random`
    - **Collections:** Almost all collections from the `java.util` package (e.g., `ArrayList`, `HashMap`, `TreeSet`) are serializable.
- **Non-Serializable Types:**
    - **Custom Classes (By Default):** Your own classes are not serializable unless you explicitly make them serializable.
    - **Certain Classes:** Some built-in classes are not serializable, often for security or resource management reasons (e.g., threads, file streams).


**Making Your Classes Serializable**

```java
public class MyClass implements Serializable {
    // Your class members
}
```

**The `NotSerializableException`**

If you attempt to serialize an object where:

- The object's class doesn't implement `Serializable`.
- Any of the object's non-transient fields are not serializable.

You will encounter a `NotSerializableException`. This exception tells you that the serialization process cannot complete because the object's state cannot be properly converted into a byte stream.

**Versioning:** If you change the structure of your serializable class later (add or remove fields), it can create compatibility issues when deserializing older objects. The `serialVersionUID` is a mechanism to handle versioning, but it's a more advanced topic.


**Transient Fields**

- **Purpose:** The `transient` keyword in Java is used to mark fields within a class that you do not want to be serialized.
- **Serialization Exclusion:** When an object is serialized, any fields marked as `transient` are simply ignored. Their values are not included in the serialized byte stream.
- **Deserialization Default:** Upon deserialization (recreating the object from the byte stream), transient fields are assigned their default values:
    - For object references: `null`
    - For numerical types (`int`, `double`, etc.): `0`
    - For `boolean` types: `false`


```java
import java.io.*;

public class User implements Serializable {
    private String username;
    private transient String password; // Password won't be serialized

    // ... (constructors, getters, setters) ...
}
```


**Deserialization**

```java
FileInputStream fileStream = new FileInputStream("MyGame.ser");
//read raw bytes from a file.
ObjectInputStream os = new ObjectInputStream(fileStream);
//read objects from a file.
Object one = os.readObject();
Object two = os.readObject();
Object three = os.readObject();
os.close();
fileStream.close();
```

- **Object Order:** When deserializing, you must read the objects in the same order they were written during serialization.
- **Class Compatibility:** The class definition of the object you're deserializing (`GameCharacter` in this case) must be available at runtime and match the version that was used during serialization. Otherwise, you might get a `ClassNotFoundException` or `InvalidClassException`.

`Serialization in Java is specifically designed for Java-to-Java communication.`

`Chain streams (like BufferedInputStream, BufferedOutputStream) are designed to be used in conjunction with connection streams (like FileInputStream, FileOutputStream). They are not typically used on their own.`

`During deserialization, the constructor of the object is not called.`

`When a subclass is serializable but its superclass is not, the superclass must have a no-argument constructor to ensure proper initialization during deserialization.`

[[Exceptions]]