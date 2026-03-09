### **1\. SQL Query: Create a New Table Based on an Existing Table**

- **Copy all columns and data** :
    
    &nbsp;
    
    <span style="color: #c678dd;">CREATE</span> <span style="color: #c678dd;">TABLE</span> <span style="color: #e06c75;">student2</span> <span style="color: #c678dd;">AS</span> <span style="color: #c678dd;">SELECT</span> <span style="color: #56b6c2;">\*</span> <span style="color: #c678dd;">FROM</span> <span style="color: #e06c75;">student</span>;
    
- **Copy specific columns** :
    
    &nbsp;
    
    <span style="color: #c678dd;">CREATE</span> <span style="color: #c678dd;">TABLE</span> <span style="color: #e06c75;">student2</span> <span style="color: #c678dd;">AS</span> <span style="color: #c678dd;">SELECT</span> <span style="color: #e06c75;">id</span>, <span style="color: #e06c75;">name</span> <span style="color: #c678dd;">FROM</span> <span style="color: #e06c75;">student</span>;
    
- **Copy only the structure (no data)** :
    
    &nbsp;
    
    <span style="color: #c678dd;">CREATE</span> <span style="color: #c678dd;">TABLE</span> <span style="color: #e06c75;">student2</span> <span style="color: #c678dd;">AS</span> <span style="color: #c678dd;">SELECT</span> <span style="color: #56b6c2;">\*</span> <span style="color: #c678dd;">FROM</span> <span style="color: #e06c75;">student</span> <span style="color: #c678dd;">WHERE</span> <span style="color: #e5c07b;">1</span> <span style="color: #56b6c2;">\=</span> <span style="color: #e5c07b;">0</span>;
    
- **Add constraints or indexes** :
    
    - After creating the table, use `ALTER TABLE` to add constraints or indexes.

&nbsp;

### **2\. Java Program: Find the First Repeating Character in a String Using Functional Programming**

- Use streams and a `HashSet` to track seen characters:
    
    &nbsp;
    
    <span style="color: #e5c07b;">Optional</span><<span style="color: #e5c07b;">Character</span>\> <span style="color: #abb2bf;">firstRepeatingChar</span> <span style="color: #56b6c2;">\=</span> <span style="color: #e06c75;">input</span><span style="color: #56b6c2;">.</span><span style="color: #61afef;">chars</span>()
    
    <span style="color: #56b6c2;">.</span><span style="color: #61afef;">mapToObj</span>(<span style="color: #abb2bf;">c</span> -> (<span style="color: #e5c07b;">char</span>) <span style="color: #e06c75;">c</span>)
    
    <span style="color: #56b6c2;">.</span><span style="color: #61afef;">filter</span>(<span style="color: #abb2bf;">c</span> -> <span style="color: #56b6c2;">!</span><span style="color: #e06c75;">seen</span><span style="color: #56b6c2;">.</span><span style="color: #61afef;">add</span>(<span style="color: #e06c75;">c</span>))
    
    <span style="color: #56b6c2;">.</span><span style="color: #61afef;">findFirst</span>()<span style="color: #abb2bf;">;</span>
    
- Example: For `"programming"`, output is `'r'`.
    

&nbsp;

### <span style="color: #fafafc;">3\. Given an integer</span> `n`<span style="color: #fafafc;">, repeatedly sum its digits until the result becomes a single-digit number. Return this single-digit number.</span>

```java
public class DigitalRoot {
    public static int findDigitalRoot(int n) {
        // Continue summing digits until we get a single-digit number
        while (n >= 10) {
            n = sumDigits(n);
        }
        return n;
    }

    // Helper method to sum the digits of a number
    private static int sumDigits(int num) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10; // Extract the last digit and add to sum
            num /= 10;       // Remove the last digit
        }
        return sum;
    }

    public static void main(String[] args) {
        int n = 1234;
        System.out.println("Digital Root: " + findDigitalRoot(n)); // Output: 1
    }
}
```

&nbsp;

### **4\. Difference Between Collections and Streams**

- **Collections** :
    - Store and manage elements in memory.
    - Allow direct access and modification.
- **Streams** :
    - Process elements in a functional style (e.g., filtering, mapping).
    - Do not store data; operate on existing collections.
    - Lazy evaluation for efficiency.

&nbsp;

### **5\. Internal Working of HashMap**

- **Hashing** :
    - Keys are hashed to determine their bucket index.
- **Collision Handling** :
    - Uses linked lists or balanced trees (in Java 8+) for collisions.
- **Retrieval** :
    - Hash code determines the bucket; `equals()` identifies the exact key.

&nbsp;

### **6\. How many objects are creating in below code?**

- **Code** :
    
    <span style="color: #e5c07b;">String</span> <span style="color: #abb2bf;">s</span> <span style="color: #56b6c2;">\=</span> <span style="color: #c678dd;">new</span> <span style="color: #e5c07b;">String</span>(<span style="color: #98c379;">"abc"</span>)<span style="color: #abb2bf;">;</span> <span style="color: #7d8799;">// Creates 1 object in heap.</span>
    
    <span style="color: #e5c07b;">String</span> <span style="color: #abb2bf;">s2</span> <span style="color: #56b6c2;">\=</span> <span style="color: #98c379;">"abc"</span><span style="color: #abb2bf;">;</span> <span style="color: #7d8799;">// Reuses string pool object.</span>
    

&nbsp;

- **Objects Created** :
    
    - `"abc"` in string pool (if not already present).
    - `new String("abc")` creates a new object in the heap.

&nbsp;