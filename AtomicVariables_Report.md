
# AtomicVariables_Report.md

## 1. Atomic Variables

### Code Snippet

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicDemo {
  private static AtomicInteger atomicCounter = new AtomicInteger(0);
  private static int normalCounter = 0;

  public static void main(String[] args) throws InterruptedException {
    Runnable task = () -> {
      for (int i = 0; i < 1_000_000; i++) {
        atomicCounter.incrementAndGet();
        normalCounter++;
      }
    };

    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);
    t1.start();
    t2.start();
    t1.join();
    t2.join();

    System.out.println("Atomic Counter: " + atomicCounter);
    System.out.println("Normal Counter: " + normalCounter);
  }
}
```

---

### **Q1: What output do you get from the program? Why?**

**Expected Output (approximate):**
```
Atomic Counter: 2000000
Normal Counter: <Less than 2000000, unpredictable>
```

**Explanation:**
- `atomicCounter` correctly reaches `2,000,000` because `AtomicInteger` provides **atomic operations** that are safe in multi-threaded environments.
- `normalCounter` will likely show a value **less than 2,000,000** due to **race conditions**. Multiple threads may read, increment, and write `normalCounter` simultaneously, causing increments to be lost.

---

### **Q2: What is the purpose of AtomicInteger in this code?**

The purpose of `AtomicInteger` is to provide **atomic (thread-safe) operations** on integers without requiring explicit synchronization (like `synchronized` blocks or locks). It ensures that the `incrementAndGet()` method is executed atomically across threads, preventing race conditions.

---

### **Q3: What thread-safety guarantees does `atomicCounter.incrementAndGet()` provide?**

`incrementAndGet()` guarantees that:
- The read, increment, and write operations happen as a **single atomic action**.
- No other thread can interfere with the operation during its execution.
- It uses **low-level compare-and-swap (CAS)** operations under the hood, ensuring lock-free thread-safe behavior.

---

### **Q4: In which situations would using a lock be a better choice than an atomic variable?**

Locks are preferable when:
- **Multiple shared variables** must be updated together atomically (atomic classes operate on single variables only).
- **Complex critical sections** need to be executed by one thread at a time.
- You need to manage **non-primitive shared state** (e.g., collections, objects with multiple fields).
- You need **fairness**, reentrancy, or interruptibility, which locks (like `ReentrantLock`) provide.

---

### **Q5: Besides `AtomicInteger`, what other data types are available in the `java.util.concurrent.atomic` package?**

- **AtomicBoolean** – For atomic operations on boolean values.
- **AtomicLong** – For atomic operations on `long` values.
- **AtomicIntegerArray / AtomicLongArray / AtomicReferenceArray** – Atomic arrays.
- **AtomicReference<T>** – For atomic operations on object references.
- **AtomicStampedReference<T>** – To solve the **ABA problem** by using version stamps.
- **AtomicMarkableReference<T>** – Similar to `AtomicStampedReference` but with a boolean marker.
- **DoubleAccumulator / LongAccumulator** – For more complex atomic accumulation operations.
- **DoubleAdder / LongAdder** – High-performance atomics for accumulating values, especially under high contention.
