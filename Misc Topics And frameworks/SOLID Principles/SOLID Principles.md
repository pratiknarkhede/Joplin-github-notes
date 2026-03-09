&nbsp;

* * *

### **S - Single Responsibility Principle (SRP)**

- **Idea:** One class = One job.
    
- **Simple:** A class should only do one specific thing. If it does too much, split it up.
    
- **Example:** Instead of one Report class doing formatting, data fetching, and emailing, have separate classes: ReportFormatter, DataFetcher, EmailSender.
    

* * *

### **O - Open/Closed Principle (OCP)**

- **Idea:** Open to add, Closed to change.
    
- **Simple:** You should be able to add new features without changing existing, working code. Use interfaces or abstract classes to allow new parts to plug in.
    
- **Example:** Instead of editing PaymentProcessor every time you add Visa/Mastercard/PayPal, make it use a PaymentMethod interface. New payment types just implement that interface.
    

* * *

### **L - Liskov Substitution Principle (LSP)**

- **Idea:** Subtypes must be substitutable for their base types.
    
- **Simple:** If you have a class Child that inherits from Parent, you should be able to use Child anywhere you could use \`Parent\* without breaking the program. The child shouldn't break the parent's rules or promises.
    
- **Example:** If Bird has a fly() method, a Penguin subclass shouldn't just throw an error for fly(). Better: Don't force Penguin to inherit fly(). Maybe have FlyingBird and NonFlyingBird.
    

* * *

### **I - Interface Segregation Principle (ISP)**

- **Idea:** Don't force classes to implement stuff they don't use.
    
- **Simple:** Keep interfaces small and focused. Classes should only need to know about methods relevant to them. Avoid "fat" interfaces.
    
- **Example:** Instead of one big Worker interface with work(), eat(), sleep(), maybe a Robot only needs work(). Split into smaller interfaces like Workable, Eatable. Robot implements only Workable.
    

* * *

### **D - Dependency Inversion Principle (DIP)**

- **Idea:** Depend on abstractions (interfaces), not concrete classes.
    
- **Simple:** High-level parts of your code shouldn't depend directly on low-level, specific parts. Both should depend on interfaces or abstract classes. Think "coding to the contract, not the implementation".
    
- **Example:** Instead of Button directly knowing about a Light bulb class, make Button depend on a Switchable interface. Then Light, Fan, etc., can all implement Switchable and work with the Button.
    

* * *

### **Quick Summary (Even Simpler!)**

- **S (SRP):** One job per class.
    
- **O (OCP):** Add features without changing old code.
    
- **L (LSP):** Subclasses shouldn't break the parent's contract.
    
- **I (ISP):** Small, specific interfaces are better.
    
- **D (DIP):** Depend on interfaces, not specific classes.
    

* * *