# ADR 0008: Driver Self-Service as a Next.js PWA in Phase 1

Status: **Proposed** (P-04)
Date: 2026-10-28
Decision owner: Nova Logistics product owner
Related: [product](../01-product-and-mvp.md), [ADR-0001](0001-go-modular-monolith-and-nextjs.md)

## Context

Drivers must upload and renew documents, run vehicle inspections with photos and see their warnings (MVP-04, MVP-15). The owner chose Go and Next.js (D-09). TTP already runs its rider screens as a PWA/TWA from Next.js. nova-sg-all uses Flutter because check-in needs background location, offline queues and native camera control; that is Phase 2 here (D-03).

## Decision

- Phase 1 driver screens are a mobile-first part of the same Next.js app, installable as a PWA, opened from LINE (LIFF deep link) or a browser.
- Capture uses the browser camera input with client-side resize; uploads go straight to storage with pre-signed URLs and resume on failure.
- Sign-in by LINE Login or phone OTP (P-05).
- The native-app decision (Flutter, as nova-sg-all) is made before Phase 2 check-in, based on background-location and offline needs.

## Alternatives

| Alternative | Benefit | Why not now |
| --- | --- | --- |
| Flutter app from day one | Native camera, offline, GPS ready for Phase 2 | Third codebase and store releases before Phase 1 needs them |
| LINE bot only (as TTP intake) | Zero install | Weak for checklists and photo sets; keep as an entry point |

## Consequences

One codebase and no store releases in Phase 1. In-app browsers (LINE, Facebook) can break file upload; TTP met this and added a guard that asks the rider to open the external browser. iOS PWA limits apply.

## Acceptance and revisit criteria

Accept after an S1 spike uploads a 6-photo inspection on a low-end Android phone over 3G-like throttling and from LINE's in-app browser.
