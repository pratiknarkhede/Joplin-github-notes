public class Test {  
    public static void main(String\[\] args) {  
        System.out.println(10 + 20 + "30" + 40 + 50);  
    }  
}

Answer:

30304050

**Explanation:**

- `10 + 20 = 30`
    
- `30 + "30" = "3030"` → string context starts
    
- `"3030" + 40 = "303040"`
    
- `"303040" + 50 = "30304050"`