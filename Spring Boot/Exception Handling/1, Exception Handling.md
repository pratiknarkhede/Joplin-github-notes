#### What is `@ControllerAdvice`?

`@ControllerAdvice` is a specialization of the `@Component` annotation which allows you to handle exceptions globally across all controllers. I==t helps in separating exception handling logic from the main business logic of your application.==

&nbsp;

#### Benefits of Using `@ControllerAdvice`

1.  **Consistent Exception Handling Across Controllers:** Ensures uniform handling of exceptions across multiple controllers.
2.  **Reduces Boilerplate Code:** Centralizes exception handling logic, reducing code duplication.
3.  **Separation of Concerns:** Keeps exception handling logic separate from business logic.
4.  **Handling Common Exceptions:** Manages common exceptions in a unified way.

&nbsp;

&nbsp;

**Exception handling in action:**  
<br/>Step 1: Create a Custom Exception Class

```java
public class CustomException extends RuntimeException {
    public CustomException(String message) {
        super(message);
    }
}
```

&nbsp;

&nbsp;

#### Step 2: Define a Global Exception Handler with `@ControllerAdvice`

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<String> handleCustomException(CustomException ex) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGenericException(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("An unexpected error occurred");
    }
}
```

&nbsp;

&nbsp;

- `@ControllerAdvice`: Marks the class as a global exception handler.
- `@ExceptionHandler`: Used to define methods that handle specific exceptions.
- `handleCustomException`: Handles `CustomException` and returns a 400 Bad Request response with the exception message.
- `handleGenericException`: Handles any other exceptions and returns a 500 Internal Server Error response with a generic message.

#### Creating a Custom Error Response

To standardize error responses, create a custom error response class.

```java
public class ErrorResponse {
    private String message;
    private int status;

    public ErrorResponse(String message, int status) {
        this.message = message;
        this.status = status;
    }

    // getters and setters
}
```

Using Custom Error Response in `@ControllerAdvice`

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomException.class)
    public ResponseEntity<ErrorResponse> handleCustomException(CustomException ex) {
        ErrorResponse errorResponse = new ErrorResponse(ex.getMessage(), HttpStatus.BAD_REQUEST.value());
        return new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        ErrorResponse errorResponse = new ErrorResponse("An unexpected error occurred", HttpStatus.INTERNAL_SERVER_ERROR.value());
        return new ResponseEntity<>(errorResponse, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

&nbsp;

&nbsp;

- `ErrorResponse`: Custom error response object to provide a consistent error structure.
- `handleCustomException` and `handleGenericException`: Methods return `ErrorResponse` objects wrapped in `ResponseEntity`.

&nbsp;

### Use Cases for `@ControllerAdvice`

1.  **Consistent Exception Handling Across Controllers:**
    
    - Example: An API with multiple endpoints where any request can result in a `CustomException`.
2.  **Reducing Boilerplate Code:**
    
    - Example: Handling `ValidationException` in a centralized way for form validation errors across different endpoints.
3.  **Separation of Concerns:**
    
    - Example: Controllers focus on handling requests and business logic, while `@ControllerAdvice` manages all the error responses.
4.  **Handling Common Exceptions:**
    
    - Example: Catching `IllegalArgumentException` and responding with a 400 Bad Request for any controller method.

&nbsp;

&nbsp;