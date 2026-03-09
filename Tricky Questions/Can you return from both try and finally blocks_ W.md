The value returned from the `finally` block **overrides** the value from the `try` block.

public static String test() {  
    try {  
        return "try";  
    } finally {  
        return "finally";  
    }  
}

public static void main(String\[\] args) {  
    System.out.println(test()); // Output: finally  
}

&nbsp;