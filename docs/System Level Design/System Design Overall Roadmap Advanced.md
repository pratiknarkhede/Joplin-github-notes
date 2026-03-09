# System Design Learning Roadmap

**Goal:** Master the fundamentals of system design for interviews and practical application.

**How to Use This Roadmap in Joplin:**

1.  Keep this note as your central roadmap.
2.  For each major topic (e.g., Scalability, Database Design), create a *new Notebook* in Joplin.
3.  As you study a specific sub-topic (e.g., Vertical Scaling), create *notes within that Notebook* detailing concepts, diagrams, trade-offs, and examples.
4.  Check the box (`- [x]`) next to the sub-topic in *this roadmap note* once you feel confident about it.

---

## Phase 1: Core Concepts (Foundation First)

*Master these foundational building blocks. They appear in nearly every system design discussion.*

### Scalability

*   **Why It's Key:** Essential for designing systems that handle growing user demand. Expected in almost every interview.
*   **Example Question:** "How would you scale a system like Twitter/Instagram/TinyURL to handle X million users?"
*   **What to Learn:**
    *   - [ ] **Vertical Scaling (Scaling Up):** Increasing single server capacity (CPU, RAM, SSD).
        *   - [ ] Understand Pros (Simplicity) & Cons (Single Point of Failure, Hardware Limits).
    *   - [ ] **Horizontal Scaling (Scaling Out):** Adding more servers to distribute load.
        *   - [ ] Understand Pros (Elasticity, Fault Tolerance) & Cons (Complexity).
        *   - [ ] Stateless vs. Stateful services impact on horizontal scaling.
    *   - [ ] **Load Balancing:** Distributing incoming requests across multiple servers.
        *   - [ ] Understand different algorithms (Round Robin, Least Connections, Least Response Time, IP Hash).
        *   - [ ] Understand layers (L4 vs L7 load balancing).
        *   - [ ] Know common tools (Nginx, HAProxy, ELB/ALB/NLB).
    *   - [ ] **Caching:** Storing frequently accessed data temporarily for faster retrieval. (See dedicated Caching section below for details).
    *   - [ ] **Database Scaling Strategies:** (See dedicated Database section below for details like Sharding, Replication).

### Database Design & Storage

*   **Why It's Key:** Choosing the right data storage is critical for performance, scalability, and consistency. You *must* be able to justify your choices.
*   **Example Question:** "How would you store user profiles, posts, and relationships for a social media app?"
*   **What to Learn:**
    *   - [ ] **SQL Databases (Relational):** Structured data, tables, relationships, ACID properties.
        *   - [ ] Common examples (MySQL, PostgreSQL, SQL Server, Oracle).
        *   - [ ] Understand core concepts: Schemas, Tables, Indexes, Foreign Keys.
        *   - [ ] When to use SQL (Structured data, transactional integrity needs).
    *   - [ ] **NoSQL Databases (Non-Relational):** Flexible schemas, various data models (Key-Value, Document, Column-Family, Graph), BASE properties.
        *   - [ ] **Key-Value Stores:** (e.g., Redis, Memcached) - Use cases: Caching, session management.
        *   - [ ] **Document Databases:** (e.g., MongoDB, Couchbase) - Use cases: User profiles, content management, catalogs.
        *   - [ ] **Wide-Column Stores:** (e.g., Cassandra, HBase) - Use cases: High-write scenarios, time-series data, analytics.
        *   - [ ] **Graph Databases:** (e.g., Neo4j, Amazon Neptune) - Use cases: Social networks, recommendation engines, fraud detection.
        *   - [ ] When to use NoSQL (Unstructured/semi-structured data, high scalability/availability needs, flexible schema).
    *   - [ ] **Indexing:** Strategies to speed up query performance.
        *   - [ ] Understand B-Trees, Hash Indexes.
        *   - [ ] Concept of Primary Keys, Secondary Indexes, Composite Indexes.
        *   - [ ] Trade-offs (Faster reads vs. slower writes/updates, storage overhead).
    *   - [ ] **Normalization & Denormalization:**
        *   - [ ] Normalization: Reducing data redundancy, improving data integrity (SQL focus). Understand 1NF, 2NF, 3NF basics.
        *   - [ ] Denormalization: Intentionally adding redundancy to improve read performance (often used in NoSQL or for specific SQL optimization).
    *   - [ ] **Database Replication:** Copying data across multiple database servers.
        *   - [ ] Purpose: High Availability, Fault Tolerance, Read Scaling.
        *   - [ ] Master-Slave Replication.
        *   - [ ] Master-Master Replication.
        *   - [ ] Read Replicas.
    *   - [ ] **Database Sharding (Partitioning):** Splitting data across multiple databases/servers.
        *   - [ ] Purpose: Horizontal scaling for very large datasets or high throughput.
        *   - [ ] Sharding Strategies (Range-based, Hash-based, Directory-based).
        *   - [ ] Challenges (Hotspots, Resharding, Cross-shard queries).

### API Design (Application Programming Interface)

*   **Why It's Key:** APIs are the contracts that allow different system components (frontend, backend, services) to communicate.
*   **Example Question:** "Design the API endpoints for a basic e-commerce checkout process." or "Compare REST vs. GraphQL for a mobile app backend."
*   **What to Learn:**
    *   - [ ] **RESTful Principles:** Using standard HTTP methods (GET, POST, PUT, DELETE, PATCH), status codes (2xx, 3xx, 4xx, 5xx), stateless communication.
        *   - [ ] Resource Naming Conventions.
        *   - [ ] Idempotency concept (GET, PUT, DELETE).
    *   - [ ] **API Gateways:** A single entry point for API requests.
        *   - [ ] Functions: Routing, Authentication, Rate Limiting, Caching, Request/Response Transformation, Monitoring.
        *   - [ ] Examples (AWS API Gateway, Apigee, Kong).
    *   - [ ] **Rate Limiting:** Protecting APIs from abuse or overload.
        *   - [ ] Algorithms (Token Bucket, Leaky Bucket, Fixed Window, Sliding Window).
        *   - [ ] Where to implement (API Gateway, individual service).
    *   - [ ] **API Versioning:** Managing changes to APIs without breaking existing clients.
        *   - [ ] Strategies (URI Path `/v1/`, Query Parameter `?v=1`, Custom Header `Accept-Version: v1`).
    *   - [ ] **Data Formats:**
        *   - [ ] JSON (Most common).
        *   - [ ] XML (Less common now, but good to know).
        *   - [ ] Protocol Buffers (gRPC).
    *   - [ ] **Authentication & Authorization:** (See Security section for details like OAuth, JWT). How it integrates with APIs.
    *   - [ ] **Alternative API Styles (Awareness):**
        *   - [ ] GraphQL: Query language for APIs, client specifies data needed.
        *   - [ ] gRPC: High-performance RPC framework (uses HTTP/2, Protocol Buffers).
        *   - [ ] WebSockets: For real-time, bidirectional communication.

### Caching

*   **Why It's Key:** Significantly improves latency and reduces load on backend systems/databases. A fundamental optimization technique.
*   **Example Question:** "Where would you add caching to improve the performance of a news feed?" or "Explain cache invalidation strategies."
*   **What to Learn:**
    *   - [ ] **When & What to Cache:** Identifying frequently accessed, relatively static data ("hot data").
    *   - [ ] **Cache Locations:**
        *   - [ ] **Client-Side Caching:** Browser cache.
        *   - [ ] **CDN Caching:** Edge locations closer to users (for static assets).
        *   - [ ] **Server-Side Caching:**
            *   - [ ] **In-Memory Cache:** Within the application server (e.g., simple dictionary/map, Ehcache).
            *   - [ ] **Distributed Cache:** Separate caching layer shared by multiple servers (e.g., Redis, Memcached).
    *   - [ ] **Cache Eviction Policies:** Deciding what data to remove when the cache is full.
        *   - [ ] LRU (Least Recently Used).
        *   - [ ] LFU (Least Frequently Used).
        *   - [ ] FIFO (First-In, First-Out).
        *   - [ ] Random.
    *   - [ ] **Cache Invalidation Strategies:** Keeping cached data consistent with the source of truth (database).
        *   - [ ] **Write-Through Cache:** Write to cache and DB simultaneously. Pro: Consistency. Con: Higher write latency.
        *   - [ ] **Write-Back Cache (Write-Behind):** Write to cache first, then asynchronously to DB. Pro: Low write latency. Con: Risk of data loss on cache failure before DB write.
        *   - [ ] **Read-Through Cache:** Application reads from cache; cache fetches from DB if data is missing.
        *   - [ ] **Cache-Aside (Lazy Loading):** Application checks cache first. If miss, app fetches from DB, then puts data into cache. (Most common).
        *   - [ ] **Time-To-Live (TTL):** Data expires after a set duration. Pro: Simple. Con: Might serve stale data until expiry.
    *   - [ ] **Common Caching Problems:**
        *   - [ ] Cache Penetration (querying for non-existent keys). Solution: Cache nulls, Bloom filters.
        *   - [ ] Cache Avalanche (many caches expire simultaneously). Solution: Randomize expiry times.
        *   - [ ] Cache Breakdown/Stampede (cache miss causes many threads to hit DB). Solution: Locking/mutex on cache miss.

### Basic Distributed Systems Concepts

*   **Why It's Key:** Most modern, scalable systems are distributed. Understanding the inherent trade-offs is crucial.
*   **Example Question:** "Explain the CAP theorem and how it applies to your database choice." or "What does eventual consistency mean?"
*   **What to Learn:**
    *   - [ ] **CAP Theorem (Brewer's Theorem):** In a distributed system, you can only simultaneously guarantee two out of the three:
        *   - [ ] **C**onsistency: All nodes see the same data at the same time.
        *   - [ ] **A**vailability: Every request receives a (non-error) response, without guarantee that it contains the most recent write.
        *   - [ ] **P**artition Tolerance: The system continues to operate despite network partitions (messages being dropped or delayed between nodes).
        *   - [ ] Understand the trade-offs (CP vs. AP systems) in the face of network partitions (which are inevitable in distributed systems).
    *   - [ ] **Consistency Models:**
        *   - [ ] **Strong Consistency:** Reads always see the latest committed write (typical of single-node DBs or systems using consensus).
        *   - [ ] **Eventual Consistency:** Given enough time (with no new updates), all replicas will eventually converge to the same value. Common in highly available NoSQL DBs (e.g., Cassandra, DynamoDB).
        *   - [ ] **Causal Consistency, Read-Your-Writes Consistency (Awareness).**
    *   - [ ] **Latency vs. Throughput:** Understand the difference and how design choices impact each.
    *   - [ ] **Availability vs. Reliability:** Availability = uptime percentage. Reliability = probability of failure-free operation over time.

#### How to Learn Phase 1:

*   - [ ] Read introductory chapters of system design books/resources (System Design Primer, Grokking the System Design Interview, Designing Data-Intensive Applications).
*   - [ ] Watch introductory videos on each core concept.
*   - [ ] Practice explaining these concepts simply, using analogies.
*   - [ ] Start whiteboarding very simple systems (e.g., a counter service, a basic URL shortener focusing *only* on these concepts).

---

## Phase 2: Intermediate Concepts (Important Next Steps)

*Build upon the core concepts. These are frequently tested in more complex interview scenarios.*

### Microservices Architecture

*   **Why It's Key:** A popular architectural style for building complex applications. Understand its pros and cons compared to monoliths.
*   **Example Question:** "Discuss the challenges of migrating a monolithic application to microservices." or "How would services communicate in a microservices architecture?"
*   **What to Learn:**
    *   - [ ] **Monolith vs. Microservices:** Understand the trade-offs (Development speed, Scalability, Fault isolation, Deployment complexity, Operational overhead).
    *   - [ ] **Service Decomposition Strategies:** How to break down a system into services (e.g., by business capability, by domain-driven design).
    *   - [ ] **Inter-Service Communication:**
        *   - [ ] **Synchronous Communication:** Request/Response (e.g., REST, gRPC). Understand blocking nature and potential for cascading failures.
        *   - [ ] **Asynchronous Communication:** Event-based/Messaging (See Message Queues section). Decouples services, improves resilience.
    *   - [ ] **Service Discovery:** How services find each other dynamically.
        *   - [ ] Client-Side Discovery (Client queries registry).
        *   - [ ] Server-Side Discovery (Router/Load Balancer queries registry).
        *   - [ ] Tools (Consul, Eureka, Zookeeper, Kubernetes DNS).
    *   - [ ] **API Gateways:** Role in microservices (single entry point, routing, aggregation, cross-cutting concerns).
    *   - [ ] **Distributed Transactions (Challenges):** Maintaining consistency across multiple services.
        *   - [ ] Two-Phase Commit (2PC) - Understand the concept and its drawbacks (blocking, single point of failure).
        *   - [ ] Sagas Pattern - Using a sequence of local transactions coordinated via messaging. Compensating transactions for rollback.
    *   - [ ] **Data Management:** Each service owns its data. Challenges with joins and data consistency.

### Message Queues & Event-Driven Systems

*   **Why It's Key:** Essential for decoupling services, handling asynchronous tasks, improving resilience, and enabling scalability.
*   **Example Question:** "Design a system for processing background jobs, like sending email notifications." or "How would you use a message queue in an e-commerce order processing flow?"
*   **What to Learn:**
    *   - [ ] **Core Concepts:** Producers, Consumers, Brokers, Queues, Topics.
    *   - [ ] **Use Cases:** Asynchronous processing (email, image resizing), decoupling services, buffering load spikes, fan-out notifications.
    *   - [ ] **Publish-Subscribe (Pub/Sub) Model:** One message broadcast to multiple interested consumers via topics.
    *   - [ ] **Point-to-Point Model:** One message delivered to a single consumer from a queue.
    *   - [ ] **Message Brokers:**
        *   - [ ] **RabbitMQ:** Mature, feature-rich, supports multiple protocols (AMQP, MQTT). Good for traditional queuing tasks.
        *   - [ ] **Kafka:** High-throughput, distributed streaming platform. Built for durability, ordering (within partitions), and replayability. Often used for event sourcing, log aggregation, stream processing.
        *   - [ ] **AWS SQS/SNS, Google Pub/Sub:** Managed cloud services.
    *   - [ ] **Key Considerations:**
        *   - [ ] **Delivery Guarantees:** At-most-once, At-least-once, Exactly-once (often requires careful consumer design).
        *   - [ ] **Message Ordering:** Guarantees (e.g., Kafka guarantees order within a partition).
        *   - [ ] **Persistence:** Storing messages to survive broker restarts.
        *   - [ ] **Idempotency:** Ensuring consumers can safely process the same message multiple times (important for at-least-once delivery).
    *   - [ ] **Event Sourcing (Concept):** Storing the state of an application as a sequence of immutable events rather than the current state snapshot. Often uses message brokers.

### Load Balancing In-Depth & Fault Tolerance

*   **Why It's Key:** Designing systems that remain available and responsive despite failures or high load. Builds on Phase 1 concepts.
*   **Example Question:** "How does your system handle the failure of a web server?" or "Describe different load balancing strategies and when you'd use them."
*   **What to Learn:**
    *   - [ ] **Load Balancer Types Review:**
        *   - [ ] Hardware Load Balancers (e.g., F5).
        *   - [ ] Software Load Balancers (e.g., Nginx, HAProxy, ELB/ALB/NLB).
        *   - [ ] L4 (Network Layer - TCP/UDP) vs. L7 (Application Layer - HTTP).
    *   - [ ] **Load Balancing Algorithms Review:** Round Robin, Least Connections, Least Response Time, Weighted versions, IP Hash (for session persistence).
    *   - [ ] **Redundancy:** Having backup components (servers, databases, load balancers) to take over if a primary fails.
        *   - [ ] Active-Passive vs. Active-Active setups.
    *   - [ ] **Failover Mechanisms:** Automatically detecting failures and switching traffic to redundant components.
        *   - [ ] Health Checks: How load balancers or monitoring systems determine if a server is operational.
    *   - [ ] **Single Point of Failure (SPOF):** Identifying and mitigating components whose failure would bring down the entire system. (e.g., single DB, single LB without redundancy).
    *   - [ ] **Circuit Breaker Pattern:** Preventing repeated calls to a failing service, allowing it time to recover.
    *   - [ ] **Timeouts & Retries:** Configuring appropriate timeouts for network calls and implementing retry logic (with backoff).
    *   - [ ] **Rate Limiting & Throttling (Review):** Protecting services from being overwhelmed.

### Security Basics

*   **Why It's Key:** Security cannot be an afterthought. Interviewers expect awareness of common threats and standard security practices.
*   **Example Question:** "How would you secure user passwords?" or "Describe how OAuth works." or "What are some common web vulnerabilities?"
*   **What to Learn:**
    *   - [ ] **Authentication (AuthN):** Verifying who a user is.
        *   - [ ] Password Hashing (e.g., bcrypt, scrypt, Argon2) + Salting. *Never store plain text passwords.*
        *   - [ ] Session-based Authentication (Cookies).
        *   - [ ] Token-based Authentication (JWT - JSON Web Tokens).
        *   - [ ] OAuth 2.0 (Delegated authorization framework - "Login with Google/Facebook"). Understand roles (Resource Owner, Client, Auth Server, Resource Server) and common flows (Authorization Code).
        *   - [ ] OpenID Connect (OIDC) (Authentication layer built on OAuth 2.0).
        *   - [ ] Multi-Factor Authentication (MFA).
    *   - [ ] **Authorization (AuthZ):** Determining what an authenticated user is allowed to do.
        *   - [ ] Role-Based Access Control (RBAC).
        *   - [ ] Access Control Lists (ACLs).
        *   - [ ] Policy-based access control.
    *   - [ ] **Encryption:** Protecting data confidentiality.
        *   - [ ] **Encryption in Transit:** TLS/SSL (HTTPS). Ensures data is encrypted between client and server.
        *   - [ ] **Encryption at Rest:** Encrypting data stored in databases, file systems, backups.
    *   - [ ] **Common Web Vulnerabilities (OWASP Top 10 Awareness):**
        *   - [ ] **Injection:** (e.g., SQL Injection). Prevention: Prepared statements, parameterized queries, input validation/sanitization.
        *   - [ ] **Cross-Site Scripting (XSS):** Injecting malicious scripts into websites viewed by other users. Prevention: Output encoding, input validation, Content Security Policy (CSP).
        *   - [ ] **Cross-Site Request Forgery (CSRF):** Forcing a user's browser to make unintended requests. Prevention: Anti-CSRF tokens.
        *   - [ ] **Insecure Design / Broken Access Control:** Weaknesses in authorization logic.
        *   - [ ] **Security Misconfiguration:** Default credentials, verbose errors, unpatched software.
    *   - [ ] **Secrets Management:** Securely storing API keys, database credentials, certificates (e.g., HashiCorp Vault, AWS Secrets Manager).
    *   - [ ] **Input Validation:** Never trust user input. Validate format, length, type, range.

#### How to Learn Phase 2:

*   - [ ] Dive deeper into specific chapters of resources like "Designing Data-Intensive Applications".
*   - [ ] Read blog posts and case studies from engineering teams at tech companies (Netflix, Uber, Twitter, etc.).
*   - [ ] Practice designing medium-complexity systems:
    *   URL Shortener (adding analytics, custom URLs).
    *   Pastebin service.
    *   Basic E-commerce Site (product catalog, cart, orders).
    *   Twitter/News Feed (focus on feed generation, scaling).
    *   Chat Application (focus on real-time communication, connection management).
*   - [ ] Focus on explaining the trade-offs of different approaches within these designs.

---

## Phase 3: Advanced Concepts & Specific Areas (Deep Dive as Needed)

*These topics are often important for more senior roles, specialized roles, or specific problem types. Build familiarity, but deep mastery might not be required for all interviews unless specified.*

### Advanced Distributed Systems Concepts

*   **Why It's Relevant:** Necessary for designing highly reliable, consistent systems at massive scale, especially core infrastructure.
*   **Example Question:** "How would you ensure strong consistency across geographically distributed data centers?"
*   **What to Learn (Awareness/Conceptual Understanding):**
    *   - [ ] **Consensus Algorithms:** How distributed nodes agree on a value/state.
        *   - [ ] Paxos (Conceptually complex).
        *   - [ ] Raft (Designed for understandability - know the basics: leader election, log replication). Used in etcd, Consul.
        *   - [ ] Use cases: Distributed locks, leader election, consistent configuration management.
    *   - [ ] **Distributed Transactions (Review/Deeper Dive):**
        *   - [ ] Two-Phase Commit (2PC) - understand locking and coordinator failure issues.
        *   - [ ] Sagas Pattern - implementation details, compensation logic.
        *   - [ ] Alternatives (e.g., trying to avoid them via design if possible).
    *   - [ ] **Quorum:** Needing a majority of nodes to acknowledge an operation for consistency.
    *   - [ ] **Gossip Protocols:** How nodes efficiently share state information in a large cluster.
    *   - [ ] **CRDTs (Conflict-free Replicated Data Types):** Data structures that can be updated independently and concurrently without coordination, guaranteeing eventual consistency convergence.

### Cloud-Native Technologies & Concepts

*   **Why It's Relevant:** Most modern systems are deployed on cloud platforms (AWS, GCP, Azure). Familiarity is expected.
*   **Example Question:** "How might you leverage cloud services to build this system?" or "Discuss the pros and cons of using Kubernetes."
*   **What to Learn (Familiarity):**
    *   - [ ] **Containers:** Packaging applications and dependencies (e.g., Docker).
    *   - [ ] **Container Orchestration:** Automating deployment, scaling, and management of containerized applications.
        *   - [ ] **Kubernetes (K8s):** De facto standard. Understand core concepts (Pods, Services, Deployments, Ingress).
        *   - [ ] Managed K8s services (EKS, GKE, AKS).
    *   - [ ] **Serverless Computing:** Running code without managing servers.
        *   - [ ] **Functions as a Service (FaaS):** (e.g., AWS Lambda, Google Cloud Functions, Azure Functions). Understand event-driven nature, statelessness, cold starts, use cases (APIs, background tasks).
        *   - [ ] **Backend as a Service (BaaS):** (e.g., Firebase).
    *   - [ ] **Infrastructure as Code (IaC):** Managing infrastructure through code (e.g., Terraform, CloudFormation, Pulumi).
    *   - [ ] **Managed Cloud Services:** Understanding common offerings (Managed Databases like RDS/Cloud SQL, Load Balancers, Message Queues, Object Storage like S3/GCS, CDNs like CloudFront/Cloudflare). Leveraging them vs. building yourself.

### Big Data & Stream Processing

*   **Why It's Relevant:** Required for roles dealing with very large datasets or real-time data analysis.
*   **Example Question:** "How would you design a system to process clickstream data for real-time analytics?"
*   **What to Learn (Awareness unless role-specific):**
    *   - [ ] **Batch Processing:** Processing large volumes of data in batches.
        *   - [ ] **Hadoop Ecosystem:** HDFS (Storage), MapReduce (Processing framework).
        *   - [ ] **Apache Spark:** Faster, more general-purpose batch (and stream) processing engine. Understand RDDs/DataFrames, lazy evaluation.
    *   - [ ] **Stream Processing:** Processing data continuously as it arrives.
        *   - [ ] **Apache Kafka (Review):** Often the ingestion layer.
        *   - [ ] **Stream Processing Frameworks:** Kafka Streams, Apache Flink, Apache Spark Streaming.
        *   - [ ] Concepts: Windowing (Tumbling, Sliding), Watermarks, State Management.
    *   - [ ] **Data Warehousing:** Central repository for structured, historical data for analytics (e.g., Redshift, BigQuery, Snowflake).
    *   - [ ] **Data Lakes:** Repository for raw data in various formats (often built on object storage like S3/GCS).
    *   - [ ] **Lambda Architecture / Kappa Architecture (Concepts):** Patterns for combining batch and stream processing.

### Performance Optimization at Scale

*   **Why It's Relevant:** Fine-tuning systems to handle extreme loads or meet strict latency requirements.
*   **Example Question:** "Your system latency is high under peak load. How would you investigate and optimize?"
*   **What to Learn (Builds on earlier concepts):**
    *   - [ ] **Monitoring & Profiling:** Identifying bottlenecks (CPU, Memory, I/O, Network). Tools (Prometheus, Grafana, Datadog, application profilers).
    *   - [ ] **Advanced Caching Strategies:** Multi-level caching (CPU L1/L2, local memory, distributed cache, CDN), cache pre-warming.
    *   - [ ] **Database Optimization:** Advanced indexing (e.g., covering indexes), query optimization (analyzing query plans), connection pooling tuning.
    *   - [ ] **Network Optimization:** Reducing chattiness, using efficient protocols (HTTP/2, gRPC), proximity-based routing (CDNs, regional deployments).
    *   - [ ] **Concurrency Patterns:** Efficiently handling multiple requests simultaneously (async I/O, thread pools).
    *   - [ ] **Data Compression:** Reducing data size for storage and network transfer.

#### How to Learn Phase 3:

*   - [ ] Focus on areas relevant to roles you are targeting.
*   - [ ] Read more advanced chapters/sections of system design resources.
*   - [ ] Explore specific technology documentation (Kubernetes, Kafka, Spark).
*   - [ ] Read papers or deep-dive blog posts on specific algorithms or system architectures (e.g., Google File System, DynamoDB paper, Raft paper).
*   - [ ] Practice designing highly complex, large-scale systems (e.g., Google Search-like typeahead, Netflix-like streaming service, Distributed Rate Limiter).

---

**General Study Tips:**

*   - [ ] **Consistency is key:** Study regularly, even if it's just for short periods.
*   - [ ] **Practice whiteboarding:** Draw diagrams, explain trade-offs aloud. This mimics the interview experience.
*   - [ ] **Use a framework:** Employ a consistent approach for tackling design problems (e.g., clarify requirements, estimate scale, design high-level components, detail specific components, discuss trade-offs & bottlenecks).
*   - [ ] **Read real-world case studies:** Understand how large tech companies solve problems.
*   - [ ] **Don't just memorize:** Focus on understanding the *why* behind design choices and the *trade-offs* involved.
*   - [ ] **Mock Interviews:** Practice with peers or mentors.

Good luck with your learning!