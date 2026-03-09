The `@ConditionalOnProperty` annotation provides a powerful mechanism for conditional bean registration based on the presence and values of properties in your application's environment (like those in `application.properties` or `application.yml`). It allows you to create flexible configurations that adapt to different environments or runtime conditions.

&nbsp;

**How It Works**

1.  **Target:** You apply this annotation to a configuration class, a bean method within a configuration class, or directly to a component class.
    
2.  **Property Check:** Spring Boot evaluates the specified property (or properties) at runtime. It checks:
    
    - If the property exists in the environment.
    - If the property has a specific value (optional).
3.  **Conditional Registration:**
    
    - If the property check passes (i.e., the property exists and has the desired value, if specified), the annotated bean is registered and becomes part of your application context.
    - If the property check fails, the bean is not registered, effectively disabling that component or configuration.

**Example: Feature Flag**

Imagine you want to enable or disable a "premium features" module in your application based on a configuration property:

```java
@Configuration
@ConditionalOnProperty(name = "app.features.premium", havingValue = "enabled")
public class PremiumFeaturesConfig {
    // ... beans for premium features
}

```

&nbsp;usage:  
<br/>

```java
@Service
public class MyService {

    @Autowired(required = false)  // Allow injection to fail if bean is absent
    private PremiumFeature premiumFeature;

    // ... other service methods

    public void doSomething() {
        if (premiumFeature != null) {
            // Premium feature is enabled, use it
            premiumFeature.somePremiumMethod(); 
        } else {
            // Premium feature is not enabled, provide alternative behavior
        }
    }
}

```

In your `application.properties`:

```yaml
app.features.premium=enabled  // or "disabled"
```

&nbsp;

- If `app.features.premium` is "enabled," the `PremiumFeaturesConfig` is processed, and its beans are created.
- If `app.features.premium` is "disabled" or missing, the configuration class is ignored.

&nbsp;

- `@Autowired(required = false)`: This ensures that if the `PremiumFeature` bean doesn't exist (because the property is not "enabled"), the service still starts up without throwing an exception.
- The `if (premiumFeature != null)` check is crucial to determine if the premium feature is accessible.

&nbsp;

**`@ConditionalOnBean`:** You can alternatively annotate a specific method in your service with `@ConditionalOnBean`, making the method's execution dependent on the presence of a particular premium feature bean.

&nbsp;

```java
@Service
public class MyService {

    @Autowired
    private ApplicationContext context; // Get the application context

    public void doSomething() {
        // ... regular logic
    }

    @ConditionalOnBean(PremiumFeature.class)
    public void doSomethingPremium() {
        // This method is only called if PremiumFeature bean exists
        PremiumFeature premiumFeature = context.getBean(PremiumFeature.class);
        premiumFeature.somePremiumMethod();
    }
}

```

* * *

**Common Use Cases**

- **Feature Flags/Toggles:** Enable/disable features at runtime.
- **Environment-Specific Configurations:** Load different beans based on the environment (e.g., development, testing, production).
- **External Service Integration:** Conditionally configure beans for interacting with external systems if the necessary credentials are provided.

&nbsp;

**Annotation Attributes**

- `name` (or `value`): The name of the property to check.
- `havingValue` (optional): The expected value of the property for the bean to be registered.
- `matchIfMissing` (optional): Controls whether the condition matches if the property is missing (default is `false`).
- `prefix` (optional): A prefix to apply to the property name.

```java
@Component
@ConditionalOnProperty(prefix = "app.db", name = "type", havingValue = "mysql")
public class MySQLDataSource {
    // ... configuration for MySQL data source
}

@Component
@ConditionalOnProperty(prefix = "app.db", name = "type", havingValue = "postgresql")
public class PostgreSQLDataSource {
    // ... configuration for PostgreSQL data source
}

```

&nbsp;