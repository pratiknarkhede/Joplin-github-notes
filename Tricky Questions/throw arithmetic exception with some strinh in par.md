Sure! Below is a full example in Java that:

- **Throws** an `ArithmeticException` with a custom message,
    
- **Catches** the same `ArithmeticException`,
    
- **Executes** the `finally` block.
    

&nbsp;

✅ Java Example

public class ExceptionDemo {  
    public static void main(String\[\] args) {  
        try {  
            System.out.println("Inside try block");  
            throw new ArithmeticException("Custom message: Division by zero is not allowed");  
        } catch (ArithmeticException e) {  
            System.out.println("Caught ArithmeticException: " + e.getMessage());  
        } finally {  
            System.out.println("Finally block executed");  
        }  
    }  
}

### ✅ Output:

Inside try block  
Caught ArithmeticException: Custom message: Division by zero is not allowed  
Finally block executed

&nbsp;

&nbsp;