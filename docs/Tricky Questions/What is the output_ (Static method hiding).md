class A {  
    static void print() {  
        System.out.println("A");  
    }  
}

class B extends A {  
    static void print() {  
        System.out.println("B");  
    }  
}

public class Test {  
    public static void main(String\[\] args) {  
        A obj = new B();  
        obj.print(); // Output: A  
    }  
}

&nbsp;

**Explanation:** Static methods are **not polymorphic** — resolved at **compile time**.