ENTERPRISE DATA MODELING STUDY GUIDE

Target: Senior / Lead Java Engineer (8+ Years Experience)
SECTION 1: NOSQL CORE DESIGN PHILOSOPHY

In Relational databases (SQL), your primary goal is normalization (reducing data redundancy by splitting data into separate tables).

In Document NoSQL (like MongoDB), your primary goal is query optimization, which often means denormalization (embedding data together inside a single document to prevent expensive runtime JOIN operations).

The "Rules of Thumb" Checklist
Embed Data When:

The data belongs exclusively to the parent entity.

The relationship is a strict 1-to-1 or a predictably bounded 1-to-Many (e.g., a few shipping addresses per user).

The child data is read together with the parent data almost every time.

Reference Data When:

The child data stands alone as an independent business entity.

The relationship is an unbounded 1-to-Many that could grow infinitely (e.g., thousands of comments on a single video).

The document size risks exceeding MongoDB's strict 16MB storage limit.

The data is modified constantly by multiple different asynchronous microservices.

SECTION 2: PRODUCTION JAVA IMPLEMENTATIONS
Pattern 1: The Embedding Model (Bounded 1-to-Many)
Scenario: A user profile possessing a small, bounded collection of shipping addresses. The address data lives natively within the users collection block on disk.

Java
package com.example.model;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import java.util.List;

@Document(collection = "users") 
public class User {
    @Id
    private String id;
    private String name;
    private String email;
    
    // EMBEDDED: Serialized directly as an array of nested objects
    private List<Address> shippingAddresses;

    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public List<Address> getShippingAddresses() { return shippingAddresses; }
    public void setShippingAddresses(List<Address> shippingAddresses) { this.shippingAddresses = shippingAddresses; }
}

class Address {
    private String street;
    private String city;
    private String zipCode;
    private boolean isDefault;

    public String getStreet() { return street; }
    public void setStreet(String street) { this.street = street; }
    public String getCity() { return city; }
    public void setCity(String city) { this.city = city; }
    public boolean isDefault() { return isDefault; }
    public void setDefault(boolean aDefault) { isDefault = aDefault; }
}
Pattern 2: The Referencing Model (Unbounded 1-to-Many with Lazy Loading)
Scenario: A video streaming document containing user comments. Because comments can scale infinitely, they are split into a separate collection to safeguard JVM heap memory and disk storage blocks.

Java
package com.example.model;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.DocumentReference;
import java.util.List;

@Document(collection = "videos") 
public class Video {
    @Id
    private String id;
    private String title;
    private String videoUrl;
    private long viewCount;

    // REFERENCED: Only an array of String IDs is saved within this parent document.
    // CRITICAL: lazy = true prevents loading thousands of comments into memory on a generic lookup.
    @DocumentReference(lazy = true) 
    private List<Comment> comments;

    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
    public List<Comment> getComments() { return comments; }
    public void setComments(List<Comment> comments) { this.comments = comments; }
}

@Document(collection = "comments") 
public class Comment {
    @Id
    private String id;
    private String userId;
    private String commentText;
    private long timestamp;

    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    public String getCommentText() { return commentText; }
    public void setCommentText(String commentText) { this.commentText = commentText; }
}
SECTION 3: ARCHITECTURAL INTERVIEW QUESTIONS
Question 1: What happens under the hood if we change @DocumentReference(lazy = true) to lazy = false?
If lazy = false is active, you turn on Eager Loading inside the NoSQL framework.

The Execution Flow: When you call videoRepository.findById("video_123"), Spring Data executes two immediate sequential database reads. First, it grabs the target video document. Second, it instantly extracts the internal array of reference IDs and fires an automated db.comments.find() batch query.

The Production Threat: If a trending video has 10,000 comments, running a query to simply fetch the video title for a homepage dashboard will silently pull all 10,000 comment documents off the network. This causes immediate high Garbage Collection pressure, network I/O bottlenecks, and potential OutOfMemoryErrors (OOM).

Question 2: How does this look in relational SQL databases versus NoSQL Document Stores?
NoSQL (Embedded): Pulls both parent metadata and child lines in exactly one disk read operation. Everything lives within a single contiguous block of data on disk.

SQL Databases (ORM): Relies heavily on fetching strategies configured via Hibernate:

Lazy Fetching: Initially pulls only the parent row. It instantiates a fake dynamic runtime placeholder (a Hibernate Proxy) for the child collection, firing follow-up queries only if your code explicitly calls the list within an open database transaction session.

Eager Fetching: Pulls everything immediately, usually using automated structural metadata joins or immediate secondary batch sweeps.

Question 3: How does the N+1 Query Problem impact SQL fetching strategies?
The N+1 problem occurs when an application executes 1 query to fetch a list of parent records, followed by separate queries to load the children for each individual row.

In LAZY Fetching: It happens incrementally if you loop through a parent collection in your Java service layer and accidentally access the lazy proxy array inside the loop body (order.getItems()).

In EAGER Fetching: It can happen instantly on the very first line of execution if you pull a list using a standard un-optimized query (like findAll()), as Hibernate realizes it must reconcile the eager state of all child entries immediately.

The Senior Resolution: Both scenarios are cleanly solved by explicitly overriding default fetching loops with a targeted JOIN FETCH query or an Entity Graph statement in your repository layer, condensing the database operation into a single network round-trip.