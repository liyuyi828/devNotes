# Service Integration

## Key Questions Before Integration

### Technical Requirements
- **What data format does the service use?** (JSON, XML, Protocol Buffers, etc.)
- **What communication protocol is supported?** (REST API, GraphQL, gRPC, WebSockets, message queues)
- **What authentication method is required?** (API keys, OAuth 2.0, JWT, Basic Auth, mTLS)
- **What are the API rate limits?** (requests per second/minute/hour, burst limits)
- **What is the API versioning strategy?** (URL versioning, header versioning, backward compatibility)
- **Are there SDK/libraries available** for your programming language?

### Data & Schema
- **What data do you need to send/receive?**
- **How does their data schema map to your internal data model?**
- **Are there required vs optional fields?**
- **What are the data validation rules?**
- **How do you handle schema evolution** and breaking changes?
- **What is the maximum payload size?**

### Reliability & Performance
- **What are the SLA/uptime guarantees?**
- **What is the expected response time?**
- **How do you handle service downtime?** (fallback mechanisms, circuit breakers)
- **What retry strategies should be implemented?**
- **Is there a test/sandbox environment** for development?
- **What monitoring and alerting is available?**

### Error Handling
- **What error codes and messages are returned?**
- **How are transient vs permanent errors differentiated?**
- **What is the retry policy for failed requests?**
- **How do you handle partial failures?**
- **Is there dead letter queue support** for failed messages?

### Security & Compliance
- **What security standards does the service follow?**
- **How is sensitive data encrypted** in transit and at rest?
- **What compliance requirements** must be met (GDPR, HIPAA, SOC2)?
- **How are API keys/credentials managed** and rotated?
- **What IP whitelisting** or network restrictions exist?
- **Are there audit logging capabilities?**

### Business & Operational
- **What are the costs** (per request, monthly subscription, usage tiers)?
- **What support options are available?** (documentation, forums, direct support)
- **What is the vendor's stability** and long-term viability?
- **Are there any vendor lock-in concerns?**
- **What is the migration strategy** if you need to switch providers?
- **What are the terms of service** and data ownership policies?

### Integration Patterns
- **Should this be synchronous or asynchronous?**
- **Do you need real-time updates** or is eventual consistency acceptable?
- **Should you use webhooks** for event notifications?
- **Do you need request/response or fire-and-forget?**
- **How will you handle duplicate messages** or idempotency?
- **What caching strategy** should be implemented?

### Testing & Deployment
- **How will you test the integration** (unit tests, integration tests, contract tests)?
- **What is the deployment strategy** (blue-green, canary, feature flags)?
- **How will you monitor the integration** in production?
- **What rollback procedures** are in place?
- **How do you handle configuration changes** without downtime?

### Documentation & Maintenance
- **Is the API documentation comprehensive** and up-to-date?
- **How are breaking changes communicated?**
- **What is the deprecation policy** for old API versions?
- **How often does the service release updates?**
- **Are there community resources** (Stack Overflow, GitHub issues)?