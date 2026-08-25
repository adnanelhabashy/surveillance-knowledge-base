---
id: IMPL-START-24
type: implementation-reference
status: code-verified
audited_commit: 0b4af2e99e530ce56a94d894865c761b7d7306e8
tags:
  - surveillance/implementation
  - deployment/local
  - postgresql
  - redis
  - kafka
---

# Local Runtime and Persistence Implementation

Parent: [[16 - Development Implementation Snapshot]]

## `run-local.sh` is the current local orchestrator

**Implemented.** The script starts THE EYE runtime hosts in dependency order. Kafka and Redis must be reachable first; PostgreSQL can be started from the persistence compose when needed.

Current startup order:

```text
prerequisite Kafka + Redis
        |
        v
PostgreSQL readiness + feature archive migration
        |
        v
1. TheEye.Api          (ASP.NET + Orleans silo)
2. TheEye.SiloConsumer (Kafka -> Orleans client)
3. GalaxyProjection    (Kafka -> Redis)
4. TheEye.Ingestion    (source ingestion / canonical production)
5. Galaxy.Web          (Vite dev server)
```

Source: [run-local.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/run-local.sh).

## Local default endpoints/dependencies

```text
Kafka runtime default: localhost:29092
Redis:                 localhost:6380
API:                   http://localhost:5175
Galaxy Web:            http://localhost:5173
PostgreSQL:            localhost:5433
PostgreSQL database:   theeye
```

PostgreSQL is a hard dependency for the silo because Orleans registers ADO.NET grain storage and reminders.

## Persistence compose

`docker-compose.persistence.yml` defines:

### PostgreSQL

- image: `postgres:17-alpine`
- database: `theeye`
- host port: `5433`
- named volume: `theeye_postgres_data`
- `pg_isready` health check

### Redis

- image: `redis:7-alpine`
- host port: `6380`
- named volume: `theeye_redis_data`
- custom config from `infra/redis/redis.conf`
- `redis-cli ping` health check

Source: [docker-compose.persistence.yml](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/docker-compose.persistence.yml).

## Local Kafka compose

**Implemented at the current head.** `docker-compose.kafka.yml` provides a single-node Kafka `3.9.0` KRaft broker plus Kafka UI.

Host listeners:

```text
localhost:9092   # integration/smoke script convention
localhost:29092  # run-local/runtime convention
```

Kafka UI:

```text
http://localhost:8085
```

Internally the UI connects to `kafka:19092`.

Source: [docker-compose.kafka.yml](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/docker-compose.kafka.yml).

## Infrastructure folders

The `infra` root contains dedicated folders for:

```text
infra/feature-archive
infra/orleans
infra/redis
```

## Database migration behavior

Be precise about the scripts:

- `scripts/persistence-db.sh apply` applies the Orleans PostgreSQL schemas.
- `scripts/persistence-db.sh apply-features` applies the feature-archive schema.
- `scripts/feature-archive-integration.sh` calls **both** before its integration scenarios.
- `run-local.sh` currently calls `apply-features` after ensuring PostgreSQL is reachable; therefore a pristine environment still needs the Orleans schema provisioned by the persistence setup path before the silo can rely on it.

Sources:
- [persistence-db.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/scripts/persistence-db.sh)
- [feature-archive-integration.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/scripts/feature-archive-integration.sh)
- [run-local.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/run-local.sh)

## Development authentication and CORS

The Galaxy web dev path uses a static development token from gitignored `.env.local`. `run-local.sh` passes the matching symmetric signing key into the API when available. If no key is supplied, protected Galaxy requests remain unauthorized rather than bypassing authentication.

Current API settings explicitly permit both local Vite origins:

```text
http://127.0.0.1:5173
http://localhost:5173
```

Source: [TheEye.Api/appsettings.json](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/TheEye.Api/appsettings.json).

This is a **development convenience path**, not a production identity architecture.

## Important deployment boundary

These files prove the current local/runtime implementation. They are not proof of the final HA/production topology, secrets strategy, replica counts, network policy or operational deployment.

Related:
- [[17 - Runtime Pipeline and Orleans Implementation]]
- [[19 - Feature Store and Archive Implementation]]
- [[20 - Galaxy Implementation]]
- [[21 - Current Implementation Gaps and Known Defects]]
