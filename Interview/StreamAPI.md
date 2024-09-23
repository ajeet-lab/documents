## What is Stream API in Java?

**Stream API** in Java is a powerful feature introduced in **Java 8** that provides a functional approach to process sequences of elements, such as collections (like lists or sets), in a **declarative way**. It enables developers to write clean, concise, and readable code to perform operations like filtering, mapping, and reducing data from a collection.

Streams are not data structures themselves but are sequences of data that can be **traversed** and processed. The **Stream API** helps in performing **bulk operations** efficiently, leveraging features like **laziness** (processing only when necessary) and **parallelism** (automatic use of multiple CPU cores).

### Key Features of Stream API

1. **Declarative Style**: Instead of writing loops and conditionals, Stream API allows you to **describe** what you want to do with the data.
   
2. **Functional Programming**: You can use **lambda expressions** and **method references** to process data in a functional manner.
   
3. **Laziness**: Streams are lazy, meaning that intermediate operations are only executed when a terminal operation is called.
   
4. **Parallelism**: It supports **parallel processing** to utilize multiple CPU cores, thus improving performance for large data sets.

5. **Immutability**: Stream operations do not modify the original data structure. Instead, they return a new stream or result, leaving the source unchanged.

### Stream API Concepts

#### 1. **Stream Creation**
Streams can be created from various data sources like collections, arrays, or input/output sources (like files).

```
// From a List
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();

// From an Array
Stream<String> streamFromArray = Stream.of("a", "b", "c");

// From a range of numbers
Stream<Integer> numberStream = Stream.iterate(1, n -> n + 1).limit(10);

// From a file (lines of text)
Stream<String> lines = Files.lines(Paths.get("file.txt"));
```

#### 2. **Intermediate Operations**
These operations **transform** a stream into another stream. They are **lazy**, meaning they don't perform the operation until a terminal operation is invoked. They are also **chained** together.

Common intermediate operations:
- **`filter()`**: Filters elements based on a condition.
  
    ```
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    numbers.stream()
           .filter(n -> n % 2 == 0) // Only even numbers
           .forEach(System.out::println); // Output: 2 4 6
    ```
  
- **`map()`**: Transforms each element in the stream into another form (e.g., converting one type to another).

    ```
    List<String> names = Arrays.asList("John", "Jane", "Tom");
    names.stream()
         .map(String::toUpperCase) // Convert each name to uppercase
         .forEach(System.out::println); // Output: JOHN JANE TOM
    ```

- **`sorted()`**: Sorts the elements in a natural order or using a comparator.

    ```
    List<Integer> nums = Arrays.asList(3, 1, 4, 1, 5, 9);
    nums.stream()
        .sorted()
        .forEach(System.out::println); // Output: 1 1 3 4 5 9
    ```

- **`distinct()`**: Removes duplicate elements from the stream.

    ```
    List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3);
    numbers.stream()
           .distinct()
           .forEach(System.out::println); // Output: 1 2 3
    ```

- **`limit()`** and **`skip()`**: `limit(n)` restricts the number of elements, while `skip(n)` skips the first `n` elements.

    ```
    Stream<Integer> infiniteStream = Stream.iterate(1, n -> n + 1);
    infiniteStream
        .skip(5)    // Skip first 5 elements
        .limit(5)   // Limit to the next 5 elements
        .forEach(System.out::println); // Output: 6 7 8 9 10
    ```

#### 3. **Terminal Operations**
These operations **consume** the stream, meaning after calling a terminal operation, the stream cannot be used again. They trigger the processing of intermediate operations.

Common terminal operations:
- **`forEach()`**: Performs an action for each element in the stream.
  
    ```
    List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
    names.stream().forEach(System.out::println); // Output: Alice Bob Charlie
    ```

- **`collect()`**: Transforms the elements of the stream into a different form, typically into a collection (like List or Set).

    ```
    List<Integer> evenNumbers = numbers.stream()
                                       .filter(n -> n % 2 == 0)
                                       .collect(Collectors.toList());
    ```

- **`reduce()`**: Combines elements of the stream into a single result.

    ```
    List<Integer> nums = Arrays.asList(1, 2, 3, 4);
    int sum = nums.stream().reduce(0, (a, b) -> a + b); // Output: 10
    ```

- **`count()`**: Returns the count of elements in the stream.

    ```
    long count = numbers.stream().filter(n -> n > 2).count(); // Output: 4
    ```

- **`findFirst()`** and **`findAny()`**: Returns the first or any element from the stream (useful in parallel streams).

    ```
    Optional<Integer> first = numbers.stream().findFirst();
    ```

#### 4. **Parallel Streams**
Streams can be processed in **parallel** to take advantage of multi-core processors. You can convert a stream into a **parallel stream** by using `parallelStream()` or `parallel()`.

```
List<Integer> largeList = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
largeList.parallelStream()
         .filter(n -> n % 2 == 0)
         .forEach(System.out::println);
```

Parallel streams automatically divide tasks into smaller chunks and process them on multiple threads, offering a performance boost when working with large data sets.

### Example: Full Stream Pipeline

Here’s an example where we use both intermediate and terminal operations to filter, sort, and collect data:

```
List<String> names = Arrays.asList("Tom", "Jerry", "Spike", "Tyke", "Tom");
List<String> result = names.stream()
                           .filter(name -> name.startsWith("T"))   // Filter names starting with 'T'
                           .distinct()                              // Remove duplicates
                           .sorted()                                // Sort alphabetically
                           .collect(Collectors.toList());           // Collect the result into a List
System.out.println(result); // Output: [Tom, Tyke]
```

### Stream vs. Collection: Key Differences

- **One-time use**: Streams cannot be reused once a terminal operation is called. Collections can be traversed multiple times.
- **Lazy Evaluation**: Intermediate operations in streams are not executed immediately; they are evaluated lazily.
- **Parallel Processing**: Streams support easy parallelization with `parallelStream()`, while collections don't directly support parallel processing.

### Conclusion

The **Stream API** in Java allows you to work with collections and other data sources more efficiently, making it easier to process large datasets in a clean, readable, and functional style. With streams, you can filter, map, sort, and reduce data in fewer lines of code while also having the ability to run tasks in parallel to boost performance.