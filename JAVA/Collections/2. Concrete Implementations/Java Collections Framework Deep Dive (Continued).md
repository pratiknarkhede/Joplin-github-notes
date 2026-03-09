# Java Collections Framework Deep Dive (Continued)

## Concrete Implementations (Continued)

### TreeSet

```java
public class TreeSet<E> extends AbstractSet<E>
        implements NavigableSet<E>, Cloneable, Serializable {
    
    private transient NavigableMap<E,Object> m;
    private static final Object PRESENT = new Object();
    
    public TreeSet() {
        this.m = new TreeMap<>();
    }
    
    public TreeSet(Comparator<? super E> comparator) {
        this.m = new TreeMap<>(comparator);
    }
    
    // Other constructors...
    
    public boolean add(E e) {
        return m.put(e, PRESENT) == null;
    }
    
    // Other methods delegate to the map...
}
```

Like HashSet, TreeSet is a thin wrapper around its map counterpart:

1.  **Backing Structure:**
    
    - Uses a TreeMap internally
    - Elements stored as keys in the backing map
    - All values are a shared dummy object (PRESENT)
2.  **Key Methods Implementation:**
    
    - Most operations delegate directly to the backing TreeMap
    - The wrapper simply translates between Set operations and Map operations
    - For example, `add(E e)` calls `m.put(e, PRESENT)`
3.  **Element Ordering:**
    
    - Elements are ordered according to:
        - Natural ordering (elements must implement Comparable)
        - Or a custom Comparator provided at construction time
    - Unlike HashSet, iteration is always in sorted order
4.  **Performance Characteristics:**
    
    - All operations are O(log n) due to TreeMap implementation
    - Slower than HashSet for basic operations, but maintains order
    - Provides efficient range operations not available in HashSet

### PriorityQueue

```java
public class PriorityQueue<E> extends AbstractQueue<E>
        implements Serializable {
    
    private static final int DEFAULT_INITIAL_CAPACITY = 11;
    
    transient Object[] queue;
    private int size = 0;
    private final Comparator<? super E> comparator;
    transient int modCount = 0;
    
    // ...
}
```

The internal structure of PriorityQueue:

1.  **Binary Heap Implementation:**
    
    - Implemented as a binary heap stored in an array
    - Default is min-heap (smallest element at root)
    - Parent-child relationships determined by indices:
        - For node at index i:
            - Left child: 2\*i + 1
            - Right child: 2\*i + 2
            - Parent: (i-1)/2
2.  **Element Ordering:**
    
    - Uses natural ordering (Comparable) by default
    - Or a custom Comparator if provided
    - Priority determined by this ordering
3.  **Key Operations Internal Implementation:**
    
    - `offer(E e)` / `add(E e)`: Adds element to heap
    
    ```java
    public boolean offer(E e) {
        if (e == null)
            throw new NullPointerException();
        modCount++;
        int i = size;
        if (i >= queue.length)
            grow(i + 1);
        size = i + 1;
        if (i == 0)
            queue[0] = e;
        else
            siftUp(i, e);
        return true;
    }
    
    private void siftUp(int k, E x) {
        if (comparator != null)
            siftUpUsingComparator(k, x);
        else
            siftUpComparable(k, x);
    }
    
    private void siftUpComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>) x;
        while (k > 0) {
            int parent = (k - 1) >>> 1;
            Object e = queue[parent];
            if (key.compareTo((E) e) >= 0)
                break;
            queue[k] = e;
            k = parent;
        }
        queue[k] = key;
    }
    ```
    
    - `poll()`: Removes and returns the head element
    
    ```java
    public E poll() {
        if (size == 0)
            return null;
        int s = --size;
        modCount++;
        E result = (E) queue[0];
        E x = (E) queue[s];
        queue[s] = null;
        if (s != 0)
            siftDown(0, x);
        return result;
    }
    
    private void siftDown(int k, E x) {
        if (comparator != null)
            siftDownUsingComparator(k, x);
        else
            siftDownComparable(k, x);
    }
    
    private void siftDownComparable(int k, E x) {
        Comparable<? super E> key = (Comparable<? super E>) x;
        int half = size >>> 1;
        while (k < half) {
            int child = (k << 1) + 1;
            Object c = queue[child];
            int right = child + 1;
            if (right < size && ((Comparable<? super E>) c).compareTo((E) queue[right]) > 0)
                c = queue[child = right];
            if (key.compareTo((E) c) <= 0)
                break;
            queue[k] = c;
            k = child;
        }
        queue[k] = key;
    }
    ```
    
    - `peek()`: Returns but doesn't remove the head
    
    ```java
    public E peek() {
        return (size == 0) ? null : (E) queue[0];
    }
    ```
    
4.  **Resizing Behavior:**
    
    - Initial capacity is 11 (DEFAULT_INITIAL_CAPACITY)
    - Grows by at least 50% when capacity reached
    - Growth formula depends on current size:
        - Small queues double in size
        - Large queues grow by 50%
5.  **Performance Characteristics:**
    
    - `offer()`, `poll()`: O(log n)
    - `peek()`: O(1)
    - `contains()`: O(n) (must search entire heap)
    - `remove(Object)`: O(n) (must search, then reheapify)
6.  **Important Notes:**
    
    - Does not permit null elements
    - Not thread-safe
    - Iterator does not guarantee order
    - For thread-safe version, use PriorityBlockingQueue

### ArrayDeque

```java
public class ArrayDeque<E> extends AbstractCollection<E>
        implements Deque<E>, Cloneable, Serializable {
    
    transient Object[] elements;
    transient int head;
    transient int tail;
    private static final int MIN_INITIAL_CAPACITY = 8;
    
    // ...
}
```

The internal structure of ArrayDeque:

1.  **Circular Array Implementation:**
    
    - Backed by a circular array (elements wrap around)
    - Maintains head and tail indices
    - Elements are stored between head (inclusive) and tail (exclusive)
    - When empty: head == tail
    - When full: head == (tail + 1) % elements.length
2.  **Index Calculations:**
    
    - Next index calculation: `(index + 1) & (elements.length - 1)`
    - Previous index: `(index - 1) & (elements.length - 1)`
    - These bitwise operations are optimized for array sizes that are powers of 2
3.  **Key Operations Internal Implementation:**
    
    - `addFirst(E e)`: Inserts at head
    
    ```java
    public void addFirst(E e) {
        if (e == null)
            throw new NullPointerException();
        elements[head = (head - 1) & (elements.length - 1)] = e;
        if (head == tail)
            doubleCapacity();
    }
    ```
    
    - `addLast(E e)`: Inserts at tail
    
    ```java
    public void addLast(E e) {
        if (e == null)
            throw new NullPointerException();
        elements[tail] = e;
        if ((tail = (tail + 1) & (elements.length - 1)) == head)
            doubleCapacity();
    }
    ```
    
    - `pollFirst()`: Removes from head
    
    ```java
    public E pollFirst() {
        int h = head;
        @SuppressWarnings("unchecked")
        E result = (E) elements[h];
        if (result == null)
            return null;
        elements[h] = null;
        head = (h + 1) & (elements.length - 1);
        return result;
    }
    ```
    
    - `pollLast()`: Removes from tail
    
    ```java
    public E pollLast() {
        int t = (tail - 1) & (elements.length - 1);
        @SuppressWarnings("unchecked")
        E result = (E) elements[t];
        if (result == null)
            return null;
        elements[t] = null;
        tail = t;
        return result;
    }
    ```
    
4.  **Resizing Behavior:**
    
    - Initial capacity is at least 8 (MIN_INITIAL_CAPACITY)
    - When full, capacity doubles (`doubleCapacity()`)
    - During resize, elements are reorganized to start at index 0
5.  **Performance Characteristics:**
    
    - All basic operations (add/remove from both ends): O(1)
    - Random access: O(1) but not directly exposed in API
    - Memory usage more efficient than LinkedList
6.  **Important Notes:**
    
    - Does not permit null elements
    - Not thread-safe
    - More efficient than LinkedList for most queue operations
    - Ideal for implementing both stacks and queues

### ConcurrentHashMap

```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
        implements ConcurrentMap<K,V>, Serializable {
    
    private static final int DEFAULT_CAPACITY = 16;
    private static final float DEFAULT_LOAD_FACTOR = 0.75f;
    private static final int DEFAULT_CONCURRENCY_LEVEL = 16;
    
    // Table of nodes
    transient volatile Node<K,V>[] table;
    
    // Number of nodes in the map
    private transient volatile long baseCount;
    
    // Map initialized flag
    private transient volatile int sizeCtl;
    
    // Counter cells for size calculations
    private transient volatile CounterCell[] counterCells;
    
    // Node class for entries
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        volatile V val;
        volatile Node<K,V> next;
        // ...
    }
    
    // Specialized nodes for tree bins
    static final class TreeBin<K,V> extends Node<K,V> {
        TreeNode<K,V> root;
        volatile TreeNode<K,V> first;
        volatile Thread waiter;
        volatile int lockState;
        // ...
    }
    
    // ...
}
```

The internal structure of ConcurrentHashMap (Java 8+):

1.  **Concurrency Design:**
    
    - Uses a fine-grained locking strategy
    - No lock on the entire table; locks only affected parts
    - Java 8+ uses synchronized blocks on individual nodes/bins
    - Pre-Java 8 used separate locks for separate segments
2.  **Internal Data Structures:**
    
    - Hash table of nodes similar to HashMap
    - Nodes include:
        - Regular nodes (linked lists)
        - TreeBin nodes (Red-Black Trees)
        - ForwardingNodes (during resizing)
        - ReservationNodes (placeholders)
    - Uses volatile fields for visibility across threads
3.  **Locking Mechanism:**
    
    - Lock striping: different hash buckets can be accessed concurrently
    - Uses synchronized on first node in bucket for write operations
    - Many read operations require no locking
    - Uses CAS (Compare-And-Swap) operations for concurrent updates
4.  **Resize/Rehash Strategy:**
    
    - Table doubles in size when threshold reached
    - Resizing is done incrementally by multiple threads
    - Each bucket is transferred independently
    - Uses ForwardingNodes to mark already-transferred buckets
5.  **Key Operation Internal Implementation:**
    
    - `put(K key, V value)`: Thread-safe insertion/update
    
    ```java
    public V put(K key, V value) {
        return putVal(key, value, false);
    }
    
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            // Initialize table if needed
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            // Create node if bucket empty (CAS operation)
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value, null)))
                    break;
            }
            // Help resize if ongoing
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            // Add/update in existing bucket
            else {
                V oldVal = null;
                // Synchronize on first node in bucket
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            // Linked list
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key, value, null);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            // Tree handling
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key, value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                    }
                }
                // Convert to tree if threshold reached
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        // Update size counters and check for resize
        addCount(1L, binCount);
        return null;
    }
    ```
    
    - `get(Object key)`: Non-blocking reads
    
    ```java
    public V get(Object key) {
        Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
        int h = spread(key.hashCode());
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (e = tabAt(tab, (n - 1) & h)) != null) {
            // Check first node
            if ((eh = e.hash) == h) {
                if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                    return e.val;
            }
            // Special case for negative hash (TreeBin, etc)
            else if (eh < 0)
                return (p = e.find(h, key)) != null ? p.val : null;
            
            // Search rest of chain
            while ((e = e.next) != null) {
                if (e.hash == h &&
                    ((ek = e.key) == key || (ek != null && key.equals(ek))))
                    return e.val;
            }
        }
        return null;
    }
    ```
    
6.  **Size Calculation:**
    
    - Uses striped counters (CounterCell array) to reduce contention
    - Size approximation during concurrent updates
    - Exact counts require traversal in some cases
7.  **Unique Features:**
    
    - Does not allow null keys or values
    - Provides atomic compound operations (putIfAbsent, replace, etc.)
    - Supports concurrent retrieval and high expected concurrency for updates
8.  **Performance Considerations:**
    
    - Higher memory overhead than HashMap
    - Slightly slower than HashMap for single-threaded use
    - Vastly outperforms synchronized HashMap for concurrent access
    - Read operations don't block other operations

### CopyOnWriteArrayList

```java
public class CopyOnWriteArrayList<E> implements List<E>, RandomAccess, Cloneable, Serializable {
    final transient Object lock = new Object();
    private transient volatile Object[] array;
    
    public CopyOnWriteArrayList() {
        setArray(new Object[0]);
    }
    
    final void setArray(Object[] a) {
        array = a;
    }
    
    // ...
}
```

The internal structure of CopyOnWriteArrayList:

1.  **Copy-on-Write Strategy:**
    
    - Maintains an immutable array internally
    - Every modification creates a new array copy
    - Reads never block and see a consistent snapshot
    - Write operations are synchronized via an internal lock
2.  **Key Operations Internal Implementation:**
    
    - `add(E e)`: Creates new array with additional element
    
    ```java
    public boolean add(E e) {
        synchronized (lock) {
            Object[] elements = getArray();
            int len = elements.length;
            Object[] newElements = Arrays.copyOf(elements, len + 1);
            newElements[len] = e;
            setArray(newElements);
            return true;
        }
    }
    ```
    
    - `remove(int index)`: Creates new array without element
    
    ```java
    public E remove(int index) {
        synchronized (lock) {
            Object[] elements = getArray();
            int len = elements.length;
            E oldValue = get(elements, index);
            int numMoved = len - index - 1;
            if (numMoved == 0)
                setArray(Arrays.copyOf(elements, len - 1));
            else {
                Object[] newElements = new Object[len - 1];
                System.arraycopy(elements, 0, newElements, 0, index);
                System.arraycopy(elements, index + 1, newElements, index, numMoved);
                setArray(newElements);
            }
            return oldValue;
        }
    }
    ```
    
    - `get(int index)`: Direct array access, no locking
    
    ```java
    public E get(int index) {
        return get(getArray(), index);
    }
    
    private E get(Object[] a, int index) {
        return (E) a[index];
    }
    ```
    
3.  **Iterator Behavior:**
    
    - Iterators work on a snapshot of the array at creation time
    - Never throw ConcurrentModificationException
    - Do not reflect modifications made after iterator creation
    
    ```java
    public Iterator<E> iterator() {
        return new COWIterator<E>(getArray(), 0);
    }
    
    static final class COWIterator<E> implements ListIterator<E> {
        private final Object[] snapshot;
        private int cursor;
        
        COWIterator(Object[] elements, int initialCursor) {
            cursor = initialCursor;
            snapshot = elements;
        }
        // ...
    }
    ```
    
4.  **Performance Characteristics:**
    
    - Read operations: O(1) for random access, very fast
    - Write operations: O(n) due to array copying, very expensive
    - Memory usage: Can be high during modifications
    - Exceptional for read-heavy, write-seldom scenarios
5.  **Important Notes:**
    
    - Thread-safe without locking for reads
    - Very expensive for frequent modifications
    - Iterators reflect a moment-in-time snapshot
    - No support for structural modifications through iterators

### LinkedHashMap

```java
public class LinkedHashMap<K,V> extends HashMap<K,V>
        implements Map<K,V> {
    
    // Doubly-linked list of entries
    transient LinkedHashMap.Entry<K,V> head;
    transient LinkedHashMap.Entry<K,V> tail;
    
    // True for access-order, false for insertion-order
    final boolean accessOrder;
    
    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
    
    // ...
}
```

The internal structure of LinkedHashMap:

1.  **Hybrid Structure:**
    
    - Extends HashMap for basic hash table functionality
    - Adds doubly-linked list to maintain iteration order
    - Each entry is both in hash table and linked list
2.  **Entry Linking:**
    
    - Custom Entry class extends HashMap.Node
    - Adds `before` and `after` pointers for linked list
    - Head and tail references track list ends
    - New entries added to end of list (tail)
3.  **Ordering Modes:**
    
    - **Insertion-order** (default): Entries remain in order they were added
    - **Access-order**: Entries reordered when accessed (get/put)
    - Access-order mode enables LRU (Least Recently Used) cache behavior
4.  **Key Operations Internal Implementation:**
    
    - Inherits `put` and `get` from HashMap, but overrides:
        
    - `afterNodeInsertion`: Called after node insertion to maintain order
        
    
    ```java
    void afterNodeInsertion(boolean evict) {
        LinkedHashMap.Entry<K,V> first;
        // removeEldestEntry only matters in access-order mode
        if (evict && (first = head) != null && removeEldestEntry(first)) {
            K key = first.key;
            removeNode(hash(key), key, null, false, true);
        }
    }
    ```
    
    - `afterNodeAccess`: Moves recently accessed node to end (for access-order)
    
    ```java
    void afterNodeAccess(Node<K,V> e) {
        LinkedHashMap.Entry<K,V> last;
        if (accessOrder && (last = tail) != e) {
            LinkedHashMap.Entry<K,V> p =
                (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
            p.after = null;
            if (b == null)
                head = a;
            else
                b.after = a;
            if (a != null)
                a.before = b;
            else
                last = b;
            if (last == null)
                head = p;
            else {
                p.before = last;
                last.after = p;
            }
            tail = p;
            ++modCount;
        }
    }
    ```
    
    - `removeEldestEntry`: Hook for implementing LRU caches
    
    ```java
    protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
        return false;  // Default implementation
    }
    ```
    
5.  **LRU Cache Implementation:**
    
    - Create subclass that overrides `removeEldestEntry`
    
    ```java
    class LRUCache<K,V> extends LinkedHashMap<K,V> {
        private final int capacity;
        
        public LRUCache(int capacity) {
            super(capacity, 0.75f, true);  // true for access-order
            this.capacity = capacity;
        }
        
        @Override
        protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
            //If the current number of entries exceeds the cache's capacity, return true to remove the eldest entry.
            return size() > capacity;
        }
    }
    ```
    
6.  **Performance Characteristics:**
    
    - Get/put: O(1) average case, like HashMap
    - Iteration: O(n) but more efficient than HashMap
    - Slightly more overhead than HashMap for entry maintenance
    - Predictable iteration order unlike HashMap

### EnumMap

```java
public class EnumMap<K extends Enum<K>, V> extends AbstractMap<K,V>
        implements Serializable, Cloneable {
    
    private final Class<K> keyType;
    private transient Object[] vals;
    private transient int size = 0;
    private transient int totalSize;
    
    // ...
}
```

The internal structure of EnumMap:

1.  **Array-Based Implementation:**
    
    - Uses a simple array indexed by enum ordinal values
    - No hash calculation required
    - Very compact and efficient representation
    - Size of array exactly matches number of possible enum values
2.  **Key Type Enforcement:**
    
    - Requires keys to be of a specific enum type
    - Stores the enum class for type checking and reflection
3.  **Key Operations Internal Implementation:**
    
    - `put(K key, V value)`: Direct array access by ordinal
    
    ```java
    public V put(K key, V value) {
        typeCheck(key);
        int index = key.ordinal();
        Object oldValue = vals[index];
        vals[index] = maskNull(value);
        if (oldValue == null)
            size++;
        return unmaskNull(oldValue);
    }
    ```
    
    - `get(Object key)`: Direct array access if key is valid enum
    
    ```java
    public V get(Object key) {
        return (isValidKey(key)) ? unmaskNull(vals[((Enum<?>)key).ordinal()]) : null;
    }
    ```
    
    - `containsKey(Object key)`: Check array slot at ordinal
    
    ```java
    public boolean containsKey(Object key) {
        return (isValidKey(key) && vals[((Enum<?>)key).ordinal()] != null);
    }
    ```
    
4.  **Null Handling:**
    
    - Uses a special marker object for null values
    - `maskNull()` and `unmaskNull()` methods handle conversion
5.  **Performance Characteristics:**
    
    - All basic operations: O(1) with minimal overhead
    - Extremely memory efficient
    - Faster than HashMap for enum keys
    - Iteration in natural enum order
6.  **Important Notes:**
    
    - Type-safe by requiring specific enum type
    - Not thread-safe
    - Iterator reflects natural enum constant order

### EnumSet

```java
public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
        implements Cloneable, Serializable {
    
    final Class<E> elementType;
    final Enum<?>[] universe;
    
    // Factory methods create appropriate implementation
    public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
        Enum<?>[] universe = getUniverse(elementType);
        if (universe.length <= 64)
            return new RegularEnumSet<>(elementType, universe);
        else
            return new JumboEnumSet<>(elementType, universe);
    }
    
    // ...
}

// Regular implementation for <= 64 elements
class RegularEnumSet<E extends Enum<E>> extends EnumSet<E> {
    private long elements = 0L;
    // ...
}

// Jumbo implementation for > 64 elements
class JumboEnumSet<E extends Enum<E>> extends EnumSet<E> {
    private long[] elements;
    // ...
}
```

The internal structure of EnumSet:

1.  **Bit Vector Implementation:**
    
    - Uses bits to represent presence/absence of enum values
    - Two concrete implementations:
        - `RegularEnumSet`: Uses a single `long` (for <= 64 elements)
        - `JumboEnumSet`: Uses `long[]` array (for > 64 elements)
    - Factory methods choose appropriate implementation
2.  **Bit Operations:**
    
    - Set bit: `elements |= (1L << ordinal)`
    - Clear bit: `elements &= ~(1L << ordinal)`
    - Test bit: `(elements & (1L << ordinal)) != 0`
3.  **Key Operations Internal Implementation (RegularEnumSet):**
    
    - `add(E e)`: Sets the bit for element's ordinal
    
    ```java
    public boolean add(E e) {
        typeCheck(e);
        long oldElements = elements;
        elements |= (1L << e.ordinal());
        return elements != oldElements;
    }
    ```
    
    - `remove(Object o)`: Clears the bit if valid element
    
    ```java
    public boolean remove(Object o) {
        if (o == null || o.getClass() != elementType)
            return false;
        Enum<?> e = (Enum<?>) o;
        long oldElements = elements;
        elements &= ~(1L << e.ordinal());
        return elements != oldElements;
    }
    ```
    
    - `contains(Object o)`: Tests the bit if valid element
    
    ```java
    public boolean contains(Object o) {
        if (o == null || o.getClass() != elementType)
            return false;
        Enum<?> e = (Enum<?>) o;
        return (elements & (1L << e.ordinal())) != 0;
    }
    ```
    
4.  **Performance Characteristics:**
    
    - All basic operations: O(1) with minimal overhead
    - Extremely memory efficient (only a few longs even for large enum sets)
    - Iteration still O(n) where n is number of enum values
    - Set operations (union, intersection) use efficient bitwise operations
5.  **Important Notes:**
    
    - Type-safe by requiring specific enum type
    - Not thread-safe
    - Iterator reflects natural enum constant order
    - Most efficient set implementation in Java

## Concurrent Collection Classes

### CopyOnWriteArraySet

```java
public class CopyOnWriteArraySet<E> extends AbstractSet<E>
        implements Serializable {
    
    private final CopyOnWriteArrayList<E> al;
    
    public CopyOnWriteArraySet() {
        al = new CopyOnWriteArrayList<E>();
    }
    
    // ...
}
```

CopyOnWriteArraySet wraps a CopyOnWriteArrayList to provide Set semantics:

1.  **Internal Implementation:**
    
    - Uses a CopyOnWriteArrayList for storage
    - Adds uniqueness check before adding elements
    - Inherits thread-safety from CopyOnWriteArrayList
2.  **Key Operations:**
    
    - `add(E e)`: Checks uniqueness, then adds to list
    
    ```java
    public boolean add(E e) {
        return al.addIfAbsent(e);
    }
    ```
    
    - `contains(Object o)`: Delegates to list
    
    ```java
    public boolean contains(Object o) {
        return al.contains(o);
    }
    ```
    
3.  **Performance Characteristics:**
    
    - Inherits COW behavior (expensive writes, fast reads)
    - O(n) for add operations (must check for duplicates)
    - Thread-safe without locking for reads

### ConcurrentSkipListMap/Set

```java
public class ConcurrentSkipListMap<K,V> extends AbstractMap<K,V>
        implements ConcurrentNavigableMap<K,V>, Cloneable, Serializable {
    
    private static final Object BASE_HEADER = new Object();
    
    private transient volatile HeadIndex<K,V> head;
    private final Comparator<? super K> comparator;
    
    static final class Node<K,V> {
        final K key;
        volatile Object value;
        volatile Node<K,V> next;
        // ...
    }
    
    static final class Index<K,V> {
        final Node<K,V> node;
        final Index<K,V> down;
        volatile Index<K,V> right;
        // ...
    }
    
    static
```