# Wiki Examples

Real-world examples of wiki pages from different domains.

## Example 1: Concept Page (DID - Decentralized Identity)

```markdown
# DID (Decentralized Identifier)

> A globally unique persistent identifier that does not require a centralized registration authority.

## Core Definition

A DID is a URI composed of three parts: the `did:` scheme, a method identifier, and a method-specific string. Example: `did:fed:abc123`. DIDs are designed so the controller can prove control without permission from any central authority.

## Key Properties

- **Scheme**: Always starts with `did:` per RFC 3986
- **Method**: Identifies the DID method (e.g., `fed`, `key`, `web`)
- **Persistence**: No renewal required; persistent by design
- **Resolution**: Resolves to a DID Document through the method's Resolve operation
- **Cryptographic verifiability**: Control proven without third-party permission

## DID Syntax

```
did = "did:" method ":" method-specific-id
method = 1*method-char
method-char = ALPHA / DIGIT
```

## In didfed

didfed implements the `did:fed` method. DIDs have format `did:fed:<id>` where `<id>` is a 32-character base32 identifier. Validation is in `crates/didfed-core/src/did.rs`.

## Related Concepts

- [[did-document]] — the document resolved from a DID
- [[did-method]] — implementation defining CRUD operations
- [[verification-method]] — cryptographic keys for proving control

## Sources

- [[spec-did-core]] §3 Identifier
```

## Example 2: Entity Page (Validator)

```markdown
# Validator

> Participant in the FBA consensus layer, proposes and commits DID operations.

## Purpose

Validators form the distributed trust layer of didfed. They vote on DID operations using Federated Byzantine Agreement, ensuring agreement without requiring all validators to trust each other directly.

## Structure

```rust
pub struct Validator {
    pub id: NodeId,           // 32-byte Ed25519 public key
    pub quorum_slice: QuorumSlice,  // Trust set for FBA
    pub endpoint: Url,        // HTTP endpoint for consensus messages
}
```

## Lifecycle

1. **Creation**: Validator joins via governance vote
2. **Operation**: Proposes DID ops, participates in FBA voting
3. **Rotation**: Quorum slice can be updated by validator
4. **Removal**: Tombstoned via consensus after governance decision

## Code References

- `crates/didfed-core/src/validator.rs` — Validator type
- `crates/didfed-server/src/consensus/` — FBA implementation

## Related Entities

- [[quorum-slice]] — validator's chosen trust set
- [[federated-voting]] — vote procedure validators follow
```

## Example 3: Decision Page (ADR-style)

```markdown
# Decision: Use FBA instead of BFT consensus

**Status**: Accepted
**Date**: 2026-04-01
**Context**: Need consensus for DID operations across untrusted validators

## Decision

Use Federated Byzantine Agreement (FBA) instead of traditional BFT (like PBFT or HotStuff).

## Rationale

| Criterion | FBA | BFT (PBFT) |
|-----------|-----|------------|
| Open membership | Yes | No |
| Validator autonomy | High | Low |
| Quorum flexibility | Per-node slices | Fixed 2f+1 |
| Throughput | Moderate | High |

FBA matches our goal of allowing organizations to join the federation without requiring global trust. Each validator chooses its own quorum slice.

## Consequences

- **Positive**: Open federation, no central authority for validator set
- **Positive**: Organizations can maintain their own trust policies
- **Negative**: Lower throughput than optimized BFT
- **Risk**: Quorum intersection requires careful configuration

## Related

- [[federated-byzantine-agreement]] — concept page
- [[stellar-consensus-protocol]] — specific FBA construction used
```

## Example 4: Comparison Page

```markdown
# did:fed vs did:web

> Distributed FBA consensus vs DNS/TLS web infrastructure trust

## Dimensions

| Dimension | did:fed | did:web | didfed position |
|-----------|---------|---------|-----------------|
| Trust model | FBA consensus | DNS + TLS | FBA for distributed trust |
| Update latency | ~seconds | ~minutes (DNS TTL) | Faster updates |
| Infrastructure | Federation of validators | Standard web server | Requires validator network |
| Key rotation | On-chain via consensus | Off-chain, manual | Automated rotation |
| Censorship resistance | High (distributed) | Low (single server) | Resilient to single points |

## Analysis

did:web is simpler — just host a DID Document at a well-known URL. But it inherits all the trust assumptions of the web: DNS registrars, certificate authorities, web hosting providers.

did:fed trades simplicity for resilience. No single entity can revoke or censor a DID. Updates require consensus among validators.

## When to use each

- **did:web**: Personal sites, small projects, when simplicity matters
- **did:fed**: Organizations, consortiums, when distributed trust matters

## Sources

- [[spec-did-web]] — did:web method specification
- [[did-fed-vs-did-key]] — comparison with simpler method
```

## Example 5: Source Summary Page

```markdown
# Source: Stellar Consensus Protocol (Mazières 2016)

**Type**: paper
**URL**: https://www.stellar.org/papers/stellar-consensus-protocol.pdf
**Date Added**: 2026-04-09
**Key Sections**: §3 FBA, §4 SCP construction, §5 Safety proof

## Summary

Mazières introduces Federated Byzantine Agreement (FBA), a consensus model where nodes choose their own trust sets (quorum slices) rather than relying on a global validator set. The Stellar Consensus Protocol (SCP) is a specific construction of FBA achieving safety and liveness without requiring all nodes to agree on membership.

## Key Extracts

> "FBA allows each node to choose its own quorum slices. Quorums are formed from the union of slices."

> "SCP guarantees safety if quorum intersection holds, and liveness if correct nodes form a quorum."

> "Open membership: nodes can join without central approval."

## Related Concepts

- [[federated-byzantine-agreement]] — the FBA model
- [[stellar-consensus-protocol]] — SCP specifically
- [[quorum-slice]] — trust sets in FBA
- [[federated-voting]] — vote/accept/confirm procedure

## Impact on didfed

FBA is the foundation of didfed's trust model. We use SCP-style federated voting for DID operation consensus. The quorum slice design allows organizations to maintain their own trust policies while participating in the federation.
```

## Example 6: Index Page

```markdown
# MyProject Wiki — Content Catalog

> Auto-updated on every ingest, query filing, and lint pass.

## Concepts

- [[DID]] — Decentralized Identifier
- [[DID-Document]] — Document resolved from a DID
- [[FBA]] — Federated Byzantine Agreement
- [[SCP]] — Stellar Consensus Protocol
- [[CID]] — Content-addressed identifier

## Entities

- [[validator]] — Consensus participant
- [[did-document-entity]] — DID Document in code

## Decisions

- [[use-fba-not-bft]] — Why we chose FBA consensus

## Comparisons

- [[did-fed-vs-did-web]] — vs did:web method
- [[did-fed-vs-did-key]] — vs did:key method

## Sources

- [[spec-did-core]] — W3C DID Core
- [[stellar-scp]] — Mazières 2016 paper

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
## [2026-04-09] ingest | W3C DID Core

Added `raw/specs/spec-did-core.md`. Created source summary. Created 5 concept pages (did, did-document, verification-method, did-method, did-url). Created 2 entity pages (validator, verification-method-entity). Updated index.

---

## [2026-04-09] lint | Wiki lint pass

Fixed 4 broken links. Created 3 stub concept pages for missing concepts. Clarified FBA open/closed membership distinction. Updated index (29 pages, 18 concepts).
```
