# System Design Interview Topics

## Core Concepts

### Scalability
- **Horizontal scaling**: Adding more servers (scale out). Better for handling large loads, more fault tolerant
- **Vertical scaling**: Adding more power to existing servers (scale up). Simpler but has limits and single point of failure

### Reliability and Fault Tolerance
- **Reliability**: System continues to work correctly even when failures occur
- **Fault tolerance**: System's ability to continue operating despite component failures
- Achieved through redundancy, replication, and failover mechanisms

#### Hardware Failures
**Examples**: Server crashes, disk failures, network card failures, power outages
**Solutions**:
- Redundant hardware (RAID for disks, multiple power supplies)
- Load balancers with health checks
- Auto-failover to backup servers
- Geographic distribution across data centers

#### Software Failures
**Examples**: Application bugs, memory leaks, deadlocks, OS crashes
**Solutions**:
- Circuit breaker pattern to isolate failing services
- Graceful degradation (reduce functionality instead of complete failure)
- Restart policies and health checks
- Blue-green deployments for safer releases

#### Network Failures
**Examples**: Network partitions, DNS failures, packet loss, high latency
**Solutions**:
- Multiple network paths and providers
- Retry mechanisms with exponential backoff
- Timeout configurations
- Service mesh for network reliability

#### Human Errors
**Examples**: Misconfigurations, accidental deletions, deployment mistakes
**Solutions**:
- Infrastructure as code (version controlled configs)
- Automated testing and CI/CD pipelines
- Rollback capabilities
- Least privilege access controls

#### Dependency Failures
**Examples**: Third-party API downtime, database unavailability, external service failures
**Solutions**:
- Bulkhead pattern (isolate dependencies)
- Fallback mechanisms and cached responses
- Multiple vendor strategies
- Service level agreements (SLAs) with dependencies

#### Scale-Related Failures
**Examples**: Traffic spikes, resource exhaustion, cascading failures
**Solutions**:
- Auto-scaling policies
- Rate limiting and throttling
- Queue systems to handle bursts
- Performance testing and capacity planning

### CAP Theorem
- **Consistency**: All nodes see the same data simultaneously
- **Availability**: System remains operational 100% of the time
- **Partition tolerance**: System continues despite network failures
- You can only guarantee 2 out of 3 properties

#### Consistency + Availability (CA) - No Partition Tolerance
**Strategy**: Traditional RDBMS in single data center
- **Examples**: PostgreSQL, MySQL (single master)
- **Trade-off**: System fails during network partitions
- **Use case**: When network reliability is guaranteed (single data center)

#### Consistency + Partition Tolerance (CP) - Sacrifice Availability
**Strategy**: Strong consistency, may become unavailable during partitions
- **Examples**: MongoDB, HBase, Redis Cluster
- **Techniques**:
  - Master-slave with synchronous replication
  - Consensus algorithms (Raft, Paxos)
  - Quorum reads/writes (majority must respond)
- **Trade-off**: System may be unavailable but data is always consistent
- **Use case**: Financial systems, banking where data accuracy is critical

#### Availability + Partition Tolerance (AP) - Sacrifice Consistency
**Strategy**: Always available, eventual consistency
- **Examples**: Cassandra, DynamoDB, CouchDB
- **Techniques**:
  - Asynchronous replication
  - Vector clocks for conflict resolution
  - Last-write-wins or application-level conflict resolution
  - Gossip protocols for state synchronization
- **Trade-off**: Temporary inconsistency but system stays available
- **Use case**: Social media, content delivery, shopping carts where availability matters more

### Latency vs Throughput
- **Latency**: Time to process a single request (response time)
- **Throughput**: Number of requests processed per unit time
- Often inversely related - optimizing one may hurt the other

#### Strategies to Improve Latency
- **CDN and edge caching**: Serve content from geographically closer locations
- **Database optimization**: Proper indexing, query optimization, connection pooling
- **Asynchronous processing**: Handle non-critical operations in background
- **Reduce payload size**: Data compression, minimize response data
- **In-memory caching**: Redis, Memcached for frequently accessed data

#### Strategies to Improve Throughput
- **Horizontal scaling**: Add more servers to handle parallel requests
- **Load balancing**: Distribute requests across multiple servers
- **Database read replicas**: Scale read operations across multiple databases
- **Message queues**: Batch processing for high-volume operations
- **Connection multiplexing**: Reuse connections to reduce overhead
- **Compression**: Reduce data transfer time and bandwidth usage

### Load Balancing Strategies
- **Round Robin**: Requests distributed sequentially
- **Least Connections**: Route to server with fewest active connections
- **IP Hash**: Route based on client IP hash
- **Weighted**: Assign different weights to servers based on capacity

### Database Sharding and Partitioning
- **Sharding**: Horizontal partitioning across multiple databases
- **Vertical partitioning**: Split tables by columns
- **Horizontal partitioning**: Split tables by rows
- **Directory-based sharding**: Lookup service to determine shard location

### Microservices vs Monolith
- **Monolith**: Single deployable unit, simpler to develop/test/deploy initially
- **Microservices**: Small, independent services. Better scalability, technology diversity, but increased complexity

## System Components

### Load Balancers
- **Layer 4 (Transport)**: Routes based on IP and port (faster)
- **Layer 7 (Application)**: Routes based on content (HTTP headers, URLs)
- Can be hardware or software-based (nginx, HAProxy)

### Application Servers
- Execute business logic, process requests
- Examples: Tomcat, Node.js, Gunicorn
- Stateless design preferred for scalability

### Databases
- **SQL (RDBMS)**: ACID properties, complex queries, structured data (MySQL, PostgreSQL)
- **NoSQL**: Better scalability, flexible schema. Types: Document (MongoDB), Key-value (Redis), Column (Cassandra), Graph (Neo4j)

### Message Queues
- Asynchronous communication between services
- **Point-to-point**: One producer, one consumer
- **Publish-subscribe**: One producer, multiple consumers
- Provides decoupling and reliability

### CDN (Content Delivery Network)
- Geographically distributed servers to serve content closer to users
- Reduces latency and bandwidth usage
- Good for static content (images, videos, CSS, JS)

### Reverse Proxy
- Sits in front of web servers, forwards client requests
- Benefits: Load balancing, SSL termination, caching, security
- Examples: nginx, Apache HTTP Server

### API Gateway
- Single entry point for all client requests
- Handles authentication, rate limiting, request routing, response transformation
- Examples: AWS API Gateway, Kong, Zuul

## Database Design

### ACID Properties
- **Atomicity**: Transaction is all-or-nothing
- **Consistency**: Database remains in valid state
- **Isolation**: Concurrent transactions don't interfere
- **Durability**: Committed changes persist

### Database Indexing
- Data structure that improves query performance
- **B-tree**: Good for range queries
- **Hash**: Good for equality lookups
- Trade-off: Faster reads, slower writes, more storage

### Replication Strategies
- **Master-Slave**: One write node, multiple read replicas
- **Master-Master**: Multiple write nodes (conflict resolution needed)
- **Synchronous**: Wait for replica confirmation (slower but consistent)
- **Asynchronous**: Don't wait (faster but potentially inconsistent)

### Consistency Models
- **Strong consistency**: All reads return most recent write
- **Eventual consistency**: System will become consistent over time
- **Weak consistency**: No guarantees about when consistency occurs

### Database Federation
- Split databases by function (users, products, orders)
- Reduces read/write traffic to each database
- More complex joins across databases

### Denormalization
- Add redundant data to reduce joins
- Trade-off: Faster reads, more storage, data consistency challenges

## Caching

### Cache Patterns
- **Cache-aside**: Application manages cache (read-through, write-around)
- **Write-through**: Write to cache and database simultaneously
- **Write-behind**: Write to cache immediately, database later
- **Refresh-ahead**: Proactively refresh cache before expiration

### Cache Levels
- **Browser cache**: Client-side caching
- **CDN**: Geographic distribution
- **Reverse proxy**: Server-side caching
- **Application cache**: In-memory (Redis, Memcached)
- **Database cache**: Query result caching

### Cache Eviction Policies
- **LRU (Least Recently Used)**: Remove least recently accessed
- **LFU (Least Frequently Used)**: Remove least frequently accessed
- **FIFO (First In, First Out)**: Remove oldest entries
- **TTL (Time To Live)**: Remove after expiration

### Distributed Caching
- **Consistent hashing**: Minimize redistribution when nodes change
- **Cache coherence**: Keep multiple cache copies synchronized
- **Hot spots**: Some keys accessed much more frequently

## Communication Patterns

### Synchronous vs Asynchronous
- **Synchronous**: Client waits for response (blocking)
- **Asynchronous**: Client doesn't wait, gets response later (non-blocking)
- Async better for long-running operations

### REST vs GraphQL vs gRPC
- **REST**: Stateless, HTTP methods, multiple round trips
- **GraphQL**: Single endpoint, client specifies data needed
- **gRPC**: Binary protocol, faster, strongly typed, good for microservices

### Message Queues
- **Kafka**: High throughput, distributed, persistent
- **RabbitMQ**: Feature-rich, complex routing, AMQP
- **SQS**: AWS managed, simple, reliable

### Pub/Sub Patterns
- Publishers send messages to topics
- Subscribers receive messages from topics they're interested in
- Loose coupling between producers and consumers

### Event-driven Architecture
- Components communicate through events
- Promotes loose coupling and scalability
- Event sourcing: Store events, not current state

## Common System Designs
- Chat system (WhatsApp, Slack)
- Social media feed (Twitter, Instagram)
- Video streaming (YouTube, Netflix)
- URL shortener (bit.ly, TinyURL)
- Search engine (Google)
- Ride-sharing system (Uber, Lyft)
- E-commerce platform (Amazon)
- Payment system
- Notification system

## Performance & Monitoring

### Metrics and Logging
- **Application metrics**: Response time, throughput, error rate
- **Infrastructure metrics**: CPU, memory, disk, network
- **Business metrics**: User engagement, conversion rates
- **Structured logging**: JSON format for easier parsing

### Circuit Breaker Pattern
- Prevent cascading failures
- States: Closed (normal), Open (failing), Half-open (testing)
- Fail fast when downstream service is down

### Rate Limiting
- **Token bucket**: Tokens added at fixed rate, consumed per request
- **Leaky bucket**: Requests processed at fixed rate
- **Fixed window**: Fixed number of requests per time window
- **Sliding window**: More precise than fixed window

### Auto-scaling
- **Horizontal**: Add/remove instances based on demand
- **Vertical**: Increase/decrease instance size
- **Predictive**: Scale based on predicted load
- **Reactive**: Scale based on current metrics

## Security

### Authentication vs Authorization
- **Authentication**: Verify user identity (who you are)
- **Authorization**: Verify user permissions (what you can do)
- Multi-factor authentication adds security layers

### OAuth, JWT
- **OAuth 2.0**: Authorization framework for third-party access
- **JWT (JSON Web Token)**: Self-contained tokens with user info
- Stateless authentication, good for microservices

### HTTPS/TLS
- Encrypts data in transit
- Certificate-based trust model
- TLS handshake establishes secure connection

### DDoS Protection
- **Rate limiting**: Limit requests per IP
- **Load balancing**: Distribute traffic
- **CDN**: Absorb traffic geographically
- **Blackholing**: Drop malicious traffic