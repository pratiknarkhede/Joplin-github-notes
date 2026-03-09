If both the `try` and `finally` blocks contain a `return` statement, **the `return` in `finally` overrides the one in `try` or `catch`**.

✅ Example:

public class ReturnTest {

&nbsp;   public static String testMethod() {  
        try {  
            System.out.println("In try block");  
            return "Returned from try";  
        } finally {  
            System.out.println("In finally block");  
            return "Returned from finally";  
        }  
    }

&nbsp;   public static void main(String\[\] args) {  
        String result = testMethod();  
        System.out.println("Result: " + result);  
    }  
}

&nbsp;

✅ Output:

In try block  
In finally block  
Result: Returned from finally

&nbsp;

### 🔍 Explanation:

- The `try` block **executes** and hits the `return "Returned from try"`.
    
- Before the method actually **returns**, the `finally` block is always executed.
    
- Since the `finally` block also has a `return`, **it overrides** the previous return.
    
- So `"Returned from finally"` is the final returned value.
    

&nbsp;

### 🚫 Best Practice Warning:

Using `return` in a `finally` block is **not recommended** because:

- It makes code **confusing** and **hard to debug**.
    
- It can **suppress exceptions** or other logic from the `try` or `catch`.
    

&nbsp;

&nbsp;