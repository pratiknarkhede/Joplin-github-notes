# Testing in Spring Boot: A Complete Guide

I'll explain Spring Boot testing in a clear, simple way to help you prepare for interviews. Let's cover JUnit 5, Mockito, and Spring Boot Test step by step.

## Spring Boot Test Basics

Spring Boot provides special tools that make testing easier. The core components are:

### @SpringBootTest

This annotation creates a full application context for your tests. It loads all your Spring beans and configurations.

```java
@SpringBootTest
class MyApplicationTests {
    // Test methods here
}
```

When you use `@SpringBootTest`, Spring Boot starts up almost everything as if your application was running normally. This is good for integration tests but can be slow.

### @WebMvcTest

This is for testing only your web layer (controllers) without starting the full application:

```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void shouldReturnUser() throws Exception {
        // Test code here
    }
}
```

`@WebMvcTest` is faster than `@SpringBootTest` because it only loads the web layer.

### @DataJpaTest

This is for testing your JPA repositories:

```java
@DataJpaTest
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldSaveUser() {
        // Test code here
    }
}
```

`@DataJpaTest` sets up an in-memory database for testing your repositories without affecting your real database.

## JUnit 5 Key Features

JUnit 5 is the testing framework used with Spring Boot. Here are important parts to know:

### Test Lifecycle Annotations

```java
@BeforeAll // Runs once before all tests
@AfterAll // Runs once after all tests
@BeforeEach // Runs before each test
@AfterEach // Runs after each test
@Test // Marks a method as a test
@Disabled // Skips a test
```

### Assertions

Assertions check if your code behaves as expected:

```java
// Basic assertions
assertEquals(expected, actual);
assertTrue(condition);
assertFalse(condition);
assertNull(object);
assertNotNull(object);

// Advanced assertions
assertThrows(Exception.class, () -> { methodThatShouldThrowException(); });
assertTimeout(Duration.ofMillis(100), () -> { slowMethod(); });

// Multiple assertions at once
assertAll(
    () -> assertEquals(1, calculator.add(0, 1)),
    () -> assertEquals(2, calculator.add(1, 1))
);
```

### Parameterized Tests

These let you run the same test with different inputs:

```java
@ParameterizedTest
@ValueSource(strings = {"apple", "banana", "orange"})
void testFruitLength(String fruit) {
    assertTrue(fruit.length() > 3);
}
```

## Mockito Essentials

Mockito helps you create and control fake objects for testing.

### Creating Mocks

There are several ways to create mocks:

```java
// Using @Mock annotation
@Mock
private UserService userService;

// Or creating manually
UserService userService = Mockito.mock(UserService.class);
```

Remember to initialize your mocks with either `MockitoAnnotations.openMocks(this)` or `@ExtendWith(MockitoExtension.class)` at the class level.

### Stubbing Methods

Stubbing means telling your mock what to return when certain methods are called:

```java
// Return a specific value
when(userService.findById(1L)).thenReturn(new User("John"));

// Throw an exception
when(userService.findById(999L)).thenThrow(new NotFoundException());

// Return different values on consecutive calls
when(counter.getCount())
    .thenReturn(1)
    .thenReturn(2)
    .thenReturn(3);
```

### Argument Matchers

When you don't care about the exact argument values:

```java
// Any long value
when(userService.findById(anyLong())).thenReturn(new User());

// Any non-null string
when(validator.isValid(anyString(), any())).thenReturn(true);
```

Common matchers include: `any()`, `anyInt()`, `anyString()`, `argThat()`.

### Verifying Interactions

Check if certain methods were called with specific arguments:

```java
// Verify a method was called exactly once
verify(userService).save(user);

// Verify with specific arguments
verify(userService).deleteById(5L);

// Verify a method was never called
verify(userService, never()).deleteAll();

// Verify a method was called a specific number of times
verify(userService, times(3)).findAll();
```

### MockBean in Spring Boot

`@MockBean` integrates Mockito with Spring Boot's application context:

```java
@SpringBootTest
class UserServiceTest {
    @Autowired
    private UserService userService;
    
    @MockBean
    private UserRepository userRepository;
    
    @Test
    void test() {
        when(userRepository.findById(1L)).thenReturn(Optional.of(new User("John")));
        User user = userService.getUserById(1L);
        assertEquals("John", user.getName());
    }
}
```

## Advanced Mockito Features

### Capturing Arguments

When you need to inspect the arguments passed to a method:

```java
@Test
void shouldSaveUserWithCorrectData() {
    ArgumentCaptor<User> userCaptor = ArgumentCaptor.forClass(User.class);
    
    userService.createUser("John", 30);
    
    verify(userRepository).save(userCaptor.capture());
    User capturedUser = userCaptor.getValue();
    
    assertEquals("John", capturedUser.getName());
    assertEquals(30, capturedUser.getAge());
}
```

### Answer Interface

For more complex stubbing behavior:

```java
when(userRepository.save(any(User.class))).thenAnswer(invocation -> {
    User user = invocation.getArgument(0);
    user.setId(1L); // Simulate ID generation
    return user;
});
```

### Spying on Real Objects

A spy is a partial mock that uses the real object but allows overriding some methods:

```java
List<String> realList = new ArrayList<>();
List<String> spyList = Mockito.spy(realList);

// Real method is called
spyList.add("one");

// Override a method
doReturn(100).when(spyList).size();

assertEquals(1, spyList.size()); // Returns 100, not 1
```

### Mock Static Methods

With Mockito 3.4.0 and above, you can mock static methods:

```java
try (MockedStatic<Utility> mockedStatic = Mockito.mockStatic(Utility.class)) {
    mockedStatic.when(() -> Utility.getCurrentDate()).thenReturn(fixedDate);
    
    // Code that uses Utility.getCurrentDate()
    LocalDate result = myService.processWithDate();
    
    assertEquals(fixedDate, result);
}
```

## Integration Testing

Integration tests check how different parts of your application work together.

### TestRestTemplate

For testing REST APIs:

```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class UserControllerIntegrationTest {
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    void shouldReturnUserById() {
        ResponseEntity<User> response = restTemplate.getForEntity("/users/1", User.class);
        
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals("John", response.getBody().getName());
    }
}
```

### @DirtiesContext

Use this when your test modifies the application context and you want it reset for other tests:

```java
@SpringBootTest
@DirtiesContext(classMode = ClassMode.AFTER_EACH_TEST_METHOD)
class DatabaseModifyingTest {
    // Test methods that change database state
}
```

### Using a Real Database

For database tests with a real database:

```java
@SpringBootTest
@TestPropertySource(properties = {
    "spring.datasource.url=jdbc:mysql://localhost:3306/test_db",
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class RealDatabaseTest {
    // Tests using an actual MySQL database
}
```

## Helper Methods and Best Practices

### Test Helper Methods

Create reusable methods for common test operations:

```java
private User createTestUser(String name, int age) {
    User user = new User();
    user.setName(name);
    user.setAge(age);
    return user;
}

private void assertUserEquals(User expected, User actual) {
    assertEquals(expected.getId(), actual.getId());
    assertEquals(expected.getName(), actual.getName());
    assertEquals(expected.getAge(), actual.getAge());
}
```

### TestConfiguration

Create test-specific beans:

```java
@TestConfiguration
static class TestConfig {
    @Bean
    public PaymentGateway testPaymentGateway() {
        return new TestPaymentGateway();
    }
}
```

## Interview Questions and Answers

Here are some common interview questions about testing in Spring Boot:

### Q1: What is the difference between @MockBean and @Mock?

**Answer**:

- `@Mock` is a pure Mockito annotation that creates a mock object.
- `@MockBean` is a Spring Boot annotation that creates a mock and adds it to the Spring application context, replacing any existing bean of the same type.
- Use `@Mock` for unit tests when you don't need Spring's context.
- Use `@MockBean` for integration tests when you need to replace a bean in Spring's context.

### Q2: How would you test a Spring Boot REST controller?

**Answer**: There are two main approaches:

1.  Using `@WebMvcTest` for focused web layer testing:
    
    ```java
    @WebMvcTest(UserController.class)
    class UserControllerTest {
        @Autowired
        private MockMvc mockMvc;
        
        @MockBean
        private UserService userService;
        
        @Test
        void shouldGetUser() throws Exception {
            when(userService.findById(1L)).thenReturn(new User("John"));
            
            mockMvc.perform(get("/users/1"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("John"));
        }
    }
    ```
    
2.  Using `@SpringBootTest` with `TestRestTemplate` for full integration testing:
    
    ```java
    @SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
    class UserControllerIntegrationTest {
        @Autowired
        private TestRestTemplate restTemplate;
        
        @Test
        void shouldGetUser() {
            ResponseEntity<User> response = restTemplate.getForEntity("/users/1", User.class);
            assertEquals(HttpStatus.OK, response.getStatusCode());
            assertEquals("John", response.getBody().getName());
        }
    }
    ```
    

### Q3: How do you test Spring Data JPA repositories?

**Answer**:

```java
@DataJpaTest
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Test
    void shouldFindByName() {
        // Given
        User user = new User("John", 30);
        entityManager.persist(user);
        entityManager.flush();
        
        // When
        User found = userRepository.findByName("John");
        
        // Then
        assertNotNull(found);
        assertEquals(30, found.getAge());
    }
}
```

The `@DataJpaTest` annotation:

- Sets up an in-memory database
- Configures Hibernate and Spring Data
- Performs entity scanning
- Enables SQL logging

### Q4: How do you handle database tests in Spring Boot?

**Answer**: Spring Boot offers several approaches:

1.  In-memory database: Use H2 or HSQL with `@DataJpaTest`
2.  Test containers: Use Docker containers with real databases
3.  Actual test database: Configure a separate database for testing
4.  Database rollback: Use `@Transactional` to roll back changes after tests

Example with test containers:

```java
@SpringBootTest
@Testcontainers
class DatabaseTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13");
    
    @DynamicPropertySource
    static void registerProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
    
    // Test methods here
}
```

### Q5: Explain the testing pyramid in Spring Boot applications.

**Answer**: The testing pyramid has three layers:

1.  **Unit Tests** (bottom layer): Test individual components like services or utils. They use mocks and are fast.
2.  **Integration Tests** (middle layer): Test how components work together. They may use Spring context and in-memory databases.
3.  **End-to-End Tests** (top layer): Test the entire application flow. They use the real application environment.

The pyramid shape means you should have:

- Many unit tests (fast, focused)
- Some integration tests
- Few end-to-end tests (slow, broad)

This approach gives you fast feedback and good test coverage.

### Q6: How do you test Spring Boot applications with external dependencies like databases and messaging systems?

**Answer**:

1.  For databases: Use in-memory databases (H2) or test containers
2.  For REST APIs: Use MockWebServer or WireMock
3.  For messaging: Use embedded brokers or test containers

Example with WireMock:

```java
@SpringBootTest
class ExternalApiTest {
    @Autowired
    private UserService userService;
    
    WireMockServer wireMockServer = new WireMockServer(8089);
    
    @BeforeEach
    void setup() {
        wireMockServer.start();
        
        wireMockServer.stubFor(get(urlEqualTo("/api/external/users/1"))
            .willReturn(aResponse()
                .withStatus(200)
                .withHeader("Content-Type", "application/json")
                .withBody("{\"id\":1,\"name\":\"John\"}")));
    }
    
    @AfterEach
    void tearDown() {
        wireMockServer.stop();
    }
    
    @Test
    void shouldFetchExternalUser() {
        User user = userService.fetchExternalUser(1L);
        assertEquals("John", user.getName());
    }
}
```

### Q7: How can you set up test data for your Spring Boot tests?

**Answer**: There are several approaches:

1.  Use `@Sql` to run SQL scripts:
    
    ```java
    @SpringBootTest
    @Sql("/test-data.sql")
    class UserServiceIntegrationTest {
        // Tests using data from test-data.sql
    }
    ```
    
2.  Use TestEntityManager in `@DataJpaTest`:
    
    ```java
    @DataJpaTest
    class UserRepositoryTest {
        @Autowired
        private TestEntityManager entityManager;
        
        @BeforeEach
        void setup() {
            entityManager.persist(new User("John", 30));
            entityManager.persist(new User("Jane", 25));
            entityManager.flush();
        }
    }
    ```
    
3.  Create helper methods or builder classes:
    
    ```java
    private User createTestUser() {
        return User.builder()
            .name("Test User")
            .email("test@example.com")
            .role(Role.USER)
            .build();
    }
    ```
    

### Q8: How would you test a service that uses @Scheduled methods?

**Answer**: For testing scheduled methods:

1.  Extract the scheduled logic into a separate method
2.  Make the scheduled method call that separate method
3.  Test the separate method directly

```java
// Original service
@Service
public class ReportService {
    @Scheduled(cron = "0 0 12 * * ?")
    public void generateDailyReport() {
        // Logic here
    }
}

// Refactored service
@Service
public class ReportService {
    @Scheduled(cron = "0 0 12 * * ?")
    public void generateDailyReport() {
        doGenerateDailyReport();
    }
    
    // This method can now be tested directly
    public void doGenerateDailyReport() {
        // Logic here
    }
}

// Test
@SpringBootTest
class ReportServiceTest {
    @Autowired
    private ReportService reportService;
    
    @Test
    void shouldGenerateReport() {
        reportService.doGenerateDailyReport();
        // Assert expected results
    }
}
```

### Q9: What's the difference between @MockBean and @SpyBean?

**Answer**:

- `@MockBean` creates a complete mock that returns default values (null, 0, etc.) unless you specifically stub methods.
- `@SpyBean` creates a partial mock that calls real methods unless you override them.

Example:

```java
@SpringBootTest
class EmailServiceTest {
    // Complete mock - all methods return null by default
    @MockBean
    private UserRepository userRepository;
    
    // Partial mock - real methods are called by default
    @SpyBean
    private EmailSender emailSender;
    
    @Test
    void test() {
        // Stub a method on the mock
        when(userRepository.findById(1L)).thenReturn(Optional.of(new User("John")));
        
        // Override a method on the spy
        doNothing().when(emailSender).send(any());
        
        // Other methods on emailSender will use the real implementation
    }
}
```

### Q10: How do you test Spring Security in a Spring Boot application?

**Answer**: There are several approaches:

1.  Using `@WithMockUser` to mock authentication:
    
    ```java
    @SpringBootTest
    class SecurityTest {
        @Autowired
        private MockMvc mockMvc;
        
        @Test
        @WithMockUser(username = "admin", roles = {"ADMIN"})
        void adminCanAccessSecuredEndpoint() throws Exception {
            mockMvc.perform(get("/admin/dashboard"))
                .andExpect(status().isOk());
        }
        
        @Test
        @WithMockUser(username = "user", roles = {"USER"})
        void userCannotAccessSecuredEndpoint() throws Exception {
            mockMvc.perform(get("/admin/dashboard"))
                .andExpect(status().isForbidden());
        }
    }
    ```
    
2.  Using `SecurityMockMvcRequestPostProcessors`:
    
    ```java
    @SpringBootTest
    class SecurityTest {
        @Autowired
        private MockMvc mockMvc;
        
        @Test
        void loginWithValidCredentials() throws Exception {
            mockMvc.perform(post("/login")
                .with(csrf())
                .param("username", "admin")
                .param("password", "password"))
                .andExpect(status().is3xxRedirection())
                .andExpect(redirectedUrl("/dashboard"));
        }
    }
    ```
    

## Summary

For Spring Boot testing interviews, make sure you understand:

1.  Different test annotations (`@SpringBootTest`, `@WebMvcTest`, `@DataJpaTest`)
2.  JUnit 5 basics (lifecycle, assertions, parameterized tests)
3.  Mockito functionality (creating mocks, stubbing, verification, argument matchers)
4.  Integration testing approaches
5.  How to test different layers (controllers, services, repositories)

Practice explaining these concepts clearly and be ready to write code examples for each one.