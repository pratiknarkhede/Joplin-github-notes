try {  
    throw new ArithmeticException();  
} catch (NullPointerException e) {  
    System.out.println("Null");  
} catch (ArithmeticException e) {  
    System.out.println("Arithmetic");  
}

**Output:** `Arithmetic`