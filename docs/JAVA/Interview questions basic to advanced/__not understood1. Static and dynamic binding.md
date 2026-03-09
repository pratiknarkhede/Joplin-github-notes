Concept related to method invocation  
<br/>They determine when and how the method is call is resolved , either at compile time for runtime

&nbsp;

### 1\. **Static Binding (Early Binding)**

- **Definition**: In static binding, the method to be called is resolved at **compile time**, ==based on the reference type (i.e., the type of the object variable).==
- **Key Characteristics**:
    - Occurs with **static methods**, **private methods**, and **final methods**.
    - These methods **cannot be overridden**, so the compiler knows exactly which method to call.
    - **Faster** since it happens during compilation, and the method call is hardcoded into the bytecode.

consider example we have two classes Animal and Dog which extends Animal  
<br/>Animal class has sound() as private so cannot be overridden

```java
class Animal {
    private void sound() {
        System.out.println("Animal is making a sound");
    }
    
    static void staticMethod() {
        System.out.println("Static method in Animal");
    }
}

```

&nbsp;

```java
class Dog extends Animal {
    private void sound() {
        System.out.println("Dog is barking");
    }
    
    static void staticMethod() {
        System.out.println("Static method in Dog");
    }
}
```

&nbsp;

```java
public class StaticBindingExample {
    public static void main(String[] args) {
        Animal obj = new Dog();
        
        // This will invoke the method in Animal
        // because private methods are statically bound
        obj.sound();
        
        // This will call the static method of the reference type (Animal)
        obj.staticMethod();
    }
}
```

&nbsp;

here we are putting reference of Dog object in Animal object  
but now when we call `obj.sound()` it will call sound() in Animal , because sound() is private   
and statically bound means it will be resolved on class name ,  
<br/>also in case of staticMethod() , since it is static , hence it is statically bound so method in Animal class will be called

* * *

### 2\. **Dynamic Binding (Late Binding)**

- **Definition**: In dynamic binding, the method to be called is resolved at **runtime**, ==based on the **actual object** type, not the reference type.==
- **Key Characteristics**:
    - Occurs with **instance methods** that can be **overridden**.
    - The JVM decides which method to invoke based on the **object type** during execution.
    - Allows **polymorphism**, where a subclass can provide its own method implementation.

**Example of Dynamic Binding:**

```java
class Animal {
    public void sound() {
        System.out.println("Animal is making a sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog is barking");
    }
}

public class DynamicBindingExample {
    public static void main(String[] args) {
        Animal obj = new Dog();
        
        // Dynamic binding: Dog's sound() will be called at runtime
        obj.sound();
    }
}

```

  
<br/> In this example, `sound()` is an instance method that is overridden in the `Dog` class. The actual object is of type `Dog`, so at runtime, the JVM calls the `sound()` method of the `Dog` class, not `Animal`.

&nbsp;

### Key Differences:

| Aspect | Static Binding | Dynamic Binding |
| --- | --- | --- |
| **When resolved** | At compile-time | At runtime |
| **Method type** | Static, private, final methods | Instance methods that can be overridden |
| **Reference vs. Object** | Depends on reference type | Depends on object type |
| **Polymorphism** | Not applicable | Supports polymorphism |
| **Speed** | Faster | Slightly slower due to runtime decision |

&nbsp;

### Summary:

- **Static Binding**: Determined at **compile-time** and used for methods that the compiler can resolve statically (e.g., `static`, `private`, and `final` methods).
- **Dynamic Binding**: Determined at **runtime** based on the **object type** (e.g., overridden instance methods), allowing the same method call to behave differently depending on the actual object's class.