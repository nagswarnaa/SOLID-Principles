### SOLID Principles:
1. Single Responsibility Principle
2. Open/Closed Principle
3. Liskov Substitution Principle
4. Interface Seggregation Principle
5. Dependency Inversion Principle

Let's dive deep into each one these along with examples:

## 1. Single Responsibility Principle 
    A class should have only one reason to change, means a class should do only one job or should have only one responsibility.
Let's dive deeper into the Single Responsibility Principle (SRP) with a practical example. Consider a system for managing a library's book inventory. A common violation of SRP occurs when a class takes on responsibilities that should be separated into different classes.

    class LibraryBook:
      def __init__(self, title, author, isbn):
          self.title = title
          self.author = author
          self.isbn = isbn
          self.location = None

      def check_in(self, location):
          self.location = location
          # Logic to add the book back to the library inventory

      def check_out(self):
          self.location = 'Checked out'
          # Logic to remove the book from the library inventory

      def save(self):
          # Logic to save the book details to a database
Above example is a violation of SRP Principle since it has multiple responsibilities, Checking-in the books(Inventory), persisting book data to a database, managing book properties. Refactoring above code adhering to SRP principle:

    class LibraryBook:
        def __init__(self, title, author, isbn):
            self.title = title
            self.author = author
            self.isbn = isbn

    class InventoryManager:
        def __init__(self):
            self.books = {}  # Stores book ISBNs and their locations

        def check_in(self, isbn, location):
            self.books[isbn] = location
            # Additional logic for adding the book back to inventory

        def check_out(self, isbn):
            self.books[isbn] = 'Checked out'
            # Additional logic for removing the book from inventory

    class BookPersistence:
        def save_book(self, library_book):
            # Logic to save the book details to a database
            pass

        def load_book(self, isbn):
            # Logic to load book details from a database
            pass
However, following the Single Responsibility Principle (SRP) often leads to a design with more classes, each handling a single responsibility. Applying SRP (and other design principles) requires balance. Over-segmentation can lead to an excessive number of classes, making the system overly complex and potentially difficult to navigate. The key is to identify genuinely distinct responsibilities that justify separation into different classes. The goal is to enhance the maintainability, scalability, and understandability of the code without overcomplicating the design.

Therefore, while SRP often results in creating more classes, the principle should be applied judiciously to strike the right balance between simplicity and modularity.

In the Java Collections Framework, the principle of Single Responsibility is followed, ensuring that each class has one and only one reason to change. This principle is a key aspect of SOLID design principles, promoting a cleaner, more modular structure.

A clear example of the Single Responsibility Principle (SRP) in the Java Collections Framework is the distinction between the `List`, `Set`, and `Map` interfaces, each serving a distinct purpose:

- **`List` Interface**: Represents an ordered collection of elements that can contain duplicate values. It is responsible for maintaining the insertion order of the elements, and it allows positional access and insertion of elements. Classes like `ArrayList` and `LinkedList` implement the `List` interface, each with a specific approach to data storage and access, but all adhering to the single responsibility of managing an ordered collection.

- **`Set` Interface**: Represents a collection that cannot contain duplicate elements. It is designed for scenarios where uniqueness of the elements is crucial. Implementations of the `Set` interface, such as `HashSet` and `TreeSet`, ensure that no two elements in the collection are the same, adhering strictly to their single responsibility of managing a set of unique elements.

- **`Map` Interface**: Represents a collection of key-value pairs, where each key is unique. This interface is responsible for associating unique keys to specific values, allowing for efficient retrieval, update, and deletion of entries based on the key. Classes like `HashMap` and `TreeMap` implement the `Map` interface, each ensuring that they adhere to the single responsibility of managing a collection of key-value pairs where keys are unique.

## 2. Open/Closed Principle: 
    A Software entity(class, module) should be open for extension and closed for modification. This principle aims to allow systems to grow and change with new requirements without directly modifying existing source code, thereby reducing the risk of introducing new bugs in previously tested and validated code.
- **Open for Extension:** You should be able to extend the behavior of a module if the requirements of the application change, or to add new features.
- **Closed for Modification:** Extending the behavior of a module should not require changing the source code of the module itself. Instead, the new behavior should be added by creating new entities.
- 
**How to Apply OCP**

Applying OCP typically involves the use of interfaces or abstract classes to abstract away the concrete implementations of behaviors or algorithms. By programming to an interface, your system can easily incorporate new behaviors by adding new classes that implement these interfaces without changing the existing code.


      from abc import ABC, abstractmethod
     class ReportGenerator(ABC):
        @abstractmethod
        def generate_report(self, content):
            pass

    class PdfReportGenerator(ReportGenerator):
        def generate_report(self, content):
            # Logic to generate a PDF report
            print("Report generated in PDF format.")

    class HtmlReportGenerator(ReportGenerator):
        def generate_report(self, content):
            # Logic to generate an HTML report
            print("Report generated in HTML format.")

The above system is open for extension because you can add new report formats by creating new classes that implement the ReportGenerator interface. It is closed for modification because you do not need to change existing classes to add new types of reports.

The Open/Closed Principle, one of the SOLID design principles, states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This principle promotes the idea of designing your modules so that new functionality can be added without changing the existing code.

In the Java Collections Framework, the Open/Closed Principle is exemplified by the design of collection interfaces and their implementation classes. Let's focus on the `Collection` interface and its relationship with various concrete classes like `ArrayList`, `LinkedList`, `HashSet`, and `TreeSet`.

- **`Collection` Interface**: This is a root interface in the Collections Framework. It declares the essential operations that all collections will have, such as `add()`, `remove()`, `size()`, `iterator()`, etc. However, it does not provide any direct implementations of these operations.

- **Concrete Implementations**: Concrete classes like `ArrayList`, `LinkedList`, `HashSet`, and `TreeSet` implement the `Collection` interface. Each of these classes provides its own implementation of the methods defined in the interface, optimized for different use cases (e.g., `ArrayList` for fast random access, `LinkedList` for efficient insertions/deletions, `HashSet` for unique elements, and `TreeSet` for sorted unique elements).

The Open/Closed Principle is followed in this design in the following ways:

1. **Closed for Modification**: The `Collection` interface and its methods define a contract that all implementing classes agree to fulfill. This interface is closed for modifications; adding new methods to it would require changes in all implementing classes, which is not desirable.

2. **Open for Extension**: Despite being closed for modifications, the `Collection` framework is open for extension in several ways:
   - New classes can implement the `Collection` interface to create a new type of collection that adheres to the defined contract.
   - Existing classes can be extended to add new behavior. For example, you could extend `ArrayList` to create a `NotifyingArrayList` that emits events every time the collection is modified.
   - The framework can be extended with new interfaces and classes without altering the existing code. This has been seen over time with additions like the `Queue` interface and its implementations.
     The Liskov Substitution Principle (LSP) is one of the five SOLID principles of object-oriented design, introduced by Barbara Liskov in 1987. LSP formalizes a foundational concept for creating maintainable, scalable, and robust object-oriented systems. It focuses on ensuring that inheritance hierarchies are designed so that derived classes can be used in place of their base classes without disrupting the correctness of the program.

## 3. Liskov Substitution Principle: 

    "Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program."

In simpler terms, if class B is a subtype of class A, then we should be able to replace A with B without affecting the behavior of our program. This implies that B should not weaken the preconditions of A and should meet the postconditions of A, ensuring that B can stand in for A without any issues.

LSP is crucial for the following reasons:

- **Reliability**: Ensures that a subclass can stand in for its superclass without causing errors, leading to more reliable software.
- **Reusability**: Promotes the reuse of base classes and interfaces by guaranteeing that subclasses fulfill the contracts defined by their base classes.
- **Maintainability**: Facilitates easier maintenance of the codebase by ensuring that changes in subclasses do not break the expected behavior of the base class.

### Applying LSP

To adhere to the Liskov Substitution Principle:

1. **Ensure Behavioral Compatibility**: Subclasses should not only inherit the interface of their base class but also its behavior. This means implementing the methods of the base class in a way that doesn’t alter their expected behavior.

2. **Preserve Invariants**: Any rules or conditions that are true for the base class should also be true for the subclass.

3. **Avoid Weakening Preconditions**: The conditions under which a subclass method can be called should not be more restrictive than those of its base class.

4. **Avoid Strengthening Postconditions**: The conditions after a subclass method has been called should not promise more than what the base class method does.

5. **Substitute Throwability**: If a method in the base class is not supposed to throw certain exceptions, the subclass method should adhere to the same constraint.

Here's a simple Java code example that illustrates the LSP violation with the `Rectangle` and `Square` scenario described:

```java
// Base class
class Rectangle {
    private int width;
    private int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int getArea() {
        return width * height;
    }
}

// Subclass that violates LSP
class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        super.setWidth(width);
        super.setHeight(width); // Violation: Changing width changes height
    }

    @Override
    public void setHeight(int height) {
        super.setWidth(height); // Violation: Changing height changes width
        super.setHeight(height);
    }
}

public class LSPDemo {
    public static void main(String[] args) {
        Rectangle rect = new Square();
        rect.setWidth(5);
        rect.setHeight(10);

        // Expectation: area == 50, but due to LSP violation, area will be 100
        System.out.println("Expected area of 5x10 rectangle: 50");
        System.out.println("Actual area: " + rect.getArea());
    }
}
```

In this example:

- The `Rectangle` class has `setWidth()` and `setHeight()` methods that independently set the width and height of a rectangle.
- The `Square` class extends `Rectangle` but violates LSP because overriding the `setWidth()` and `setHeight()` methods to ensure the square's sides are equal leads to a situation where changing the width also changes the height and vice versa. This behavior is not expected from the base class perspective.

When a `Rectangle` reference points to a `Square` object and attempts to set the width and height to different values, the end result does not match the expectation based on the behavior defined in `Rectangle`. This demonstrates the LSP violation, where `Square` cannot be used as a substitute for `Rectangle` without altering the correctness of the program.

To adhere to LSP, one could avoid such an inheritance structure and instead use interfaces or favor composition over inheritance, ensuring that subclasses can indeed be substituted for their base class without unexpected behavior.
In the Java Collections Framework, the Liskov Substitution Principle (LSP) is followed thoroughly to ensure that subclasses of the collection interfaces can be used interchangeably without breaking the functionality. A prominent example of LSP in action within the Java Collections Framework is the relationship between the `List` interface and its implementing classes, such as `ArrayList` and `LinkedList`.

Consider the `List` interface and its methods, such as `add(E e)`, `get(int index)`, `remove(int index)`, etc. Both `ArrayList` and `LinkedList` implement the `List` interface and adhere to the contracts defined by it. This means you can substitute an `ArrayList` with a `LinkedList` in a piece of code without altering the correctness of the program from the perspective of list operations.

Here is a simple code example to illustrate this:

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class ListExample {
    public static void main(String[] args) {
        List<String> arrayList = new ArrayList<>();
        List<String> linkedList = new LinkedList<>();
        
        fillList(arrayList);
        fillList(linkedList);

        System.out.println("ArrayList contents: " + arrayList);
        System.out.println("LinkedList contents: " + linkedList);
    }

    public static void fillList(List<String> list) {
        list.add("Java");
        list.add("Python");
        list.add("C++");
    }
}
```

In this example:

- The `fillList` method accepts a `List<String>` argument and adds several strings to it. This method illustrates that you can use an `ArrayList` or a `LinkedList` (or any other class that implements the `List` interface) interchangeably without needing to change the method's implementation. The `fillList` method doesn't need to know the specific type of `List` it's working with, thanks to the LSP adherence in the design of these classes.
- Both `ArrayList` and `LinkedList` provide their own implementation of the `List` interface methods, optimized for their respective data structures, but from the perspective of a `List` user, they can be used interchangeably.

This example showcases that the `List` interface and its implementers (`ArrayList`, `LinkedList`, etc.) follow the Liskov Substitution Principle. It allows for flexibility and interoperability in code that uses the Java Collections Framework, enabling developers to choose the specific implementation that best suits their performance characteristics and requirements without altering the correctness of the program.

The "I" in SOLID principles stands for the Interface Segregation Principle (ISP), which states that no client should be forced to depend on methods it does not use. Essentially, ISP suggests that it's better to have many smaller, more specific interfaces rather than a single, do-all interface. This principle helps in minimizing the impact of changes, improving code organization, and supporting high cohesion with low coupling in software designs.

## 4. Interface Seggregation Principle: 

    ISP aims to reduce the side effects and frequency of required changes by splitting a wide set of actions into smaller and more specific ones. The principle encourages designing interfaces that are client-specific rather than general-purpose, so clients will only know about the methods that are of interest to them.

### Importance

- **Minimizes Code Reusability Issues**: By adhering to ISP, systems become more flexible and adaptable to change. Clients can implement only the interfaces their need, reducing the dependencies on unused code.
- **Enhances System Maintainability**: Smaller, well-defined interfaces are easier to implement, test, and maintain over time.
- **Supports High Cohesion**: Ensures that modules or components have tightly related and focused functionalities.
- **Encourages Decoupling**: Clients are less likely to be affected by changes in unrelated system parts, leading to systems that are easier to refactor, change, and understand.

### Applying ISP

To apply ISP effectively:

1. **Identify the Clients**: Understand who or what will be using the interface. A client can be another class or module in the system.
2. **Define Specific Interfaces**: Based on the client's needs, define interfaces that contain only the methods required by the client.
3. **Implement Interfaces**: Classes should implement these specific interfaces, ensuring that they don't carry additional unnecessary methods that aren't of use to the client.

### Comsider Below example

Consider an all-in-one interface for a smart device:

```java
interface SmartDevice {
    void print();
    void fax();
    void scan();
}
```

This design forces all implementing classes to define methods for `print`, `fax`, and `scan`, even if a specific device does not support all these functions. This violates ISP.

A better approach following ISP would be to segregate the `SmartDevice` interface into more specific interfaces:

```java
interface Printer {
    void print();
}

interface Fax {
    void fax();
}

interface Scanner {
    void scan();
}
```

Now, a device that only supports printing need only implement the `Printer` interface:

```java
class SimplePrinter implements Printer {
    public void print() {
        // Implementation for print
    }
}
```

And a multifunctional device could implement multiple interfaces:

```java
class MultiFunctionalPrinter implements Printer, Scanner, Fax {
    public void print() {
        // Implementation for print
    }

    public void scan() {
        // Implementation for scan
    }

    public void fax() {
        // Implementation for fax
    }
}
```
In the Java Collections Framework, the Interface Segregation Principle (ISP) is inherently applied through its design, which favors specific interfaces for specific purposes rather than a single, monolithic interface for all collection types. This design allows for flexibility and ensures that implementing classes only need to provide functionality relevant to their specific type of collection, adhering to the ISP.

### Example of ISP in Java Collections Framework

Consider how Java's Collection Framework segregates interfaces for different collection types:

- **`Collection` Interface**: The root interface in the Collections Framework hierarchy. It represents a group of objects, known as its elements. This interface is at the top level and is implemented by various other collection interfaces like `List`, `Set`, and `Queue`.

- **`List` Interface**: An ordered collection (also known as a sequence). Lists can contain duplicate elements. The user can access elements by their integer index (position in the list), and search for elements in the list.

- **`Set` Interface**: A collection that cannot contain duplicate elements. It models the mathematical set abstraction and is used to represent sets, such as the deck of cards.

- **`Queue` Interface**: A collection used to hold multiple elements prior to processing. Besides basic Collection operations, queues provide additional insertion, extraction, and inspection operations. Queues typically, but not necessarily, order elements in a FIFO (first-in-first-out) manner.

Each of these interfaces targets a specific type of collection with specific behaviors and properties, adhering to the Interface Segregation Principle. Clients can use these interfaces without being forced to implement methods that are irrelevant to the type of collection they are interested in.

### Applying ISP in Practice

Here's a simplified example of how you might apply ISP when working with these interfaces:

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.LinkedList;
import java.util.Queue;

public class CollectionExample {
    public static void main(String[] args) {
        // List example
        ArrayList<String> arrayList = new ArrayList<>();
        arrayList.add("Item 1");
        arrayList.add("Item 2");
        System.out.println("ArrayList: " + arrayList);

        // Set example
        HashSet<String> hashSet = new HashSet<>();
        hashSet.add("Item 1");
        hashSet.add("Item 1");
        System.out.println("HashSet (duplicates not allowed): " + hashSet);

        // Queue example
        Queue<String> queue = new LinkedList<>();
        queue.offer("First");
        queue.offer("Second");
        System.out.println("Queue: " + queue);
    }
}
```

In this example:

- `ArrayList` implements the `List` interface, providing an ordered, index-accessible collection that allows duplicates.
- `HashSet` implements the `Set` interface, providing a collection that ensures uniqueness of its elements.
- `LinkedList` implements both the `List` and `Queue` interfaces, showing how a class can serve multiple collection roles, thanks to the segregation of interfaces in the Collections Framework.

This approach adheres to ISP by ensuring that a class is not forced to implement interfaces and methods that it does not use.

## 5. Dependency Inversion Principle: 

     DIP is a crucial principle in software design aimed at reducing dependencies between high-level modules (which provide complex logic) and low-level modules (which provide utility features or basic operations) by introducing an abstraction layer.

### Understanding High-Level and Low-Level Modules

- **High-Level Modules** are classes or components that implement business rules or logic. They operate on a higher level of abstraction, orchestrating the behavior of the application.
- **Low-Level Modules** are the workhorses that perform specific operations, like database access, network communication, or device control. They are detailed implementations that high-level modules rely on to function.

### The Problem DIP Addresses

Without DIP, high-level modules directly depend on low-level modules, making the system rigid, difficult to change, and hard to test. For example, if a high-level module `OrderProcessor` directly creates and uses a low-level module `MySQLDatabase` for data persistence, it's tightly coupled to that specific implementation. If you decide to switch databases, you'd need to modify the `OrderProcessor`, violating the open/closed principle.

### The DIP Solution

DIP solves this problem by inverting the traditional dependency relationship:

1. **Both Should Depend on Abstractions**: Instead of high-level modules depending on low-level modules, both should depend on abstractions. An abstraction is a layer that provides a general interface for interaction, such as interfaces or abstract classes in Java.
   
2. **Abstractions Should Not Depend on Details**: Interfaces or abstract classes should not be tailored to specific implementations but should define a general contract that any implementation can fulfill.

### How to Apply DIP

1. **Define Interfaces or Abstract Classes**: Identify what operations high-level modules need from low-level modules and define these operations in interfaces or abstract classes. This step creates the abstraction layer.
   
2. **Implement Interfaces for Low-Level Modules**: Create concrete implementations of the interfaces for your low-level modules. These are the specific details that fulfill the contract defined by the abstraction.

3. **Inject Dependencies**: High-level modules should be designed to accept any implementation of the interface they depend on. This is typically achieved through dependency injection, where the specific instances of the low-level modules are provided to the high-level modules (e.g., via a constructor, method parameter, or using a dependency injection framework).

### Detailed Example

Let's expand on the `Button` and `Lamp` example with a focus on dependency injection for better clarity:

```java
// Step 1: Define an interface as an abstraction layer
interface Device {
    void turnOn();
    void turnOff();
}

// Step 2: Implement the interface with a low-level module
class Lamp implements Device {
    @Override
    public void turnOn() {
        System.out.println("Lamp is now on.");
    }

    @Override
    public void turnOff() {
        System.out.println("Lamp is now off.");
    }
}

// Another low-level module
class Fan implements Device {
    @Override
    public void turnOn() {
        System.out.println("Fan is now on.");
    }

    @Override
    public void turnOff() {
        System.out.println("Fan is now off.");
    }
}

// Step 3: High-level module that depends on the abstraction (interface)
class RemoteControl {
    private Device device;

    // Dependency injection via constructor
    public RemoteControl(Device device) {
        this.device = device;
    }

    public void togglePower() {
        if (Math.random() > 0.5) {
            device.turnOn();
        } else {
            device.turnOff();
        }
    }
}

// Demonstrating DIP with dependency injection
public class HomeAutomation {
    public static void main(String[] args) {
        Device lamp = new Lamp();
        Device fan = new Fan();

        RemoteControl controlForLamp = new RemoteControl(lamp);
        RemoteControl controlForFan = new RemoteControl(fan);

        controlForLamp.togglePower();
        controlForFan.togglePower();
    }
}
```

In this example:

- `Device` is the abstraction that both high-level and low-level modules depend on.
- `Lamp` and `Fan` are low-level modules that implement the `Device` interface. They can be replaced or extended without affecting the high-level module (`RemoteControl`).
- `RemoteControl` is a high-level module that operates on the `Device` abstraction. It's designed to control any device, demonstrating DIP by depending on an abstraction rather than concrete implementations.

### Scenario: Data Processing and Storage

Imagine you have a high-level module responsible for processing data and then storing it. Without applying DIP, this module might directly depend on a specific collection implementation (e.g., `ArrayList`) for its storage needs. However, this direct dependency makes the module less flexible and more tightly coupled to a specific implementation of the Java Collections Framework.

### Step 1: Define an Interface as an Abstraction

First, define an interface that abstracts the concept of data storage. This interface declares methods for adding and retrieving data, without specifying the underlying storage mechanism.

```java
interface DataStorage<T> {
    void add(T item);
    T get(int index);
}
```

### Step 2: Implement the Interface with Specific Collections

Next, create concrete implementations of the `DataStorage` interface using different Java Collections. These implementations are the low-level modules in this context.

```java
class ListDataStorage<T> implements DataStorage<T> {
    private List<T> list = new ArrayList<>();

    @Override
    public void add(T item) {
        list.add(item);
    }

    @Override
    public T get(int index) {
        return list.get(index);
    }
}

class SetDataStorage<T> implements DataStorage<T> {
    private Set<T> set = new HashSet<>();
    private List<T> list = new ArrayList<>();

    @Override
    public void add(T item) {
        if (set.add(item)) { // Add item to set to ensure uniqueness, and list for retrieval
            list.add(item);
        }
    }

    @Override
    public T get(int index) {
        return list.get(index);
    }
}
```

### Step 3: High-Level Module That Depends on the Abstraction

Create a high-level module, such as a data processor, that operates on the `DataStorage` abstraction. This module is designed to work with any data storage implementation that conforms to the `DataStorage` interface.

```java
class DataProcessor<T> {
    private DataStorage<T> storage;

    public DataProcessor(DataStorage<T> storage) {
        this.storage = storage;
    }

    public void processData(T data) {
        // Process data (details omitted for brevity)
        storage.add(data);
    }

    public T retrieveData(int index) {
        return storage.get(index);
    }
}
```

### Demonstrating Dependency Inversion

```java
public class Application {
    public static void main(String[] args) {
        DataStorage<String> listStorage = new ListDataStorage<>();
        DataStorage<String> setStorage = new SetDataStorage<>();

        DataProcessor<String> processorUsingList = new DataProcessor<>(listStorage);
        DataProcessor<String> processorUsingSet = new DataProcessor<>(setStorage);

        processorUsingList.processData("Data1");
        processorUsingList.processData("Data2");

        processorUsingSet.processData("Data1");
        processorUsingSet.processData("Data1"); // Duplicate, will not be added in set

        System.out.println(processorUsingList.retrieveData(0)); // Outputs: Data1
        System.out.println(processorUsingSet.retrieveData(0)); // Outputs: Data1
    }
}
```

In this example:

- `DataStorage<T>` serves as an abstraction for different ways of storing data, allowing the `DataProcessor` (a high-level module) to depend on an abstraction rather than concrete implementations.
- `ListDataStorage<T>` and `SetDataStorage<T>` are concrete implementations of the `DataStorage` interface, showing how low-level




