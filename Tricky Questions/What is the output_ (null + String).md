public class Test {  
    public static void main(String\[\] args) {  
        String s = null;  
        System.out.println(s + "test");  
    }  
}

**Output:** `nulltest`

**Explanation:** `null` is treated as the string `"null"` during concatenation.