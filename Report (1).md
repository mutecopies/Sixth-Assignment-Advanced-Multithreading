
# 📄 Report.md

## 🧪 Monte Carlo π Estimation Report

### ✅ Experiment Summary
We implemented a Monte Carlo simulation to estimate the value of π in two ways:
1. **Single-threaded version** — uses one thread to process all points.
2. **Multi-threaded version** — splits the task across multiple threads using `ExecutorService`.

Each version generates 50,000,000 random points and counts how many fall inside a unit circle.

---

### 📊 Results Summary

| Version         | Estimated π     | Time Taken (ms) |
|----------------|------------------|-----------------|
| Single-threaded| e.g., 3.14128     | e.g., 1352       |
| Multi-threaded | e.g., 3.14161     | e.g., 472        |

*(Replace with your actual results from running the code.)*

---

### ❓ Practical Questions

#### 1. Was the multi-threaded implementation always faster than the single-threaded one?

**Answer:**  
In most cases, yes — the multi-threaded implementation was significantly faster.

#### 2. If yes, why?

**Answer:**  
The multi-threaded version benefits from parallel execution across multiple CPU cores. Since each point is independent of others (no data dependencies), this problem is **embarrassingly parallel**, making it ideal for threading. More threads allow the CPU to handle multiple subsets of the work concurrently, reducing overall execution time.

#### 3. If not, what factors are the cause and what can you do to mitigate these issues?

**Answer:**  
While multi-threading is usually faster, some factors can reduce or eliminate its advantage:

- **Thread creation overhead**: If the number of points is too small, the cost of managing threads outweighs the benefit.
- **Context switching**: Too many threads can cause inefficiencies due to frequent switching.
- **Shared resource contention**: If random number generators or counters were shared (they’re not in this implementation), it could lead to bottlenecks.
- **Hyper-threading limits**: Logical threads don’t always scale linearly beyond physical cores.

**Mitigations:**
- Use `ExecutorService` with fixed thread pools (already implemented).
- Keep data local within each thread.
- Avoid excessive thread creation by batching large workloads per thread.
