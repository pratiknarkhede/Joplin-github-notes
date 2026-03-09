&nbsp; 

## ✅ Goal:

We want to make sure that when someone sends a POST request to create a user, the data is valid — for example, name should not be blank and email should be valid.

* * *

## 🧱 Step 1: Add Dependency (if not already there)

Make sure your `pom.xml` includes validation starter:

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-validation</artifactId>  
</dependency>  
```

This gives you access to Bean Validation annotations like `@NotBlank`, `@Email`, etc.

* * *

## 📦 Step 2: Create a DTO (Data Transfer Object)

Create a class to receive the request body:

```java
public class UserDTO {

&nbsp;   @NotBlank(message = "Name is required")  
    private String name;

&nbsp;   @Email(message = "Email should be valid")  
    private String email;

&nbsp;   // Getters and setters

&nbsp;   public String getName() {  
        return name;  
    }

&nbsp;   public void setName(String name) {  
        this.name = name;  
    }

&nbsp;   public String getEmail() {  
        return email;  
    }

&nbsp;   public void setEmail(String email) {  
        this.email = email;  
    }  
}  
```

* * *

## 🌐 Step 3: Use `@Valid` in Controller

Now, use `@Valid` in your controller method to validate the input:

```java
@RestController  
@RequestMapping("/api/users")  
public class UserController {

&nbsp;   @PostMapping  
    public ResponseEntity<?> createUser(@Valid @RequestBody UserDTO userDTO, BindingResult result) {

&nbsp;       if (result.hasErrors()) {  
            return ResponseEntity.badRequest().body("Validation failed: " + result.getAllErrors());  
        }

&nbsp;       return ResponseEntity.ok("User is valid!");  
    }  
}  
```

### 🔍 What’s happening here?

- `@Valid`: Tells Spring to validate the `userDTO` object based on the rules defined (like `@NotBlank`, `@Email`)
- `BindingResult`: Holds any validation errors that occur
- If there are errors, we return a `400 Bad Request` with error messages
- If no errors, we proceed normally

* * *

## 🧪 Step 4: Try It Out

Send a POST request using Postman or curl:

### ❌ Invalid Example (name is blank, email invalid):

```json
{  
  "name": "",  
  "email": "not-an-email"  
}  
```

**Response:**

```
Validation failed: [Field error in object 'userDTO' on field 'name': ...  
Field error in object 'userDTO' on field 'email': ...]  
```

### ✅ Valid Example:

```json
{  
  "name": "Alice",  
  "email": "alice@example.com"  
}  
```

**Response:**

```
User is valid!  
```

* * *

## 🎯 Summary

| Part | Purpose |
| --- | --- |
| `@NotBlank`, `@Email` | Define validation rules |
| `@Valid` | Triggers validation |
| `BindingResult` | Captures validation errors |

* * *

Let me know if you want to add custom error responses or handle validation globally using `@ControllerAdvice`!