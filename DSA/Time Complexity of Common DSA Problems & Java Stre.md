# Time Complexity of Common DSA Problems & Java Stream Operations

Let me explain time complexity for common DSA problem patterns and Java Stream operations in an easily understandable way.

## Common DSA Problem Fundamentals

### 1\. Array Traversal

```java
for (int i = 0; i < array.length; i++) {
    // Constant time operations
    System.out.println(array[i]);
}
```

**Time Complexity: O(n)**

**Explanation:** We process each of the n elements exactly once, performing constant-time operations on each. The number of operations grows linearly with the input size.

### 2\. Two-Pointer Technique

```java
// Find pair in sorted array with sum equal to target
public boolean findPair(int[] array, int target) {
    int left = 0;
    int right = array.length - 1;
    
    while (left < right) {
        int sum = array[left] + array[right];
        
        if (sum == target) {
            return true;
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return false;
}
```

**Time Complexity: O(n)**

**Explanation:** We start with two pointers at opposite ends and move them toward each other. In the worst case, we might examine each element once, making this a linear time algorithm.

### 3\. Binary Search

```java
public int binarySearch(int[] sortedArray, int target) {
    int left = 0;
    int right = sortedArray.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (sortedArray[mid] == target) {
            return mid;
        } else if (sortedArray[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1; // Not found
}
```

**Time Complexity: O(log n)**

**Explanation:** In each step, we eliminate half of the remaining elements. The number of operations is proportional to how many times we can divide n by 2 until reaching 1, which is log₂(n).

### 4\. Sliding Window

```java
// Find maximum sum subarray of size k
public int maxSubarraySum(int[] array, int k) {
    int maxSum = 0;
    int windowSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += array[i];
    }
    maxSum = windowSum;
    
    // Slide window and update maxSum
    for (int i = k; i < array.length; i++) {
        windowSum = windowSum + array[i] - array[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}
```

**Time Complexity: O(n)**

**Explanation:** We process each element at most twice - once when it enters the window and once when it exits. This makes the algorithm linear regardless of window size k.

### 5\. Depth-First Search (DFS) on Graph/Tree

```java
public void dfs(Node root) {
    if (root == null) return;
    
    // Process the current node
    System.out.println(root.value);
    
    // Recur for all neighbors/children
    for (Node neighbor : root.neighbors) {
        dfs(neighbor);
    }
}
```

**Time Complexity: O(V + E)**

**Explanation:** In a graph, we visit each vertex (V) once and traverse each edge (E) once. For trees (a special graph), this simplifies to O(n) where n is the number of nodes.

### 6\. Breadth-First Search (BFS)

```java
public void bfs(Node root) {
    if (root == null) return;
    
    Queue<Node> queue = new LinkedList<>();
    queue.add(root);
    
    while (!queue.isEmpty()) {
        Node current = queue.poll();
        
        // Process current node
        System.out.println(current.value);
        
        // Add all neighbors to queue
        for (Node neighbor : current.neighbors) {
            queue.add(neighbor);
        }
    }
}
```

**Time Complexity: O(V + E)**

**Explanation:** Similar to DFS, we visit each vertex once and each edge once, giving O(V + E) complexity.

### 7\. Dynamic Programming - Fibonacci

```java
public int fibonacci(int n) {
    if (n <= 1) return n;
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

**Time Complexity: O(n)**

**Explanation:** We calculate each Fibonacci number exactly once and store it for future use. This approach requires n calculations, resulting in linear time complexity.

### 8\. Merge Sort

```java
public void mergeSort(int[] array, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        // Sort first and second halves
        mergeSort(array, left, mid);
        mergeSort(array, mid + 1, right);
        
        // Merge the sorted halves
        merge(array, left, mid, right);
    }
}
```

**Time Complexity: O(n log n)**

**Explanation:** The recursion tree has log n levels (dividing array in half each time), and at each level, we do O(n) work during the merge step. So total work is O(n log n).

## Java Stream API Time Complexity

Now let's look at common Java Stream operations and their time complexity:

### 1\. Basic Stream Operations

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Simple map operation
List<Integer> doubled = numbers.stream()
                              .map(n -> n * 2)
                              .collect(Collectors.toList());
```

**Time Complexity: O(n)**

**Explanation:** The map operation processes each element exactly once, performing a constant-time operation on each.

### 2\. Filter Operation

```java
List<Integer> evenNumbers = numbers.stream()
                                  .filter(n -> n % 2 == 0)
                                  .collect(Collectors.toList());
```

**Time Complexity: O(n)**

**Explanation:** Filter examines each element once, applying a condition that takes constant time.

### 3\. Distinct Operation

```java
List<Integer> withDuplicates = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5, 5);

List<Integer> unique = withDuplicates.stream()
                                    .distinct()
                                    .collect(Collectors.toList());
```

**Time Complexity: O(n²) in worst case, O(n) in best case**

**Explanation:** The `distinct()` operation internally uses a `HashSet` to track unique elements. For each element:

- It computes the hashCode (O(1))
- Checks if the element exists in the set (O(1) average case, but O(n) worst case if there are many hash collisions)
- Adds the element to the set if it's new (O(1) amortized)

So while the average case is O(n), in a pathological case with many hash collisions, it could be O(n²).

### 4\. Sorted Operation

```java
List<Integer> sorted = numbers.stream()
                            .sorted()
                            .collect(Collectors.toList());
```

**Time Complexity: O(n log n)**

**Explanation:** Sorting operations in Java use algorithms like TimSort, which has O(n log n) time complexity.

### 5\. Common Collectors

#### a. toList() / toSet()

```java
List<Integer> list = numbers.stream().collect(Collectors.toList());
Set<Integer> set = numbers.stream().collect(Collectors.toSet());
```

**Time Complexity: O(n)**

**Explanation:** These collectors simply add each element to a collection, which is an O(1) operation per element.

#### b. toMap()

```java
Map<Integer, String> map = numbers.stream()
                                .collect(Collectors.toMap(
                                    n -> n,       // key mapper
                                    n -> "Value" + n  // value mapper
                                ));
```

**Time Complexity: O(n)**

**Explanation:** For each element, we compute a key and value (O(1) in this example) and add to a HashMap (O(1) amortized).

#### c. groupingBy()

```java
Map<Integer, List<Integer>> grouped = numbers.stream()
                                          .collect(Collectors.groupingBy(
                                              n -> n % 2  // classifier function
                                          ));
```

**Time Complexity: O(n)**

**Explanation:** We process each element once, applying the classifier function (O(1)) and adding it to the appropriate group in a HashMap (O(1) amortized).

#### d. joining()

```java
String joined = numbers.stream()
                     .map(String::valueOf)
                     .collect(Collectors.joining(", "));
```

**Time Complexity: O(n)**

**Explanation:** Each element is processed once and converted to a string. The joining operation uses a StringBuilder internally, which has amortized O(1) append operations.

### 6\. Terminal Operations

#### a. count(), anyMatch(), allMatch(), noneMatch()

```java
long count = numbers.stream().count();
boolean anyEven = numbers.stream().anyMatch(n -> n % 2 == 0);
```

**Time Complexity:**

- For `count()`: Always O(n)
- For `anyMatch()`: O(1) best case (if first element matches), O(n) worst case
- For `allMatch()` and `noneMatch()`: O(1) best case (if first element fails), O(n) worst case

**Explanation:** These operations may need to process each element, but can short-circuit when possible.

#### b. reduce()

```java
Optional<Integer> sum = numbers.stream().reduce(Integer::sum);
```

**Time Complexity: O(n)**

**Explanation:** Reduce processes each element once, combining it with the accumulated result.

### 7\. Parallel Streams

```java
List<Integer> result = numbers.parallelStream()
                           .map(n -> expensiveOperation(n))
                           .collect(Collectors.toList());
```

**Time Complexity: O(n/p)**

**Explanation:** With p processor cores, parallel streams can theoretically reduce the time by a factor of p. However, the actual speedup depends on many factors including overhead of splitting and merging work.

### 8\. Complex Stream Chains

```java
List<String> result = people.stream()
                         .filter(p -> p.getAge() > 18)  // O(n)
                         .sorted(Comparator.comparing(Person::getName))  // O(n log n)
                         .map(Person::getName)  // O(n)
                         .distinct()  // O(n) avg case
                         .collect(Collectors.toList());  // O(n)
```

**Time Complexity: O(n log n)**

**Explanation:** The overall complexity is determined by the most expensive operation, which is the sorting step with O(n log n). All other operations are O(n) or better.

## Important Considerations for Stream API

1.  **Lazy Evaluation**: Stream operations are lazy until a terminal operation is encountered. This means that in a chain like `filter().map().collect()`, elements flow through the entire chain one at a time rather than executing each operation on the entire dataset before moving to the next operation.
    
2.  **Short-Circuiting**: Operations like `findFirst()`, `anyMatch()`, etc. can terminate processing early without examining all elements.
    
3.  **Stateful vs. Stateless**: Operations like `sorted()` and `distinct()` are stateful and may require multiple passes or buffering of data, which affects performance.
    
4.  **Collection Type Impact**: The underlying collection type can affect performance. For example, accessing elements in an ArrayList is O(1) but in a LinkedList is O(n) for random access.
    

By understanding these fundamentals, you can better analyze and optimize your code when working with Java Streams and traditional data structures algorithms.

Would you like me to explain any particular stream operation or DSA problem pattern in more detail?