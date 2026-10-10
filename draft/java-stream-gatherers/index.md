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
Stream Gatherers is a novel capability introduced [JEP 461 - Streams Gatherers](https://openjdk.org/jeps/461) as a preview feature. It enables the creation of custom intermediate operations,
providing flexibility in transforming data within stream pipelines in ways that are not readily achievable through existing built-in intermediate operations.

### 2. Getting Started: Built-In Gatherers in Action
#### 2.1 Understanding Stream Cardinality
#### 2.2 The Five Ready-to-Use Gatherers
##### 2.2.1 Batching Items with `windowFixed`
##### 2.2.2 Analyzing Sequences with `windowSliding`
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
