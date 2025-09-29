# Technology Overview

## Database

### NoSQL - Document Database

#### MongoDB
**Pros:**
- Schema flexibility and document-based structure
- Horizontal scaling with built-in sharding
- Rich query language with aggregation framework
- Good performance for read-heavy workloads
- Native JSON/BSON support

**Cons:**
- Memory usage can be high
- No ACID transactions across multiple documents (before v4.0)
- Eventual consistency in replica sets
- Less mature ecosystem compared to SQL databases

**Limitations:**
- Max document size: 16MB
- Max index key length: 1024 bytes
- Max collection name length: 120 characters
- Max database name length: 64 characters
- Max depth for nested documents: 100 levels

### SQL - Relational Database

#### PostgreSQL
**Pros:**
- ACID compliance and strong consistency
- Advanced SQL features and extensibility
- JSON/JSONB support for semi-structured data
- Excellent performance for complex queries
- Rich ecosystem and mature tooling

**Cons:**
- Vertical scaling limitations
- More complex horizontal scaling setup
- Higher resource usage for simple operations
- Steeper learning curve for advanced features

**Limitations:**
- Max database size: unlimited (practically limited by storage)
- Max table size: 32TB
- Max row size: 1.6TB
- Max field size: 1GB
- Max connections: typically 100-400 (configurable)
- Max query execution time: configurable (default unlimited)

#### MySQL
**Pros:**
- Fast and reliable for web applications
- Large community and ecosystem
- Good replication and clustering options
- Lower resource usage compared to PostgreSQL
- Easy to learn and deploy

**Cons:**
- Limited JSON support compared to PostgreSQL
- Less advanced SQL features
- InnoDB storage engine limitations
- Weaker consistency guarantees in some configurations

**Limitations:**
- Max database size: unlimited (MyISAM: 256TB per table)
- Max row size: 65,535 bytes
- Max key length: 3072 bytes (InnoDB)
- Max connections: 151 (default, configurable up to 100,000)
- Max tables per database: unlimited (filesystem dependent)

### NoSQL - In-Memory Cache/Data Store

#### Redis
**Pros:**
- Extremely fast in-memory operations
- Rich data structures (strings, hashes, lists, sets, sorted sets)
- Built-in pub/sub messaging
- Lua scripting support
- Simple setup and operation

**Cons:**
- Memory-bound storage
- Limited query capabilities
- Single-threaded for commands
- Data persistence options have trade-offs

**Limitations:**
- Max key size: 512MB
- Max value size: 512MB
- Max memory usage: depends on available RAM
- Max connections: 10,000 (default, configurable)
- Max databases: 16 (default, configurable up to thousands)

### NoSQL - Wide-Column Store

#### Cassandra
**Pros:**
- Linear scalability across multiple nodes
- No single point of failure
- Excellent write performance
- Tunable consistency levels
- Good for time-series data

**Cons:**
- Limited query flexibility (no JOINs)
- Eventual consistency by default
- Complex data modeling requirements
- High operational complexity

**Limitations:**
- Max column value size: 2GB
- Max row size: 2GB
- Max partition size: 100MB (recommended)
- Max number of columns: 2 billion
- Max clustering columns: 65,535

### NoSQL - Key-Value Store (Managed)

#### DynamoDB
**Pros:**
- Fully managed AWS service
- Predictable performance at any scale
- Built-in security and backup
- Serverless scaling
- Global tables for multi-region

**Cons:**
- Vendor lock-in to AWS
- Limited query patterns
- Can be expensive at scale
- No complex queries or aggregations

**Limitations:**
- Max item size: 400KB
- Max partition key length: 2048 bytes
- Max sort key length: 1024 bytes
- Max secondary indexes per table: 20
- Max query result size: 1MB
- Max batch write size: 25 items or 16MB