public class Test {  
    public static void main(String\[\] args) {  
        String a = "Java";  
        String b = new String("Java");  
        System.out.println(a == b); // false  
        System.out.println(a.equals(b)); // true  
    }  
}

**Explanation:**

- `a == b` → false (different memory references)
    
- `a.equals(b)` → true (same content)
    

&nbsp;

&nbsp;