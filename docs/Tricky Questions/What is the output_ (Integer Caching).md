Integer a = 100;  
Integer b = 100;  
System.out.println(a == b); // true

Integer x = 200;  
Integer y = 200;  
System.out.println(x == y); // false

&nbsp;

**Explanation:**

- Integers between `-128` and `127` are cached.
    
- So `a == b` is true; `x == y` is false (new objects).