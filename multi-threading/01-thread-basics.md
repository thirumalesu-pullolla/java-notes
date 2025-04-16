# 🧵 Java Multithreading - Step 1: Thread Creation Basics

---

## ✅ What is Multithreading?

- **Multithreading** is a programming technique where multiple threads run **concurrently** within a single process to perform tasks independently.
- Java supports multithreading using the `java.lang.Thread` class and the `Runnable` interface.

---

## 🛠️ Ways to Create Threads in Java

### 1. **Extending the Thread Class**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // starts new thread
    }
}
```

---

### 2. **Implementing the Runnable Interface**
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Runnable running...");
    }
}

public class Test {
    public static void main(String[] args) {
        MyRunnable runnable = new MyRunnable();
        Thread t1 = new Thread(runnable);
        t1.start();
    }
}
```

---

### 3. **Using Lambda Expressions (Java 8+)**
```java
public class LambdaThread {
    public static void main(String[] args) {
        Thread t = new Thread(() -> System.out.println("Thread running via lambda"));
        t.start();
    }
}
```

---

## 🔀 Difference Between `start()` and `run()`

| Method | Description |
|--------|-------------|
| `start()` | Creates a new thread and executes `run()` in a separate call stack |
| `run()` | Executes in the current thread like a normal method |

---

## 🌟 MCQs (Quiz)

### Q1. What happens if you call the `run()` method directly instead of `start()`?  
**Answer:** B. It runs in the current thread (no multithreading)

---

### Q2. Which is a correct way to create a thread in Java?  
**Answer:** C. `Thread t = new Thread(() -> {}); t.start();`

---

### Q3. What's the output of this code?
```java
public class Test {
    public static void main(String[] args) {
        Thread t = new Thread(() -> System.out.println("Thread running"));
        t.run();
        System.out.println("Main finished");
    }
}
```
**Answer:** B. "Thread running" followed by "Main finished"

---

## 💬 Interview Questions

### Q4. What's the difference between extending Thread vs implementing Runnable?
- Prefer **Runnable** because:
  - Java supports single inheritance.
  - Runnable is a **functional interface** (Java 8+).
  - Promotes **separation of task & thread management**.

---

### Q5. Can two threads run the same `Runnable` instance?
**Yes**, and they share the same instance variables (data).  
Example:
```java
class CounterTask implements Runnable {
    int count = 0;

    public void run() {
        count++;
        System.out.println(Thread.currentThread().getName() + " Count: " + count);
    }
}
```
Used with:
```java
Runnable task = new CounterTask();
new Thread(task).start();
new Thread(task).start();
```

**Implication:** Shared data → risk of race conditions if not synchronized.

---

### Q6. What are the benefits of using lambda expressions with threads?
- Cleaner, functional syntax
- No need for extra classes
- Better readability for short tasks

---

## 🔧 Real-World Coding Challenges

---

### ✅ Challenge 1: Simulate a Coffee Shop

**Goal:**  
- One thread takes orders (prints 5 messages).  
- Another thread prepares coffee (prints 5 messages).  
- Both run **concurrently**.

```java
class Orders extends Thread {
    public void run() {
        for(int i = 1; i <= 5; i++) {
            System.out.println("Taking order #" + i + " from the customer.");
        }
    }
}

class PreparingCoffee extends Thread {
    public void run() {
        for(int i = 1; i <= 5; i++) {
            System.out.println("Preparing coffee #" + i + "... please wait.");
        }
    }
}

public class CoffeeShop {
    public static void main(String[] args) {
        Orders ordersThread = new Orders();
        PreparingCoffee prepThread = new PreparingCoffee();
        ordersThread.start();
        prepThread.start();
    }
}
```

---

### ✅ Challenge 2: Simulate a Printer

**Goal:**  
- Create a `Printer` class with `printDocument(String docName)`  
- Print 3 documents using 3 threads

```java
public class Printer {
    public void printDocument(String docName) {
        System.out.println(Thread.currentThread().getName() + " is printing: " + docName);
    }
}

class PrintJob implements Runnable {
    private Printer printer;
    private String docName;

    public PrintJob(Printer printer, String docName) {
        this.printer = printer;
        this.docName = docName;
    }

    public void run() {
        printer.printDocument(docName);
    }
}

public class PrintSimulation {
    public static void main(String[] args) {
        Printer sharedPrinter = new Printer();

        Thread t1 = new Thread(new PrintJob(sharedPrinter, "Invoice.pdf"), "Thread-1");
        Thread t2 = new Thread(new PrintJob(sharedPrinter, "Report.docx"), "Thread-2");
        Thread t3 = new Thread(new PrintJob(sharedPrinter, "Resume.pdf"), "Thread-3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

---

### Bonus: Thread Safety Improvement
To make `printDocument()` thread-safe:
```java
public synchronized void printDocument(String docName) {
    System.out.println(Thread.currentThread().getName() + " is printing: " + docName);
}
```

---

