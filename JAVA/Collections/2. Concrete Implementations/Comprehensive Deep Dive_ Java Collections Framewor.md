# Comprehensive Deep Dive: Java Collections Framework

## Core Framework Architecture

The Java Collections Framework is organized through a carefully designed hierarchy of interfaces, abstract classes, and concrete implementations. Let's build a thorough understanding from the ground up.

## Root Interfaces

### Iterable Interface

```java
public interface Iterable<T> {
    Iterator<T> iterator();
    // Since Java 8
    default void forEach(Consumer<? super T> action) {...}
    // Since Java 8
    default Spliterator<T> spliterator() {...}
}
```

The `Iterable` interface sits at the very root of the collection hierarchy. It defines just one abstract method `iterator()` that returns an Iterator over elements of type T.

The significance of `Iterable`:

- Enables the use of the enhanced for-loop (for-each)
- Allows collections to expose a way to iterate their elements without exposing internal structure
- Since Java 8, provides default methods for functional-style iterations via `forEach()` and parallel iteration via `spliterator()`

### Collection Interface

```java
public interface Collection<E> extends Iterable<E> {
    // Basic Operations
    int size();
    boolean isEmpty();
    boolean contains(Object o);
    boolean add(E e);
    boolean remove(Object o);
    Iterator<E> iterator();
    
    // Bulk Operations
    boolean containsAll(Collection<?> c);
    boolean addAll(Collection<? extends E> c);
    boolean removeAll(Collection<?> c);
    boolean retainAll(Collection<?> c);
    void clear();
    
    // Array Operations
    Object[] toArray();
    <T> T[] toArray(T[] a);
    
    // Java 8 Additions
    default Stream<E> stream() {...}
    default Stream<E> parallelStream() {...}
    default boolean removeIf(Predicate<? super E> filter) {...}
}
```

The `Collection` interface extends `Iterable` and represents a group of objects. This is the foundation for all collection implementations (except Maps, which follow a separate hierarchy).

Key aspects:

- Defines the core methods all collections must support
- Some operations are optional (may throw `UnsupportedOperationException`)
- Integrates with Java 8 Stream API through `stream()` and `parallelStream()`
- Supports bulk operations on entire collections
- Implementations must specify:
    - Thread safety guarantees
    - Whether null elements are allowed
    - Performance characteristics

## Primary Interface Branches

### List Interface

```java
public interface List<E> extends Collection<E> {
    // Positional Access
    E get(int index);
    E set(int index, E element);
    void add(int index, E element);
    E remove(int index);
    
    // Search Operations
    int indexOf(Object o);
    int lastIndexOf(Object o);
    
    // List Iteration
    ListIterator<E> listIterator();
    ListIterator<E> listIterator(int index);
    
    // View
    List<E> subList(int fromIndex, int toIndex);
    
    // Java 8 Additions
    default void sort(Comparator<? super E> c) {...}
    default void replaceAll(UnaryOperator<E> operator) {...}
}
```

`List` represents an ordered collection (also known as a sequence) that maintains insertion order and allows duplicates.

Key characteristics:

- Indexed access (like arrays)
- Support for bidirectional traversal via `ListIterator`
- Allows positional insertion and removal
- Can create views of subranges via `subList()`
- Default sorting operation via `sort()`

### Set Interface

```java
public interface Set<E> extends Collection<E> {
    // Inherits methods from Collection, with more specific semantics
    // No additional methods beyond Collection
    
    // Java 8 Additions
    default Spliterator<E> spliterator() {...}  // More specialized behavior
}
```

`Set` represents a collection that contains no duplicate elements—based on `equals()` method.

Key characteristics:

- Enforces uniqueness constraint
- Some implementations provide ordering guarantees (LinkedHashSet, TreeSet)
- Mathematical set operations are modeled by methods inherited from Collection
- Implementations determine iteration order

### Queue Interface

```java
public interface Queue<E> extends Collection<E> {
    // Insertion
    boolean add(E e);      // Throws exception if capacity restricted
    boolean offer(E e);    // Returns false if capacity restricted
    
    // Removal
    E remove();            // Throws exception if empty
    E poll();              // Returns null if empty
    
    // Examination
    E element();           // Throws exception if empty
    E peek();              // Returns null if empty
}
```

`Queue` represents a collection designed for holding elements prior to processing, typically in FIFO order (first-in-first-out).

Key characteristics:

- Each operation exists in two forms:
    - Throws exception on failure
    - Returns special value on failure (null or false)
- Different implementations may provide:
    - Unbounded queues
    - Bounded queues
    - Priority ordering (via `PriorityQueue`)

### Deque Interface

```java
public interface Deque<E> extends Queue<E> {
    // First element (head) operations
    void addFirst(E e);
    void offerFirst(E e);
    E removeFirst();
    E pollFirst();
    E getFirst();
    E peekFirst();
    
    // Last element (tail) operations
    void addLast(E e);
    void offerLast(E e);
    E removeLast();
    E pollLast();
    E getLast();
    E peekLast();
    
    // Stack operations
    void push(E e);        // Equivalent to addFirst
    E pop();               // Equivalent to removeFirst
    
    // Removes first occurrence
    boolean removeFirstOccurrence(Object o);
    boolean removeLastOccurrence(Object o);
    
    // Bidirectional iterator
    Iterator<E> descendingIterator();
}
```

`Deque` (pronounced "deck") stands for double-ended queue, which supports insertion and removal at both ends.

Key characteristics:

- Can be used as both FIFO queues and LIFO stacks
- Provides methods to add, remove, and examine elements at both ends
- Offers methods compatible with `Stack` (push/pop)
- Supports bidirectional iteration

### Map Interface

```java
public interface Map<K, V> {
    // Basic Operations
    V put(K key, V value);
    V get(Object key);
    V remove(Object key);
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    int size();
    boolean isEmpty();
    
    // Bulk Operations
    void putAll(Map<? extends K, ? extends V> m);
    void clear();
    
    // Collection Views
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();
    
    // Entry Interface for map entries
    interface Entry<K, V> {
        K getKey();
        V getValue();
        V setValue(V value);
        // Java 8 additions for comparisons
        // ...
    }
    
    // Java 8 Additions
    default V getOrDefault(Object key, V defaultValue) {...}
    default void forEach(BiConsumer<? super K, ? super V> action) {...}
    default void replaceAll(BiFunction<? super K, ? super V, ? extends V> function) {...}
    default V putIfAbsent(K key, V value) {...}
    default boolean remove(Object key, Object value) {...}
    default boolean replace(K key, V oldValue, V newValue) {...}
    default V replace(K key, V value) {...}
    default V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction) {...}
    default V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) {...}
    default V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) {...}
    default V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction) {...}
}
```

`Map` represents a mapping from keys to values with no duplicate keys allowed.

Key characteristics:

- Not a Collection (doesn't extend Collection interface)
- Three collection views: keys, values, and key-value pairs (entries)
- Rich set of default methods in Java 8+ for common operations
- Implementations determine:
    - Key order (if any)
    - Key comparison method (equals() or compareTo())
    - Performance characteristics

### SortedSet and NavigableSet Interfaces

```java
public interface SortedSet<E> extends Set<E> {
    // Range-view operations
    SortedSet<E> subSet(E fromElement, E toElement);
    SortedSet<E> headSet(E toElement);
    SortedSet<E> tailSet(E fromElement);
    
    // Endpoints
    E first();
    E last();
    
    // Ordering
    Comparator<? super E> comparator();
}

public interface NavigableSet<E> extends SortedSet<E> {
    // Navigation methods
    E lower(E e);      // greatest element < e
    E floor(E e);      // greatest element <= e
    E ceiling(E e);    // least element >= e
    E higher(E e);     // least element > e
    
    // Retrieval and removal
    E pollFirst();     // retrieve and remove first (lowest)
    E pollLast();      // retrieve and remove last (highest)
    
    // Enhanced range views (inclusive/exclusive bounds)
    NavigableSet<E> subSet(E fromElement, boolean fromInclusive, 
                          E toElement, boolean toInclusive);
    NavigableSet<E> headSet(E toElement, boolean inclusive);
    NavigableSet<E> tailSet(E fromElement, boolean inclusive);
    
    // Reverse order view
    NavigableSet<E> descendingSet();
    Iterator<E> descendingIterator();
}
```

These interfaces extend `Set` to provide sorted collections.

`SortedSet` characteristics:

- Elements are ordered using natural ordering or a provided Comparator
- Provides methods for range views and endpoints

`NavigableSet` extends `SortedSet` with:

- Navigation methods to find closest matches
- Methods that poll from either end
- More flexible range-view operations with inclusive/exclusive options
- Descending views

### SortedMap and NavigableMap Interfaces

```java
public interface SortedMap<K, V> extends Map<K, V> {
    // Range views
    SortedMap<K, V> subMap(K fromKey, K toKey);
    SortedMap<K, V> headMap(K toKey);
    SortedMap<K, V> tailMap(K fromKey);
    
    // Endpoints
    K firstKey();
    K lastKey();
    
    // Ordering
    Comparator<? super K> comparator();
}

public interface NavigableMap<K, V> extends SortedMap<K, V> {
    // Navigation methods
    Map.Entry<K, V> lowerEntry(K key);    // greatest entry with key < k
    K lowerKey(K key);
    Map.Entry<K, V> floorEntry(K key);    // greatest entry with key <= k
    K floorKey(K key);
    Map.Entry<K, V> ceilingEntry(K key);  // least entry with key >= k
    K ceilingKey(K key);
    Map.Entry<K, V> higherEntry(K key);   // least entry with key > k
    K higherKey(K key);
    
    // Retrieval and removal
    Map.Entry<K, V> firstEntry();
    Map.Entry<K, V> lastEntry();
    Map.Entry<K, V> pollFirstEntry();
    Map.Entry<K, V> pollLastEntry();
    
    // Enhanced range views
    NavigableMap<K, V> subMap(K fromKey, boolean fromInclusive,
                             K toKey, boolean toInclusive);
    NavigableMap<K, V> headMap(K toKey, boolean inclusive);
    NavigableMap<K, V> tailMap(K fromKey, boolean inclusive);
    
    // Reverse order view
    NavigableMap<K, V> descendingMap();
    NavigableSet<K> navigableKeySet();
    NavigableSet<K> descendingKeySet();
}
```

These interfaces extend `Map` to provide sorted mappings.

`SortedMap` characteristics:

- Keys are ordered using natural ordering or a provided Comparator
- Provides methods for range views and endpoints

`NavigableMap` extends `SortedMap` with:

- Rich navigation methods
- Operations that retrieve and remove entries
- Reverse order views
- More flexible range-view operations

## Concrete Implementations: The Deep Internals

### ArrayList

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, Serializable {
    
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ELEMENTDATA = {};
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    
    transient Object[] elementData;
    private int size;
    // ...
}
```

The internal structure of ArrayList:

1.  **Array Management:**
    
    - Backed by a dynamically resizable Object array (`elementData`)
    - Initial capacity is 10 by default
    - Growth formula: new capacity = old capacity + (old capacity >> 1)
    - This translates to ~1.5x growth factor (50% increase)
2.  **Memory Usage:**
    
    - An empty ArrayList uses minimal memory until elements are added
    - Maintains separate size tracking from actual array capacity
    - Memory overhead beyond elements: ~24 bytes + array reference overhead
3.  **Key Operations Internal Implementation:**
    
    - `add(E e)`: Checks capacity, grows if needed using `ensureCapacityInternal()`, then adds element at the end
    
    ```java
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);
        elementData[size++] = e;
        return true;
    }
    ```
    
    - `add(int index, E element)`: Shifts elements right using `System.arraycopy()`
    
    ```java
    public void add(int index, E element) {
        // Range check and capacity check...
        System.arraycopy(elementData, index, elementData, index + 1, size - index);
        elementData[index] = element;
        size++;
    }
    ```
    
    - `remove(int index)`: Shifts elements left using `System.arraycopy()`
    
    ```java
    public E remove(int index) {
        // Range check...
        E oldValue = elementData(index);
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index, numMoved);
        elementData[--size] = null; // Let GC do its work
        return oldValue;
    }
    ```
    
    - `trimToSize()`: Reduces capacity to match size
    - `ensureCapacity()`: Proactively grows the backing array
4.  **Iterators:**
    
    - Uses a fail-fast iterator that tracks structural modifications
    - `modCount` field inherited from AbstractList tracks structural changes
    - `ConcurrentModificationException` thrown if list modified during iteration
5.  **Performance Considerations:**
    
    - Random access: O(1)
    - Append at end: Amortized O(1), occasional O(n) for resize
    - Insert/delete in middle: O(n) - requires shifting elements
    - Memory overhead when capacity > size
    - Serial iteration is faster than LinkedList due to cache locality

### LinkedList

```java
public class LinkedList<E> extends AbstractSequentialList<E>
        implements List<E>, Deque<E>, Cloneable, Serializable {
    
    transient int size = 0;
    transient Node<E> first;
    transient Node<E> last;
    
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;
        
        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
    // ...
}
```

The internal structure of LinkedList:

1.  **Node Structure:**
    
    - Uses private static inner class `Node<E>`
    - Each node contains:
        - The data element (`item`)
        - Reference to previous node (`prev`)
        - Reference to next node (`next`)
    - Maintains head (`first`) and tail (`last`) pointers
    - Implemented as a doubly-linked list, not singly-linked
2.  **Memory Usage:**
    
    - Each node requires:
        - Element reference (4 or 8 bytes)
        - Next pointer (4 or 8 bytes)
        - Previous pointer (4 or 8 bytes)
        - Node object overhead (~16 bytes)
    - Total overhead ~24-40 bytes per element (vs ~4-8 bytes for ArrayList)
3.  **Key Operations Internal Implementation:**
    
    - `add(E e)`: Appends to end by updating `last` pointer
    
    ```java
    public boolean add(E e) {
        linkLast(e);
        return true;
    }
    
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
    ```
    
    - `add(int index, E element)`: Finds the node at position, then inserts
    
    ```java
    public void add(int index, E element) {
        checkPositionIndex(index);
        if (index == size)
            linkLast(element);
        else
            linkBefore(element, node(index));
    }
    
    // Finds node at specified index by traversing from closest end
    Node<E> node(int index) {
        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
    ```
    
    - `remove(int index)`: Finds node, unlinks it by updating adjacent nodes
    
    ```java
    public E remove(int index) {
        checkElementIndex(index);
        return unlink(node(index));
    }
    
    E unlink(Node<E> x) {
        final E element = x.item;
        final Node<E> next = x.next;
        final Node<E> prev = x.prev;
        
        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }
        
        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }
        
        x.item = null;
        size--;
        modCount++;
        return element;
    }
    ```
    
4.  **Deque Operations:**
    
    - Implements all Deque operations (addFirst, addLast, removeFirst, etc.)
    - Stack operations (push, pop) map to addFirst, removeFirst
    - Queue operations (offer, poll) map to addLast, removeFirst
5.  **Iterators:**
    
    - Provides `ListIterator` implementation for bidirectional traversal
    - `descendingIterator()` for reverse traversal
    - Fail-fast on structural modification
6.  **Performance Considerations:**
    
    - Random access: O(n) - requires traversal
    - Optimizes traversal by choosing closest end (first or last)
    - Insert/delete at known position: O(1) - only updates pointers
    - Finding a position: O(n)
    - Higher memory overhead per element than ArrayList

### HashMap

```java
public class HashMap<K, V> extends AbstractMap<K, V>
        implements Map<K, V>, Cloneable, Serializable {
    
    static final int DEFAULT_INITIAL_CAPACITY = 16;
    static final int MAXIMUM_CAPACITY = 1 << 30;
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    static final int TREEIFY_THRESHOLD = 8;
    static final int UNTREEIFY_THRESHOLD = 6;
    static final int MIN_TREEIFY_CAPACITY = 64;
    
    transient Node<K, V>[] table;
    transient int size;
    transient int modCount;
    int threshold;
    final float loadFactor;
    
    static class Node<K, V> implements Map.Entry<K, V> {
        final int hash;
        final K key;
        V value;
        Node<K, V> next;
        // ...
    }
    
    static final class TreeNode<K, V> extends LinkedHashMap.Entry<K, V> {
        TreeNode<K, V> parent;
        TreeNode<K, V> left;
        TreeNode<K, V> right;
        TreeNode<K, V> prev;
        boolean red;
        // ...
    }
    // ...
}
```

The internal structure of HashMap (Java 8+):

1.  **Array and Node Structure:**
    
    - Backbone is an array (`table`) of nodes (buckets)
    - Each bucket can contain:
        - Single entry
        - Linked list of entries (for collisions)
        - Red-Black tree (for many collisions)
    - Each entry includes:
        - Hash code (cached for performance)
        - Key reference
        - Value reference
        - Next pointer (for linked lists)
2.  **Hashing and Indexing:**
    
    - Uses key's `hashCode()` method, then applies additional hash function
    - Index calculation: `(n - 1) & hash` (where n is table size)
    - Table size is always a power of 2 to optimize this calculation
3.  **Advanced Collision Resolution:**
    
    - Initially uses linked lists for collision chains
    - When a bucket exceeds TREEIFY_THRESHOLD (8 nodes):
        - Converts to balanced Red-Black Tree
        - But only if table size >= MIN_TREEIFY_CAPACITY (64)
    - When a tree bucket shrinks below UNTREEIFY_THRESHOLD (6):
        - Converts back to linked list
4.  **Resizing and Load Factor:**
    
    - Initial capacity: 16 buckets
    - Load factor: 0.75 (default)
    - Threshold = capacity \* load factor
    - When size exceeds threshold:
        - Creates new array of double size
        - Rehashes all entries into new table
        - Expensive O(n) operation, but amortized
5.  **Key Operations Internal Implementation:**
    
    - `put(K key, V value)`: Core method, places entry or updates existing
    
    ```java
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
    // Actual implementation (simplified)
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        // Create table if null
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        
        // Find bucket index and create new node if bucket empty
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            // Check if key already exists
            if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            // Check if tree node
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            // Handle linked list
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        // Convert to tree if threshold reached
                        if (binCount >= TREEIFY_THRESHOLD - 1)
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            // Update existing value
            if (e != null) {
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        // Resize if threshold exceeded
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
    ```
    
    - `get(Object key)`: Finds entry for key
    
    ```java
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
    
    // Actual implementation (simplified)
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            // Check first node
            if (first.hash == hash && ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            // Check rest of chain
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
    ```
    
6.  **Key Equality Rules:**
    
    - Two keys are considered equal if:
        - `key1.equals(key2)` returns true, AND
        - Their hash codes are the same
    - Null keys are allowed (stored in bucket 0)
7.  **Performance Considerations:**
    
    - Average case: O(1) for get/put
    - Worst case: O(log n) with tree buckets (vs O(n) with just lists)
    - Iteration: O(capacity + size) - must scan all buckets

### HashSet

```java
public class HashSet<E> extends AbstractSet<E>
        implements Set<E>, Cloneable, Serializable {
    
    private transient HashMap<E,Object> map;
    private static final Object PRESENT = new Object();
    
    public HashSet() {
        map = new HashMap<>();
    }
    
    public boolean add(E e) {
        return map.put(e, PRESENT) == null;
    }
    
    public boolean contains(Object o) {
        return map.containsKey(o);
    }
    
    public boolean remove(Object o) {
        return map.remove(o) == PRESENT;
    }
    
    // etc...
}
```

HashSet is a thin wrapper around HashMap:

1.  **Backing Structure:**
    
    - Uses a HashMap internally
    - Elements stored as keys in the backing map
    - All values are a shared dummy object (PRESENT)
2.  **Key Methods Implementation:**
    
    - `add(E e)`: Calls `map.put(e, PRESENT)`
    - `contains(Object o)`: Calls `map.containsKey(o)`
    - `remove(Object o)`: Calls `map.remove(o)`
    - `size()`: Returns `map.size()`
    - `iterator()`: Returns iterator over map keys
3.  **Performance Characteristics:**
    
    - Identical to HashMap for equivalent operations
    - Smaller memory footprint than HashMap for same number of elements
4.  **Special Constructors:**
    
    - Can specify initial capacity and load factor
    - Can construct from another Collection

### TreeMap

```java
public class TreeMap<K,V> extends AbstractMap<K,V>
        implements NavigableMap<K,V>, Cloneable, Serializable {
    
    private final Comparator<? super K> comparator;
    private transient Entry<K,V> root;
    private transient int size = 0;
    private transient int modCount = 0;
    
    private static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;
        // ...
    }
    // ...
}
```

The internal structure of TreeMap:

1.  **Red-Black Tree Implementation:**
    
    - Self-balancing binary search tree
    - Each node is colored either red or black
    - Tree maintains these properties:
        - Root is black
        - No red node has a red child
        - All paths from root to leaves have same number of black nodes
    - These properties ensure O(log n) height
2.  **Entry Structure:**
    
    - Each entry contains:
        - Key and value
        - Left, right, and parent pointers
        - Color flag
    - The tree is ordered by keys
3.  **Key Ordering:**
    
    - Uses natural ordering (Comparable interface) by default
    - Or a custom Comparator if provided
    - Unlike HashMap, doesn't use hashCode() or equals()
    - Uses compareTo() or compare() instead
4.  **Key Operations Internal Implementation:**
    
    - `put(K key, V value)`: Adds or updates entry
    
    ```java
    public V put(K key, V value) {
        Entry<K,V> t = root;
        // Handle empty tree
        if (t == null) {
            // Key must be comparable if no comparator
            // ...
            root = new Entry<>(key, value, null);
            size = 1;
            modCount++;
            return null;
        }
        
        int cmp;
        Entry<K,V> parent;
        // Comparator or natural ordering
        Comparator<? super K> cpr = comparator;
        if (cpr != null) {
            // Find position to insert
            do {
                parent = t;
                cmp = cpr.compare(key, t.key);
                if (cmp < 0)
                    t = t.left;
                else if (cmp > 0)
                    t = t.right;
                else
                    return t.setValue(value);
            } while (t != null);
        } else {
            // Similar loop for comparable
            // ...
        }
        
        // Insert new entry
        Entry<K,V> e = new Entry<>(key, value, parent);
        if (cmp < 0)
            parent.left = e;
        else
            parent.right = e;
        
        // Rebalance tree
        fixAfterInsertion(e);
        size++;
        modCount++;
        return null;
    }
    ```
    
    - `get(Object key)`: Finds entry for key
    - `remove(Object key)`: Removes entry, rebalances tree
    - Range operations: Traverse tree within given bounds
5.  **Tree Balancing Operations:**
    
    - `rotateLeft()` and `rotateRight()`: Standard tree rotations
    - `fixAfterInsertion()`: Rebalances after adding node
    - `fixAfterDeletion()`: Rebalances after removing node
    - These operations maintain the Red-Black properties
6.  **Performance Characteristics:**
    
    - All operations guaranteed O(log n)