String s = null;  
if (s != null && s.length() > 0) {  
    System.out.println("Not empty");  
}

&nbsp;

**Output:** No exception. Nothing printed.

**Explanation:** `s != null` is false, so short-circuiting prevents `s.length()` from executing.