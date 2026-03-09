class A {  
    static {  
        System.out.println("Static Block");  
    }  
}  
public class Test {  
    public static void main(String\[\] args) {  
        A obj = new A();  
    }  
}

**Output:** `Static Block` (executed once when class is loaded)