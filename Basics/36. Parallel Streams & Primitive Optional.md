# Parallel Streams & Primitive Optional

## 1. Sequential vs Parallel Streams

A sequential stream processes elements through a pipeline in a single thread.

```java
numbers.stream()
        .map(n -> n * 2)
        .forEach(System.out::println);
```

A parallel stream divides the workload into smaller tasks that can be processed concurrently.

```java
numbers.parallelStream()
        .map(n -> n * 2)
        .forEach(System.out::println);
```

Parallel streams use Java's Fork/Join framework and commonly use the shared `ForkJoinPool.commonPool()`.

**Important:** Parallel streams do not guarantee that elements will execute in encounter order or on a particular thread.

## 2. How Parallel Streams Work

Parallel stream processing broadly involves:

1. **Splitting:** The source is divided into smaller parts.
2. **Processing:** Different tasks process the parts concurrently.
3. **Combining:** Partial results are combined into a final result.

The Stream API uses a `Spliterator` to traverse and split the source.

### Spliterator

`Spliterator` stands for *Splitable Iterator*.

It supports:
- `tryAdvance()` — processes one element if available.
- `forEachRemaining()` — processes remaining elements.
- `trySplit()` — attempts to split off a portion for separate processing.
- `estimateSize()` — estimates remaining elements.

This splitting capability helps parallel streams distribute work.

The Fork/Join framework uses tasks to recursively divide work and combine results.

## 3. Parallel Stream Example

```java
int sum = IntStream.rangeClosed(1, 1_000_000)
        .parallel()
        .sum();
```

The stream can divide the range into smaller parts, calculate partial sums concurrently, and combine them.

Parallel processing is most useful when:
- The dataset is sufficiently large.
- Operations are CPU-intensive.
- Tasks are independent.
- The source can be split efficiently.
- Combining partial results is relatively inexpensive.

## 4. When Parallel Streams May Be Slower

Parallel processing introduces overhead:
- Task creation and scheduling.
- Splitting the source.
- Combining results.
- Thread coordination and contention.

It may be slower when:
- The dataset is small.
- Each operation is inexpensive.
- The source is difficult to split efficiently.
- The workload involves blocking I/O.
- Shared resources cause contention.
- Ordering constraints limit parallel execution.

**Do not assume that parallel streams are automatically faster.** Measure performance with representative workloads before choosing them.

## 5. Stateful Operations

Some intermediate operations need information about multiple elements or the entire input.

Examples:
- `sorted()`
- `distinct()`

These are stateful intermediate operations and can require coordination or buffering.

They can reduce the benefits of parallel processing, although they are not automatically slower in every scenario.

## 6. Ordering in Parallel Streams

Parallel execution does not necessarily preserve the order in which actions are performed.

```java
numbers.parallelStream()
        .forEach(System.out::println);
```

Output order is not guaranteed.

Use `forEachOrdered()` when encounter order must be preserved:

```java
numbers.parallelStream()
        .forEachOrdered(System.out::println);
```

This can reduce parallelism because ordered processing may require coordination.

Also distinguish:
- **Encounter order:** The logical order defined by the source or pipeline.
- **Execution order:** The order in which threads actually process elements.

They are not necessarily the same.

## 7. Shared Mutable State and Race Conditions

Avoid modifying shared mutable resources from parallel stream operations.

Unsafe example:

```java
List<Integer> result = new ArrayList<>();

numbers.parallelStream()
        .forEach(result::add);
```

Multiple threads may modify the same `ArrayList` concurrently. This can cause race conditions and incorrect results.

Prefer a reduction or collector:

```java
List<Integer> result = numbers.parallelStream()
        .filter(n -> n % 2 == 0)
        .toList();
```

Or:

```java
List<Integer> result = numbers.parallelStream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

Collectors are designed to support reduction. A correctly implemented collector can use separate intermediate containers and combine them safely.

**Interview point:** Avoid side effects in stream pipelines, especially parallel ones. Prefer stateless operations and reduction.

## 8. Parallel Stream Best Practices

- Use parallel streams only when there is a measurable performance benefit.
- Prefer independent operations.
- Avoid shared mutable state.
- Avoid blocking operations in the common Fork/Join pool.
- Consider source splitting efficiency.
- Account for the cost of combining results.
- Use `forEachOrdered()` only when ordering is required.
- Do not assume a parallel stream creates a dedicated thread pool for each pipeline.

## 9. Primitive Optional Classes

Regular `Optional<T>` works with reference types.

For primitive results, Java provides specialized Optional classes to avoid boxing.

| Class | Represents |
|---|---|
| `OptionalInt` | Optional int |
| `OptionalLong` | Optional long |
| `OptionalDouble` | Optional double |

Example:

```java
OptionalInt max = IntStream.of(10, 20, 30).max();

if (max.isPresent()) {
    System.out.println(max.getAsInt());
}
```

Other examples:

```java
OptionalLong count = LongStream.of(10L, 20L, 30L)
        .filter(n -> n > 10)
        .count();

OptionalDouble average = IntStream.of(10, 20, 30)
        .average();
```

### Common Methods

| Method | OptionalInt | OptionalLong | OptionalDouble |
|---|---|---|---|
| `isPresent()` | Yes | Yes | Yes |
| `isEmpty()` | Yes (Java 11+) | Yes (Java 11+) | Yes (Java 11+) |
| `getAsInt()` | Yes | — | — |
| `getAsLong()` | — | Yes | — |
| `getAsDouble()` | — | — | Yes |
| `orElse()` | Yes | Yes | Yes |
| `orElseGet()` | Yes | Yes | Yes |
| `orElseThrow()` | Yes | Yes | Yes |

These classes provide primitive-specific accessors instead of returning wrapper objects.

## 10. Interview Quick Revision

**How do parallel streams work?**

They split the source into smaller tasks using Spliterator and process tasks concurrently, commonly using the Fork/Join framework.

**What is Spliterator?**

An interface that supports traversal and splitting of elements, enabling parallel processing.

**Are parallel streams always faster?**

No. Parallelism introduces overhead. Performance depends on data size, operation cost, source characteristics, contention, and result-combination cost.

**Which operations can be expensive in parallel pipelines?**

Stateful operations such as `sorted()` and `distinct()` can require buffering or coordination.

**Why is modifying an ArrayList inside parallel forEach unsafe?**

Multiple threads may modify the same non-thread-safe collection concurrently, causing race conditions.

**How can shared mutable state be avoided?**

Prefer stateless transformations and reduction operations such as `collect()` or `toList()`.

**What is the difference between forEach() and forEachOrdered()?**

`forEach()` does not guarantee encounter order in a parallel stream. `forEachOrdered()` preserves encounter order when one exists.

**Why use OptionalInt instead of Optional<Integer>?**

It avoids boxing the primitive result into an `Integer`.

---

### Quick Revision

- `parallelStream()` enables parallel processing.
- Parallel streams commonly use the Fork/Join common pool.
- `Spliterator` supports traversal and splitting.
- Parallelism is not automatically faster.
- Avoid shared mutable state.
- `sorted()` and `distinct()` are stateful operations.
- `forEachOrdered()` preserves encounter order.
- `OptionalInt`, `OptionalLong`, `OptionalDouble` avoid primitive boxing.
