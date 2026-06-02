1. Throughput & Latency
2. Bottleneck
3. Load Balancing
4. Rate Limiting
5. CDN & Caching Techniques
6. Scalability (Horizontal vs. Vertical)
7. CAP Theorem & Consistency
8. Idempotence
9. Availability, Fault Tolerance, & Reliability

Part 2: How to Explain Each Concept
When discussing these individual concepts during the deep dive, use clear, concise definitions and real-world context.

1. Throughput & Latency
What it is: Throughput is how much work the system can do in a given time (e.g., Queries Per Second/QPS). Latency is how long it takes for a single request to complete (measured in milliseconds).

What to tell the interviewer: "We want to maximize throughput while keeping latency as low as possible. High latency ruins user experience, even if throughput is high."

2. Bottleneck
What it is: The slowest part of your system that limits the overall capacity. If your backend can handle 10,000 QPS but your database can only handle 500 QPS, your database is the bottleneck.

What to tell the interviewer: "I am identifying the bottleneck here—which is database disk I/O—so I can introduce a cache to alleviate the load."

3. Load Balancing
What it is: A device or service that sits in front of your servers and routes incoming client requests across multiple backend servers.

What to tell the interviewer: "I will use a Load Balancer (like NGINX or AWS ALB) to distribute incoming traffic. This prevents any single server from becoming a bottleneck or crashing."

4. Rate Limiting
What it is: A strategy to limit the number of requests a user or API client can make within a certain timeframe (e.g., 100 requests per minute).

What to tell the interviewer: "I will implement a Rate Limiter using a Redis token bucket algorithm. This achieves two things: it protects our Backend (less load on BE) from DoS attacks and ensures Fair Usage among all users."

5. CDN & Caching Techniques
What it is: Caching stores temporary data in fast memory (like Redis). A CDN (Content Delivery Network) is a distributed cache network that stores static files (images, videos) physically closer to the user.

What to tell the interviewer: "To reduce database load and latency, I'll cache popular user profiles in Redis. For static media files, I will use a CDN like Cloudflare so the requests never even hit our core backend servers."

6. Scalability (Horizontal vs. Vertical)
What it is: The ability of a system to handle growing amounts of work. Vertical means buying a bigger machine; Horizontal means adding more standard machines.

What to tell the interviewer: "Instead of scaling vertically (which has a hard hardware ceiling), I will design this system to scale horizontally so we can add or remove nodes dynamically based on traffic."

7. CAP Theorem & Consistency
What it is: In a distributed system with unavoidable network partitions (P), you must choose between absolute Consistency (everyone sees the exact same data instantly, but might have to wait/error out) or high Availability (the system always responds, but the data might be slightly stale).

What to tell the interviewer: "Per the CAP Theorem, since we are designing a social media feed, I will prioritize Availability over strict Consistency. It is okay if a user sees a post 2 seconds late, as long as the app doesn't crash."

8. Idempotence
What it is: An operation is idempotent if executing it multiple times yields the exact same result as executing it once.

What to tell the interviewer: "In distributed systems, networks fail. If a user clicks 'Pay Now', the request might succeed on the server but fail on the way back. If the user clicks it again, an Idempotent API (using a unique request ID) ensures they are only charged once."

9. Availability, Fault Tolerance, & Reliability
These three are highly related but distinct:

Reliability: Does the system do what it’s supposed to do over time without errors? (Measured as a probability of success).

Fault Tolerance: The mechanism to survive a failure. The ability of a system to continue operating properly even if a component (like a server or hard drive) dies.

Availability: The percentage of time the system is up and running (e.g., "99.99% uptime").

What to tell the interviewer: "To ensure high Reliability and maximize Uptime (Availability), the system must be Fault Tolerant. I will design this by replicating our services across multiple data centers so there is no single point of failure."