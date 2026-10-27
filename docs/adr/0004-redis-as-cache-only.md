# ADR 0004: Redis as a Cache, Never the Source of Truth

Status: Redis **Accepted** 2026-10-28 (owner, D-09). Usage rules below **Proposed**.
Date: 2026-10-28
Decision owner: Nova Logistics engineering owner
Related: [architecture](../03-architecture.md#runtime-components), [environment workflow](../09-environment-workflow.md)

## Context

The owner asked for Redis cache support. SWP already uses Redis for planning caches and OTP codes. nova-sg-all deliberately keeps jobs and outbox in MySQL because Redis Pub/Sub loses messages when a subscriber is away. Grids with UDA columns, settings resolution and permission checks are read on almost every request.

## Decision

Redis 7.4 is a cache and a short-lived coordination store only.

| Use | Pattern | Rule |
| --- | --- | --- |
| Settings resolution, UDA definitions, role permissions, document-type catalogue | Cache-aside | Key `t:{tenant}:{module}:{name}:{version}`; TTL ≤ 10 min; invalidated by version bump on write |
| Dashboard counts (compliance %, expiring in 7/30/60 days) | Cache-aside with short TTL | TTL ≤ 60 s; always recomputable from MySQL |
| Import and ZIP job progress for the UI | Write-through counter | The job row in MySQL is authoritative; Redis only speeds polling |
| Rate limits (sign-in, OTP requests, uploads per tenant) | Fixed-window counters | Fail open for reads, fail closed for OTP sends |
| OTP codes | Short TTL value, hashed | TTL 5 min; also limited by attempts |

Never in Redis only: business records, job queue, outbox, idempotency keys, sessions' authority, audit. The system must stay correct (only slower) when Redis is empty or down. Every key starts with the tenant ID (platform keys with `p:`). No personal data in values beyond IDs, except hashed OTP codes.

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| No Redis (MySQL only, as nova-sg-all) | One less component | The owner asked for Redis; grid and dashboard reads benefit |
| Redis as job queue (Asynq, BullMQ style) | Mature libraries | Loses the transactional outbox guarantee; jobs must commit with the business change |
| In-process cache only | Simplest | Multiple API replicas would disagree; invalidation is harder |

## Consequences

One more service to run and monitor; tests must cover Redis-down behaviour. Cache invalidation bugs show stale data, never wrong authorization, because permission checks re-read the version on write paths.

## Acceptance and revisit criteria

Accept the usage rules when the S1 cache adapter has hit/miss/invalidation tests and a Redis-down test. Revisit if a real-time feature (Phase 2 live board) needs fan-out; then add Redis Pub/Sub for notifications of change only, never as the record.
