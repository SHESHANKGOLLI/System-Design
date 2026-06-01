1. You spring boot project taking longer time to get started due to heavy bean initialization. How will you optimize and start the app sooner.
Ans: We do have lazy initialization in the spring boot, what ever is required later point of time we can put those beans as lazy and start the application , in this was we can optimize and start the application sooner.

2. As REST endpoint, now it is returning outed data instead of fresh data. (GET endpoint), how will you debug it.
Ans: The issue lies in cache. We will have to check whether when doing post operations they are updating the cache based on the primary key or not. In that way, we can figure and debug the issue.

3. Two spring boot microserceis and communcication via rest endpoints. One is frequently times out under the load.
Ans: So , Gng timeout due to load, then we need to check whether our load balancer is working or not . Along with that if load balancer is good, then we have to check the DB calls or for loops or we need to use ciricuit breaker.

4. Have an api which is public accessible. Another api wanted to be restrict.
Ans: In security filter chain, we have an option to restrict the endpoint. 
Permit All with authentication.

5. Application works locally, but failed to connect to DB in production.
Ans: What ever the props there in the app.prop are correct or not. We need to cehc the prod profile is binded properly or not, in that way, it will fetch proper.

6. One of the api take too long because it load large data set, how will you improve the performance.
Ans: We can implement the pagination, based on the user input we can send only limited data, In that way performance will be increased,.

7. U have sensitive config, db password or etc. How will you secure them.
Ans: We won't store amny actual values in the application server. We have config server with some encrypted values, from there we will get the values and decrypt the data.

8. When two instance get the task to execute at same time using @Scheduled. 
Ans: @SchedulerLock(
    name = "importantTaskLock", 
    lockAtMostFor = "PT15M", // Maximum time the lock is held if the node dies (15 minutes)
    lockAtLeastFor = "PT30S" // Overcomes time drift by keeping it locked for at least 30 seconds
)

9. You have the rest api for uploading the files. User complaints about saying large files are failing silently.
Ans: We have to tell spring boot api to restrict the file size with app.prop defining a file size.

max-request-size is the limit for the entire HTTP request as a whole, which includes the sum of all files, text form fields, and metadata combined.
Therefore, max-request-size must always be configured to be equal to or larger than max-file-size to accommodate the entire payload wrapper.

spring.servlet.multipart.max-file-size=50MB
# Total request can hold up to 3 files (150MB) plus a 10MB cushion for form metadata
spring.servlet.multipart.max-request-size=160MB

10. After spring boot version upgradation, your custom bean stopped working because your default bean started overriding it.
Ans: To fix it, I would either mark our custom bean with @Primary to give it explicit precedence (So even after upgrading as well, @Primary will have more priority), or set spring.main.allow-bean-definition-overriding=true in the application properties to restore legacy overriding behavior."

11. Your REST api endpoint return empty response, even though data is present in the DB.
Ans: 
The Connection: Jackson's Visibility and Rules
Jackson follows strict rules to determine what fields make it into the final JSON output.

If you are missing getter methods, Jackson's visibility rule drops the fields completely because it cannot "see" them. It assumes the object has no properties, resulting in an empty JSON block ({}).

This Question (@JsonInclude): When you add @JsonInclude(JsonInclude.Include.NON_NULL), you are giving Jackson an explicit instruction: "Even if you can see this field via its getter, check its value. If the value is null, drop it from the output completely."

12. You have 10 spring boot microservices , you have many configs and feature flags etc, how will you handle all these.
Ans: We can create a config server and we can create different yaml file for each microservice w,r,t to each env. So that it will be easy to handle add or update.

13. How will you design the checkout process for an ecommerce-application.
Ans: CheckoutService -PaymentService -OrderService -NotificationService (background to send alerts)

14. When two applications uses the same bean , what does spring boot do and how do you override it.
Ans: We will have @Primary or @Qualifier annaotionl which will help to resolve the dependency injection if two bean of same type, Spring boot will throw BeanDefinitionOverrideException at the start of spring boot application.

15. What happens if we use two repository connection for one entity.
Ans: spring will throw BeanDefinitionOverrideException and we need to use the @Qualifier.

16. @Contoller and @RestController, if i annaote both , what will happend.
Ans: "If you annotate a class with both @Controller and @RestController, the application will start up normally, but @RestController takes precedence.

This happens because @RestController is a meta-annotation that includes @ResponseBody. When Spring detects @ResponseBody, it stops looking for HTML view templates and serializes the return value directly into the HTTP response body as data (like JSON or plain text)."


17. Why string is immutable, with an example.
Ans: Strings are immutable in Java to ensure security, memory efficiency, and thread safety.

Because they cannot be changed, Java can safely cache string literals in the String Constant Pool to save heap memory. It also prevents security vulnerabilities where sensitive data like database credentials or file paths could be modified dynamically, and it makes them inherently thread-safe without needing synchronization blocks."

18. How hashmap works internally before java 8 and after java 8
Ans: 
Before Java 8, if five different items were assigned to Row #3, the manager simply stacked them one after the other in a single file line. To find the last item, you had to physically walk past and check every single package in that line from start to finish.

Java 8 solved this line problem by introducing a rule: "If a line in a single row gets too long, organize it into a structured pyramid."

Now, items start out in a single file line just like before. However, the moment an 8th item collides into that exact same row, Spring and Java automatically rearrange that line into a perfectly balanced Red-Black Tree (a branching pyramid structure).

19. How concurrrent hashmap works differentlty from hashmap and how it handle concurrency.
Ans: concurrencyhashmap handle locking mechanism on the segment level on the index. Each key, value is locked by one thread on one segemnt index level.

20. How are the static variables are different from the non-static variables.
Ans: Static is related to class level and non-static are related to instance level.
Static is stored in Method area and non static belongs to heap area.

21. Design Patterns.
Creational, Structurlal, Behavioural.

22. What is the best way to write the singeton desing pattern.
Ans: The absolute best way to create a Singleton design pattern in Java is using an Enum.
Java ensures that Enum instances are created in a thread-safe manner by default during class loading.

public enum AbsoluteSingleton {
    INSTANCE; // This is your single instance

    // You can add any variables or methods you need here
    private int configurationData = 0;

    public void doSomething() {
        System.out.println("Singleton is working cleanly!");
    }
}

23. Have you ever a get a chance to design a immutable class.
Ans: Immutable class , is defined with final then it is an immutable class.
Our class should be final and all the data variable should be final and private. We should provide only getters.

The Rules for Creating an Immutable Class
Make the class final: This prevents other classes from subclassing it and overriding your methods to make them mutable.

Make all fields private and final: This ensures fields can only be initialized once inside the constructor and cannot be accessed directly from outside the class.

Provide NO setter methods: Do not expose any methods that allow changing field values.

Deep Clone Mutable Fields (Defensive Copying): If your class contains references to mutable objects (like a List, Map, or a custom Date object), you must perform a defensive copy during initialization and retrieval. Never return the original reference directly.

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

// Rule 1: Class is marked final so it cannot be inherited
public final class UserSession {

    // Rule 2: All fields are private and final
    private final String sessionId;
    private final long expiryTimestamp;
    
    // A List is a mutable object! We must handle it carefully.
    private final List<String> permissions;

    // Constructor initializing all final fields
    public UserSession(String sessionId, long expiryTimestamp, List<String> permissions) {
        this.sessionId = sessionId;
        this.expiryTimestamp = expiryTimestamp;
        
        // Rule 4: Defensive Copying during initialization
        // If the calling code modifies the original list passed in here later, 
        // our internal list remains completely unaffected.
        if (permissions != null) {
            this.permissions = new ArrayList<>(permissions);
        } else {
            this.permissions = new ArrayList<>();
        }
    }

    // Rule 3: Only provide Getters, NO Setters!
    public String getSessionId() {
        return sessionId; // Strings are inherently immutable, safe to return directly
    }

    public long getExpiryTimestamp() {
        return expiryTimestamp; // Primitives are copied by value, safe to return directly
    }

    // Rule 4: Defensive Copying/Wrapping during retrieval
    public List<String> getPermissions() {
        // Return an unmodifiable wrapper so calling code gets an UnsupportedOperationException
        // if they try to run .add() or .remove() on this list.
        return Collections.unmodifiableList(permissions);
    }
}

