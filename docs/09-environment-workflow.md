# Local and Cloud Environment Workflow

**Status: local topology confirmed by the owner on 28 October 2026 (D-10); cloud open (P-02).** The local service file exists (`platform/compose.yaml`: MinIO and Redis). The backend and web app do not exist yet, so the `apps` profile is added with the scaffold.

## Topology

| Environment | MySQL 8.4 | Object storage | Redis | Application |
| --- | --- | --- | --- | --- |
| Developer local | The developer's **existing host MySQL 8.4**, outside Docker | **MinIO in Docker**, private dev and test buckets | **Redis 7.4 in Docker** | API, worker and web on the host, later also as containers with the `apps` profile |
| Cloud UAT | Managed MySQL 8.4 (provider P-02) | S3-compatible bucket, private | Managed Redis or a container | Same images as production |
| Cloud production | Separate managed MySQL 8.4 | Separate private bucket | Separate Redis | Same versioned release |

Compose never starts MySQL. Stopping the stack or removing its volumes resets MinIO and Redis only, never the database.

## Local developer loop

1. **Database.** On the host MySQL 8.4, create the schema and two users once (replace the placeholders; never commit real passwords):

   ```sql
   CREATE DATABASE nova_logistics CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
   CREATE DATABASE nova_logistics_test CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;
   CREATE USER 'nl_migrate'@'%' IDENTIFIED BY '<migrate-password>';
   CREATE USER 'nl_app'@'%' IDENTIFIED BY '<app-password>';
   GRANT ALL PRIVILEGES ON nova_logistics.* TO 'nl_migrate'@'%';
   GRANT ALL PRIVILEGES ON nova_logistics_test.* TO 'nl_migrate'@'%';
   GRANT SELECT, INSERT, UPDATE, DELETE ON nova_logistics.* TO 'nl_app'@'%';
   GRANT SELECT, INSERT, UPDATE, DELETE ON nova_logistics_test.* TO 'nl_app'@'%';
   ```

   Containers reach the host database at `host.docker.internal`; host processes use `127.0.0.1`.
2. **Configuration.** Copy `platform/.env.example` to `platform/.env` and fill local-only values. `platform/.env` is ignored by Git.
3. **Services.** Start MinIO, the bucket bootstrap and Redis:

   ```bash
   docker compose --env-file platform/.env -f platform/compose.yaml up -d --wait
   ```

   MinIO API `127.0.0.1:9000`, console `127.0.0.1:9001`; Redis `127.0.0.1:6379` with a password. Every port binds to loopback.
4. **Migrations** (when the backend exists): `migrate up` with the `nl_migrate` user; the API and worker run as `nl_app`.
5. **Smoke check** (when the backend exists): health endpoint, a private upload and download through MinIO, a cache hit and miss, a denied cross-tenant read, an outbox retry.
6. **Reset.** `docker compose -f platform/compose.yaml down -v` clears MinIO and Redis only.

## Configuration contract (to implement)

| Variable | Used by | Meaning |
| --- | --- | --- |
| `NL_ENV` | all | `local`, `uat`, `production` |
| `NL_DB_HOST`, `NL_DB_PORT`, `NL_DB_NAME`, `NL_DB_TLS` | api, worker, migrate | MySQL 8.4 connection |
| `NL_DB_USER`, `NL_DB_PASSWORD` | api, worker | Runtime identity (DML only) |
| `NL_MIGRATE_DB_USER`, `NL_MIGRATE_DB_PASSWORD` | migrate | Migrator identity (DDL) |
| `NL_REDIS_ADDR`, `NL_REDIS_PASSWORD`, `NL_REDIS_DB`, `NL_REDIS_TLS` | api, worker | Cache |
| `NL_S3_ENDPOINT`, `NL_S3_REGION`, `NL_S3_BUCKET`, `NL_S3_PATH_STYLE`, `NL_S3_ACCESS_KEY_ID`, `NL_S3_SECRET_ACCESS_KEY` | api, worker | Object storage |
| `NL_S3_MAX_GRANT_TTL` | api | Longest pre-signed URL (≤ 5m) |
| `NL_FIELD_KEY` | api, worker | Local development key for field encryption (cloud: KMS, P-17) |

Each process gets only the variables it reads; the migrator's password never reaches the API or the worker.

## Cloud release loop (when P-02 is decided)

Verify separate UAT and production databases, buckets and Redis; TLS to each; secrets in the provider's secret store; S3 put/get/multipart/pre-signed/lifecycle on the real provider with synthetic files (a MinIO pass is not proof); backup and restore of MySQL and objects together; then migrate, deploy, health and smoke checks.
