public class Test {  
    public static void main(String\[\] args) {  
        String s = "abc";  
        change(s);  
        System.out.println(s);  
    }

&nbsp;   static void change(String str) {  
        str = "xyz";  
    }  
}

**Output:**

`abc`

&nbsp;

**Explanation:**  
Java is **pass-by-value**. The reference is copied, but the original object is not affected.