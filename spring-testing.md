# Random notes on testing with Spring
## 1. Testing configuration properties in Spring Boot:

For example, let's assume we have the following properties:
```yml
myApp:
  prop1: value1
  prop2: value2
```

And the following Configuration class
```java
@ConfigurationProperties(prefix = "myApp")
@ConstructorBinding
public class MyAppConfigurationProps {
    
    private final String prop1;
    private final String prop2;

    public MyAppConfigurationProps(String prop1, String prop2) {
        this.prop1 = prop1;
        this.prop2 = prop2;
    }

    public String getProp1() {
        return prop1;
    }

    public String getProp2() {
        return prop2;
    }
}
```

Then the test would look something like this:

```java
@SpringBootTest(classes = MyAppConfigurationProps.class)
@ActiveProfiles({"test"}) // this implies application-test.yml file in test resources is available and has test prop values
@EnableConfigurationProperties(MyAppConfigurationProps.class)
public MyAppConfigurationPropsTest {

    @Autowired
    private MyAppConfigurationProps props;

    @Test
    public void testProps() {
        assertEquals("value1", props.getProp1());
        assertEquals("value2", props.getProp2()); 
    }
}
```