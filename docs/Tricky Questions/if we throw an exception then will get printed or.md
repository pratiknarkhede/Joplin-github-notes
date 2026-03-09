### 💡 What happens when an exception is thrown?

If you throw an exception inside a `try` block:

1.  Java **searches for a matching `catch` block**.
    
2.  If it finds a matching `catch` block, the exception is **caught** and **handled** there.
    
3.  After that, the `finally` block (if present) is **always executed**, regardless of whether the exception was caught or not.
    
4.  If there is no matching `catch` block, the exception will **propagate**, but the `finally` block will still be executed before the method exits.
    

&nbsp;

✅ Example 1: Exception is thrown and caught

try {  
    System.out.println("Inside try");  
    int result = 10 / 0; // This will throw ArithmeticException  
} catch (ArithmeticException e) {  
    System.out.println("Caught exception: " + e.getMessage());  
} finally {  
    System.out.println("Finally block executed");  
}

**Output:**

Inside try  
Caught exception: / by zero  
Finally block executed

&nbsp;

### ✅ Example 2: Exception is thrown but not caught

java

try {  
    System.out.println("Inside try");  
    String str = null;  
    str.length(); // NullPointerException  
} catch (ArithmeticException e) {  
    System.out.println("Caught ArithmeticException");  
} finally {  
    System.out.println("Finally block executed");  
}

&nbsp;

**Output:**

Inside try  
Finally block executed  
Exception in thread "main" java.lang.NullPointerException

&nbsp;

So here, the exception is not caught (no matching catch block), but **finally still runs**, and the exception **propagates** further.

&nbsp;

### ✅ Example 3: No exception thrown

try {  
    System.out.println("Inside try");  
} catch (Exception e) {  
    System.out.println("Caught exception");  
} finally {  
    System.out.println("Finally block executed");  
}

Output:

Inside try  
Finally block executed

### 🔁 Summary:

| Scenario | Catch Block | Finally Block |
| --- | --- | --- |
| Exception thrown and caught | ✅ Yes | ✅ Always |
| Exception thrown but not caught | ❌ No | ✅ Always |
| No exception thrown | ❌ No | ✅ Always |