# Divide and Conquer

[CLRS — Introduction to Algorithms, Chapter 4](https://mitpress.mit.edu/9780262046305/) | [Divide and Conquer — Khan Academy](https://www.khanacademy.org/computing/computer-science/algorithms/merge-sort/a/divide-and-conquer-algorithms)

Divide and conquer is a fundamental problem-solving strategy: break a large problem into smaller independent subproblems, solve each, then combine the results. It appears at every level of computing — algorithms, data systems, and distributed architecture.

---

## The general structure

```
Large Problem
      ↓ divide
┌─────┴─────┐
Sub A      Sub B
  ↓            ↓
solve        solve
  ↓            ↓
Result A   Result B
      ↓ combine
   Final Result
```

Three steps: **divide** → **conquer** → **combine**.

The power comes from the divide step: if each division halves the problem, complexity drops from O(n²) to O(n log n) or O(log n).

---

## 1. Algorithmic divide and conquer

### Merge Sort — O(n log n)
Recursively split the array in half until single elements remain, then merge back in sorted order.

```
[8, 3, 5, 1, 4, 2]
        ↓ divide
[8, 3, 5]      [1, 4, 2]
      ↓                ↓
[8][3][5]        [1][4][2]
      ↓ merge           ↓ merge
  [3, 5, 8]       [1, 2, 4]
           ↓ combine
      [1, 2, 3, 4, 5, 8]
```

### Binary Search — O(log n)
Divide the sorted search space in half each step — discard the half that cannot contain the target.

```kotlin
fun binarySearch(arr: IntArray, target: Int): Int {
    var low = 0
    var high = arr.lastIndex
    while (low <= high) {
        val mid = (low + high) / 2
        when {
            arr[mid] == target -> return mid
            arr[mid] < target  -> low = mid + 1    // discard left half
            else               -> high = mid - 1   // discard right half
        }
    }
    return -1
}
```

1 billion elements → at most 30 comparisons.

### Quick Sort — average O(n log n)
Pick a pivot, partition into "smaller" and "larger", recursively sort each partition.

### Other examples
| Algorithm | Domain | Complexity gain |
|---|---|---|
| Fast Fourier Transform (FFT) | Signal processing, audio/image compression | O(n²) → O(n log n) |
| Karatsuba multiplication | Large integer arithmetic | O(n²) → O(n^1.585) |
| Strassen matrix multiplication | Linear algebra | Reduces matrix multiply exponent |

---

## 2. Data divide and conquer

### [[Database Partitioning]] / Sharding
Split a large dataset across multiple nodes — each node conquers its partition locally. Cross-partition queries combine results from all partitions.

### MapReduce
Divide-and-conquer for massive datasets across thousands of machines:

```
Input (huge dataset)
      ↓ Map — parallel, each machine processes its chunk
Key-Value pairs per chunk
      ↓ Shuffle — group by key across machines
      ↓ Reduce — combine values per key
Final aggregated result
```

Example — count word frequency across 1 TB of text:
- **Map:** each machine emits `("word", 1)` for every word in its chunk
- **Reduce:** sum all counts per word

Used by Hadoop, Apache Spark, Google BigQuery.

---

## 3. System-level divide and conquer

### [[Load Balancer]]
Divide incoming requests across multiple servers — each server conquers its share independently.

### CDN (Content Delivery Network)
Divide content delivery geographically — each edge node conquers requests from its region. Users reach the nearest node, not a central server.

### DNS resolution
Hierarchical divide and conquer: root servers → TLD servers (.com, .de) → authoritative servers → final address.

### Microservices decomposition
Break a monolith into services by domain — each service owns and conquers its bounded problem. The [[Strangler Fig]] pattern is divide and conquer applied to migration.

---

## 4. Concurrency divide and conquer

Split computation across threads or coroutines:

```kotlin
// Kotlin coroutines — parallel divide and conquer
val result = (1..1_000_000)
    .chunked(100_000)                   // divide into 10 chunks
    .map { chunk -> async { chunk.sum() } }  // conquer each in parallel
    .awaitAll()
    .sum()                              // combine
```

**Fork/Join** (Java) is the standard framework for recursive parallel divide and conquer.

---

## Why it is powerful

| Property | Effect |
|---|---|
| Reduces complexity | O(n²) → O(n log n) by halving at each step |
| Enables parallelism | Independent subproblems run simultaneously |
| Scales horizontally | Add machines to handle more subproblems |
| Isolates failures | One subproblem failing doesn't break others |

---

## Divide and conquer at every scale

| Scale | Example |
|---|---|
| Single function | Binary search, merge sort |
| Thread / coroutine | Parallel chunk processing |
| Service | Microservices decomposition |
| Data layer | Sharding, [[Database Partitioning]] |
| Infrastructure | [[Load Balancer]], CDN, MapReduce |

---

## Related Topics

- [[Database Partitioning]] — data-level divide and conquer; each partition is conquered independently
- [[Load Balancer]] — infrastructure-level divide and conquer across server instances
- [[Strangler Fig]] — applies divide and conquer to legacy system migration
- [[Messaging Patterns]] — fan-out is divide and conquer applied to event processing
