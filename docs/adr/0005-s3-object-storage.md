# ADR 0005: S3-Compatible Object Storage, MinIO Locally

Status: **Accepted** 2026-10-28 (owner: "the document using s3", "minio for s3" locally, D-09, D-10). Cloud: NIPA S3 (D-28).
Date: 2026-10-28
Decision owner: Nova Logistics engineering owner
Related: [architecture](../03-architecture.md#files-zip-and-the-document-browser), [environment workflow](../09-environment-workflow.md)

## Context

Documents (ID cards, licences, insurance policies, registration books), vehicle photos and inspection photos are the heaviest data. TTP stores files in Google Drive and GCS; SWP in GCS with signed URLs; nova-sg-all in MinIO locally and NIPA S3 in the cloud through one S3 adapter. Users want a bucket-style browser, ZIP download and ZIP/bulk upload.

## Decision

- One S3 adapter (aws-sdk-go-v2) used in every environment; only endpoint, region, credentials, path style and TLS change. Locally MinIO in Docker; in the cloud NIPA S3 (D-28). The adapter stays S3-generic, so another provider remains possible.
- One private bucket per environment. Object key: `t/{tenant_id}/{module}/{yyyy}/{mm}/{object_id}`; the original file name lives only in MySQL metadata.
- MySQL keeps the object metadata: tenant, owner record, key, size, MIME type sniffed from content, SHA-256, uploader, capture time, GPS (photos), retention class.
- Browser and app upload straight to storage with a short-lived pre-signed PUT (multipart for files over 16 MB) after the API checks access and quota; the API confirms the upload by checking size and hash.
- Downloads use pre-signed GET URLs valid at most 5 minutes, issued after a fresh access check and audited. ZIP downloads are built by the worker into a temporary object that expires after 24 hours.
- Images are re-encoded without EXIF; capture time and GPS are copied to metadata first.
- Lifecycle rules delete temporary objects; retention by document type is applied by the worker, not by bucket rules alone.

## Alternatives

| Alternative | Benefit | Why not |
| --- | --- | --- |
| GCS native API | TTP/SWP already use it | Locks the product to one cloud; MinIO would not match locally |
| Files in MySQL BLOBs | One backup | Database bloat; no direct upload; slow ZIPs |
| Public-read bucket with obscure keys | Simple links | Leaks personal documents; no audit |

## Consequences

Portable across clouds; local tests use the same adapter. A green MinIO test does not prove the cloud provider's S3 behaviour: run the storage checks again on UAT.

## Acceptance and revisit criteria

Before UAT, exercise put, get, multipart, pre-signed grants, delete and lifecycle on the chosen provider with synthetic files, and rehearse a restore of objects and metadata together.
