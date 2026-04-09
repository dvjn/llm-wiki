# Wiki Examples

Real-world examples of wiki pages from different domains.

## Example 1: Concept Page (Rate Limiting)

```markdown
# Rate Limiting

> A technique to control the rate of incoming requests to prevent resource exhaustion and abuse.

## Core Definition

Rate limiting restricts the number of requests a client can make within a specified time window. It protects services from being overwhelmed by too many requests, whether intentional (DDoS) or unintentional (misconfigured clients).

## Key Properties

- **Algorithm**: Token bucket, sliding window, or fixed window counter
- **Scope**: Per-client, per-endpoint, or global
- **Response**: HTTP 429 (Too Many Requests) when exceeded
- **Headers**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- **Granularity**: Time window size (seconds, minutes, hours)

## In MyProject

Implemented as a middleware layer using the token bucket algorithm. Rate limits are configured per endpoint in `config/rate_limits.yaml`. Burst allowance is set to 10% above steady-state limit.

## Related Concepts

- [[concept-throttling]] — similar but focuses on slowing rather than rejecting
- [[concept-circuit-breaker]] — protects downstream services from cascading failures
- [[concept-backpressure]] — reactive approach to overload handling

## Sources

- [[source-article-rate-limiting-patterns]] §2 Algorithms
- [[source-spec-rest-api]] §4 Error Handling
```

## Example 2: Entity Page (User Session)

```markdown
# User Session

> Represents an authenticated user's active interaction with the system.

## Purpose

Sessions track user state across multiple requests, maintaining authentication status, preferences, and temporary data without requiring re-authentication on each interaction.

## Structure

```json
{
  "session_id": "uuid",
  "user_id": "integer",
  "created_at": "timestamp",
  "expires_at": "timestamp",
  "data": {
    "preferences": {},
    "cart_items": []
  }
}
```

## Lifecycle

1. **Creation**: Generated on successful authentication
2. **Access**: Validated on each request, extended if close to expiry
3. **Update**: Modified when user changes preferences or cart
4. **Termination**: Expired automatically or manually on logout

## Code References

- `src/auth/session.rs` — Session type and validation logic
- `src/middleware/session.rs` — Request session injection

## Related Entities

- [[entity-user]] — the user who owns this session
- [[entity-authentication-token]] — JWT used to create session
```

## Example 3: Decision Page (ADR-style)

```markdown
# Decision: Use PostgreSQL with Connection Pooling

**Status**: Accepted
**Date**: 2026-04-01
**Context**: Need database for user data with high read/write throughput

## Decision

Use PostgreSQL with PgBouncer connection pooling instead of a connection-per-request model.

## Rationale

| Criterion | Connection Pool | Direct Connections |
|-----------|-----------------|-------------------|
| Connection overhead | Low (reuse) | High (new each request) |
| Memory usage | Constant | Linear with requests |
| Latency | Lower (warm connections) | Higher (handshake each time) |
| Complexity | Moderate (pool management) | Simple |

Connection pooling reduces connection overhead from ~35ms per connection to ~1ms for pooled reuse. With expected 500 concurrent requests, direct connections would exceed PostgreSQL's default 100 connection limit.

## Consequences

- **Positive**: Handles 5x more concurrent requests with same PostgreSQL config
- **Positive**: Lower latency for database operations
- **Negative**: Pool sizing requires tuning based on actual load patterns
- **Risk**: Pool exhaustion if transaction hold times increase unexpectedly

## Related

- [[concept-connection-pooling]] — concept page
- [[comparison-postgres-vs-mysql]] — database choice comparison
```

## Example 4: Comparison Page

```markdown
# PostgreSQL vs MySQL

> Two popular open-source relational databases with different strengths

## Dimensions

| Dimension | PostgreSQL | MySQL | Project Position |
|-----------|------------|-------|------------------|
| JSON support | Native JSONB, indexed | JSON type, limited indexing | PostgreSQL for JSON queries |
| Extensibility | Extensions, custom types | Plugins, limited | PostgreSQL for flexibility |
| Replication | Multiple modes built-in | Requires setup | Both viable |
| MVCC | Full implementation | Limited | PostgreSQL for consistency |
| Community | Strong enterprise focus | Strong web focus | MySQL for simpler web apps |

## Analysis

PostgreSQL offers richer features (JSONB, window functions, custom types) and stricter consistency. MySQL is simpler to configure and more common in web hosting.

For projects requiring complex queries, JSON operations, or strict consistency guarantees, PostgreSQL is superior. For simple CRUD web apps with predictable schemas, MySQL is often sufficient.

## When to use each

- **PostgreSQL**: Complex queries, JSON-heavy data, analytical workloads, strict consistency
- **MySQL**: Simple web apps, existing MySQL expertise, hosting constraints

## Sources

- [[source-article-postgres-features]] — PostgreSQL capabilities
- [[source-comparison-db-choice]] — database selection guide
```

## Example 5: Source Summary Page

```markdown
# Source: Raft Consensus Algorithm (Ongaro 2014)

**Type**: paper
**URL**: https://raft.github.io/raft.pdf
**Date Added**: 2026-04-09
**Key Sections**: §3 Leader Election, §4 Log Replication, §5 Safety

## Summary

Ongaro and Ousterhout present Raft, a consensus algorithm designed for understandability. It achieves the same safety guarantees as Paxos but with a clearer mental model: leader-based log replication with heartbeats for leader election.

## Key Extracts

> "Raft decomposes consensus into three relatively independent subproblems: leader election, log replication, and safety."

> "The leader accepts log entries from clients, replicates them to other servers, and tells servers when to apply entries to their state machines."

> "Raft uses a randomized election timeout to reduce likelihood of split votes."

## Related Concepts

- [[concept-consensus]] — distributed agreement
- [[concept-leader-election]] — selecting the coordinator node
- [[concept-log-replication]] — maintaining consistency across nodes
- [[concept-split-brain]] — failure mode to avoid

## Impact on Project

Raft is the foundation for our distributed state machine. We use Raft's leader election with 150-300ms randomized timeouts. Log replication handles configuration changes and user state transitions.
```

## Example 6: Index Page

```markdown
# MyProject Wiki — Content Catalog

> Auto-updated on every ingest, query filing, and lint pass.

## Concepts

- [[concept-rate-limiting]] — Request throttling technique
- [[concept-connection-pooling]] — Database connection management
- [[concept-consensus]] — Distributed agreement protocols
- [[concept-leader-election]] — Coordinator selection in distributed systems
- [[concept-authentication]] — Identity verification

## Entities

- [[entity-user-session]] — Active user interaction state
- [[entity-api-key]] — Authentication credential for API access

## Decisions

- [[decision-use-postgres-pooling]] — Why we chose connection pooling
- [[decision-rate-limit-algorithm]] — Token bucket vs sliding window

## Comparisons

- [[comparison-postgres-vs-mysql]] — Database selection trade-offs
- [[comparison-jwt-vs-session]] — Authentication approach comparison

## Sources

- [[source-paper-raft-consensus]] — Ongaro 2014
- [[source-spec-rest-api]] — REST design guidelines

## Statistics

| Metric | Count |
|--------|-------|
| Raw sources | 7 |
| Wiki pages | 29 |
| Concepts | 5 |
| Entities | 2 |
```

## Example 7: Log Entry

```markdown
## [2026-04-09] ingest | Raft Consensus Paper

Added `raw/papers/paper-raft-consensus.md`. Created source summary. Created 5 concept pages (consensus, leader-election, log-replication, split-brain, safety). Created 2 entity pages (leader-node, follower-node). Updated index.

---

## [2026-04-09] lint | Wiki lint pass

Fixed 4 broken links. Created 3 stub concept pages for missing concepts. Clarified distinction between rate limiting and throttling. Updated index (29 pages, 18 concepts).
```