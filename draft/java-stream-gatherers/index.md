# Java Stream Gatherers: Unlocking the Missing Piece from Built-ins to Custom Pipelines

## Table of Contents

* [1. Introduction & Context](#1-introduction--context)
  * [1.1 The Evolution of Streams](#11-the-evolution-of-streams)
  * [1.2 The "Collector Problem"](#12-the-collector-problem)
  * [1.3 What are Stream Gatherers?](#13-what-are-stream-gatherers)
* [2. Getting Started: Built-In Gatherers in Action](#2-getting-started-built-in-gatherers-in-action)
  * [2.1 Understanding Stream Cardinality](#21-understanding-stream-cardinality)
  * [2.2 The Five Ready-to-Use Gatherers](#22-the-five-ready-to-use-gatherers)
    * [2.2.1 Batching Items with `windowFixed`](#221-batching-items-with-windowfixed)
    * [2.2.2 Analyzing Sequences with `windowSliding`](#222-analyzing-sequences-with-windowsliding)
    * [2.2.3 Accumulating Values with `scan`](#223-accumulating-values-with-scan)
    * [2.2.4 Intermediate Aggregations with `fold`](#224-intermediate-aggregations-with-fold)
    * [2.2.5 Bounded Parallelism with `mapConcurrent`](#225-bounded-parallelism-with-mapconcurrent)
* [3. Leveling Up: From Using Gatherers to Authoring Your Own](#3-leveling-up-from-using-gatherers-to-authoring-your-own)
  * [3.1 When Should You Write a Custom Gatherer?](#31-when-should-you-write-a-custom-gatherer)
  * [3.2 Pipeline Composition with `andThen()`](#32-pipeline-composition-with-andthen)
* [4. Under the Hood: Architecture & Anatomy of Custom Gatherers](#4-under-the-hood-architecture--anatomy-of-custom-gatherers)
  * [4.1 Anatomy of a `Gatherer<T, A, R>`](#41-anatomy-of-a-gatherert-a-r)
  * [4.2 The Four Building Blocks (`initializer`, `integrator`, `finisher`, `combiner`)](#42-the-four-building-blocks-initializer-integrator-finisher-combiner)
  * [4.3 Concrete Walkthrough: Building a Custom Gatherer](#43-concrete-walkthrough-building-a-custom-gatherer)
* [5. Production-Ready Gatherers: Parallelism, State, and Pitfalls](#5-production-ready-gatherers-parallelism-state-and-pitfalls)
  * [5.1 Parallel Execution Modes & Combiners](#51-parallel-execution-modes--combiners)
  * [5.2 Greedy vs. Short-Circuiting Integrators](#52-greedy-vs-short-circuiting-integrators)
  * [5.3 State Management & Thread Safety](#53-state-management--thread-safety)
  * [5.4 Gatherer vs. Collector: A Quick Comparison](#54-gatherer-vs-collector-a-quick-comparison)
* [6. Conclusion & Summary](#6-conclusion--summary)

---

### 1. Introduction & Context
#### 1.1 The Evolution of Streams

In my previous article, I have already covered the evolution of the [Streams API](https://foojay.io/today/java-demystifying-the-stream-api-part-3/) 

In this article, we will delve into Java's Streams Gatherers feature, elucidating its benefits to developers. This feature facilitates the creation of custom operations and enables
data transformation concurrently with built-in operations such as `map`, `filter`, and `reduce`

#### 1.2 The "Collector Problem"

As a Java developer, we often encounter challenges when dealing with intricate nested-collectors and multi-step operations. Writing such code can result in verbose and complex code.
To address this issue, we can leverage the new capability of creating custom intermediate operations.

#### 1.3 What are Stream Gatherers?
**Stream Gatherers** is a novel capability introduced **[JEP 461 - Streams Gatherers](https://openjdk.org/jeps/461)** as a preview feature and finalized in JDK 24 via **[JEP 485](https://openjdk.org/jeps/485). It enables the creation of custom intermediate operations,
providing flexibility in transforming data within stream pipelines in ways that are not readily achievable through existing built-in intermediate operations.

### 2. Getting Started: Built-In Gatherers in Action
#### 2.1 Understanding Stream Cardinality
Prior to the introduction of Java Stream Gatherers, every intermediate operation within the Stream API was governed by stringent "cardinality rules," which defined the mathematical relationship
between the number of elements entering an operation and the number of elements exiting it.

To elucidate the transformative impact of Stream Gatherers, let us examine how standard intermediate operations manage cardinality:

- **1 to 1 (`map`)**: Each input element undergoes a transformation resulting in precisely one output element.
  - _Example_: Converting a stream of strings to lowercase or uppercase, along with their respective lengths `stream.map(String::length)` or `stream.map(String::toUpperCase)`
- **1 to 0 or 1 (`filter`)**: Each input element yields at most one output element (either it posses through or is discarded)
  - _Example_: Retaining only even numbers `stream.filter(n -> n % 2 == 0)`
- **1 to Many (`flatMap`)**: Each input element can generate zero, one, or multiple output elements, which are subsequently flattened into a continuous stream.
   - _Example_:  Splitting a stream of sentences into individual words (`stream.flatMap(sentence -> Arrays.stream(sentence.split(" "))`).

While most of the built-in intermediate operations cover a predominantly majority of day-to-day data transformations, they come with a significant limitation: they are stateless and operate independently on each element. When our business logic requires many-to-many relationships or stateful grouping across multiple elements, such as:

- Grouping items into fixed-size batches of 10 for bulk database inserts
- Calculating a sliding window moving average of financial stock prices
- Comparing an element to its predecessor or successor
- Running a cumulative sum or rolling balance

Most developers resort to using Nested Collectors, Map, Transform, or Overusing Collectors, which leads to code verbosity.

Stream gatherers bridge this exact gap, enabling intermediate operations to maintain private state, buffer elements, and emit custom chunks of data down the pipeline—all while maintaining clean, lazy, and functionally pure streams.

#### 2.2 The Five Ready-to-Use Gatherers
`java.util.stream.Gatherers` is a factory class introduced to provide standard, built-in implementations of custom intermediate operations for the **Java Stream API**.

Recently, I explored **Virtual Threads** by utilizing the endpoint of [**HackerNews**](https://hacker-news.firebaseio.com/v0/topstories.json) to retrieve the top stories. During this exploration, I attempted to implemented the built-in Gatherers methods.
 
##### 2.2.1 Batching Items with `windowFixed`

Splits incoming stream elements into non-overlapping lists (batches) of a specified maximum size. The final batch may contain fewer elements if the stream size is not evenly divisible.

**Use-cases:** Simulating HackerNews top story Ids or batching items for bulk database inserts, pagination chunks, or batch fetching API requests.

```java
// Simulating Hacker News top story IDs
List<Integer> storyIds = List.of(50028275, 50029123, 50019911, 50022292, 49997481, 49981264, 50023450);

// Group into fixed windows of 3
List<List<Integer>> batches = storyIds.stream()
    .gather(Gatherers.windowFixed(3))
    .toList();

batches.forEach(batch -> System.out.println("Batch: " + batch));
// Output:
// Batch: [50028275, 50029123, 50019911]
// Batch: [50022292, 49997481, 49981264]
// Batch: [50023450]
```

##### 2.2.2 Analyzing Sequences with `windowSliding`
Generates sliding windows of a predetermined size. Unlike `windowFixed`, these windows overlap, incrementally shifting forward by one element at a time.

**Use-cases:**: Utilizing sliding windows of adjacent story IDs to monitor submission velocity or identify sequential ordering discrepancies in real-time.

```java
List<Integer> storyIds = List.of(50028275, 50029123, 50019911, 50022292, 49997481, 49981264, 50023450);

// Examine sliding windows of 3 consecutive story IDs
storyIds.stream()
    .limit(10)
    .gather(Gatherers.windowSliding(3))
    .forEach(window -> System.out.println("Sliding ID Window: " + window));

// Output:
// Sliding ID Window: [50028275, 50029123, 50019911]
// Sliding ID Window: [50019911, 50022292, 49997481]
// Sliding ID Window: [49997481, 49981264, 50023450]
```

##### 2.2.3 Accumulating Values with `scan`
##### 2.2.4 Intermediate Aggregations with `fold`
##### 2.2.5 Bounded Parallelism with `mapConcurrent`

### 3. Leveling Up: From Using Gatherers to Authoring Your Own
#### 3.1 When Should You Write a Custom Gatherer?
#### 3.2 Pipeline Composition with `andThen()`

### 4. Under the Hood: Architecture & Anatomy of Custom Gatherers
#### 4.1 Anatomy of a `Gatherer<T, A, R>`
#### 4.2 The Four Building Blocks (`initializer`, `integrator`, `finisher`, `combiner`)
#### 4.3 Concrete Walkthrough: Building a Custom Gatherer

### 5. Production-Ready Gatherers: Parallelism, State, and Pitfalls
#### 5.1 Parallel Execution Modes & Combiners
#### 5.2 Greedy vs. Short-Circuiting Integrators
#### 5.3 State Management & Thread Safety
#### 5.4 Gatherer vs. Collector: A Quick Comparison

### 6. Conclusion & Summary
