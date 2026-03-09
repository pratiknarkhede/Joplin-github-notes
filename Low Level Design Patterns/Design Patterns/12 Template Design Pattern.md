### **Essence of Template Method Pattern**

1.  **What it does:**  
    Defines a ==**skeleton algorithm** in a base class, letting subclasses override specific steps without changing the structure.==
2.  **Key Idea:**
    - **Reusable workflow** with customizable parts.
    - "Don’t call us, we’ll call you" (Hollywood Principle).

* * *

### **Simple Example: Pizza Maker**

Imagine a pizza-making process with fixed steps, but customizable toppings.

#### **1\. Abstract Template Class**

```java
public abstract class PizzaMaker {
    // Template method (FINAL to prevent overriding)
    public final void makePizza() {
        prepareDough();
        addSauce();
        addToppings();  // Subclasses implement this
        bake();
        serve();
    }

    // Fixed steps (common to all pizzas)
    private void prepareDough() {
        System.out.println("Rolling out dough...");
    }

    private void addSauce() {
        System.out.println("Adding tomato sauce...");
    }

    private void bake() {
        System.out.println("Baking at 200°C for 15 mins...");
    }

    private void serve() {
        System.out.println("Pizza ready! 🍕");
    }

    // Customizable step (to be implemented by subclasses)
    protected abstract void addToppings();
}
```

#### **2\. Concrete Implementations**

```java
public class MargheritaPizza extends PizzaMaker {
    @Override
    protected void addToppings() {
        System.out.println("Adding mozzarella and basil...");
    }
}

public class PepperoniPizza extends PizzaMaker {
    @Override
    protected void addToppings() {
        System.out.println("Adding pepperoni and cheese...");
    }
}
```

#### **3\. Client Code**

```java
public class PizzaShop {
    public static void main(String[] args) {
        PizzaMaker margherita = new MargheritaPizza();
        margherita.makePizza();  // Uses the template

        PizzaMaker pepperoni = new PepperoniPizza();
        pepperoni.makePizza();
    }
}
```

**Output:**

```
Rolling out dough...
Adding tomato sauce...
Adding mozzarella and basil...  // Custom step
Baking at 200°C for 15 mins...
Pizza ready! 🍕
```

* * *

### **Key Takeaways from the Example**

1.  **Fixed Workflow:**  
    `makePizza()` enforces the order of steps (dough → sauce → toppings → bake).
2.  **Customizable Parts:**  
    Subclasses only worry about `addToppings()`.
3.  **No Duplication:**  
    Common logic (baking, serving) stays in the base class.

* * *

### ==**Template Pattern in Spring**==

Spring uses this pattern extensively to simplify repetitive tasks. Here’s how:

#### **1\. `JdbcTemplate` (Database Operations)**

**Problem:**  
JDBC code has fixed steps (open connection, execute query, close resources), but the query logic varies.

**Solution:**  
`JdbcTemplate` handles the boilerplate, letting you focus on the SQL.

**Example:**

```java
@Repository
public class UserDao {
    private final JdbcTemplate jdbcTemplate;

    public UserDao(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public List<User> getAllUsers() {
        // You only provide the SQL + row mapping logic
        return jdbcTemplate.query(
            "SELECT * FROM users",
            (rs, rowNum) -> new User(rs.getInt("id"), rs.getString("name"))
        );
    }
}
```

**Template Steps in `JdbcTemplate`:**

1.  Open connection.
2.  Execute query.
3.  Map results (custom logic via `RowMapper`).
4.  Handle exceptions.
5.  Close resources.

* * *

#### **2\. `RestTemplate` (HTTP Requests)**

**Problem:**  
HTTP calls involve fixed steps (create request, send, parse response), but the URL and response type vary.

**Solution:**  
`RestTemplate` handles the boilerplate.

**Example:**

```java
@Service
public class WeatherService {
    private final RestTemplate restTemplate;

    public WeatherService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public Weather fetchWeather(String city) {
        // You only provide the URL + response type
        return restTemplate.getForObject(
            "https://api.weather.com/v1/" + city,
            Weather.class
        );
    }
}
```

**Template Steps in `RestTemplate`:**

1.  Build HTTP request.
2.  Execute request.
3.  Parse response (custom logic via `ResponseEntity`).
4.  Handle errors.

* * *

Let’s explore **RabbitMQTemplate** and other commonly used templates in Spring, breaking down how they leverage the **Template Method Pattern** to simplify complex workflows. We’ll use practical examples and analogies to solidify understanding.

* * *

### **3\. RabbitMQTemplate (Spring AMQP)**

#### **What it Does:**

Simplifies sending/receiving messages via RabbitMQ by handling connection management, serialization, and error handling.

#### **Template Pattern in Action:**

- **Fixed Steps:**
    1.  Establish connection to RabbitMQ.
    2.  Convert Java objects to messages (serialization).
    3.  Publish to an exchange/queue.
    4.  Handle acknowledgments/errors.
- **Customizable Parts:**
    - Message content.
    - Routing keys.
    - Callbacks (e.g., confirmations).

#### **Example: Sending a Message**

```java
@Service
public class OrderService {
    private final RabbitTemplate rabbitTemplate;

    public OrderService(RabbitTemplate rabbitTemplate) {
        this.rabbitTemplate = rabbitTemplate;
    }

    public void placeOrder(Order order) {
        // You only define WHAT to send (order) and WHERE (routing key)
        rabbitTemplate.convertAndSend(
            "orders.exchange",  // Exchange
            "order.created",    // Routing key
            order               // Payload (auto-serialized to JSON)
        );
    }
}
```

#### **Example: Receiving a Message**

```java
@RabbitListener(queues = "orders.queue")
public void handleOrder(Order order) {
    System.out.println("Received order: " + order.getId());
}
```

**Key Point:**  
You focus on business logic (`handleOrder`), while `RabbitTemplate` manages connections, threading, and deserialization.

* * *

### **4\. KafkaTemplate (Spring Kafka)**

#### **What it Does:**

Simplifies producing/consuming messages with Apache Kafka.

#### **Template Pattern in Action:**

- **Fixed Steps:**
    1.  Create a producer/consumer.
    2.  Serialize/deserialize messages.
    3.  Handle batching/retries.
- **Customizable Parts:**
    - Topic names.
    - Message payloads.
    - Listeners (for consumers).

#### **Example: Sending a Message**

```java
@Service
public class NotificationService {
    private final KafkaTemplate<String, String> kafkaTemplate;

    public NotificationService(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendAlert(String message) {
        // You define WHAT (message) and WHERE (topic)
        kafkaTemplate.send("alerts.topic", message);
    }
}
```

#### **Example: Receiving a Message**

```java
@KafkaListener(topics = "alerts.topic")
public void listen(String alert) {
    System.out.println("ALERT: " + alert);
}
```

* * *