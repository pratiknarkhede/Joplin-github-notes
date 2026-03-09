- `private` methods are **not inherited**, so they **cannot be overridden**.
    
- `static` methods belong to the **class**, not the instance, so they can be **hidden** (method hiding), but not overridden.
    

&nbsp;

class Parent {  
    private void show() { }  
    static void display() {  
        System.out.println("Parent");  
    }  
}

class Child extends Parent {  
    // This is a new method, not an override  
    private void show() { }

&nbsp;   // Method hiding  
    static void display() {  
        System.out.println("Child");  
    }  
}