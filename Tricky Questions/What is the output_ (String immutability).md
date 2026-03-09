String s1 = "hello";  
String s2 = s1;  
s1 = s1 + " world";  
System.out.println(s1); // hello world  
System.out.println(s2); // hello

&nbsp;

**Explanation:** Strings are **immutable**. `s1` now refers to a new object.