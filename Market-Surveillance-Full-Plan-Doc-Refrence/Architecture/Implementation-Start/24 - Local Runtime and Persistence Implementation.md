---
id: IMPL-START-24
type: implementation-reference
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - deployment/local
  - postgresql
  - redis
---

# Local Runtime and Persistence Implementation

Parent: [[16 - Development Implementation Snapshot]]

## `run-local.sh` is the current local orchestrator

**Implemented.** The script starts THE EYE runtime hosts in dependency order and expects Kafka and Redis to be reachable first.

Current startup order:

```text
prerequisite Kafka + Redis
        |
        v
PostgreSQL persistence readiness + DB migrations
        |
        v
1. TheEye.Api          (ASP.NET + Orleans silo)
2. TheEye.SiloConsumer (Kafka -> Orleans client)
3. GalaxyProjection    (Kafka -> Redis)
4. TheEye.Ingestion    (source ingestion / canonical production)
5. Galaxy.Web          (Vite dev server)
```

Source: [run-local.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/run-local.sh).

## Local default endpoints/dependencies

The script currently uses these defaults unless overridden:

```text
Kafka:      localhost:29092
Redis:      localhost:6380
API:        http://localhost:5175
Galaxy Web: http://localhost:5173
PostgreSQL: localhost:5433
```

PostgreSQL is a hard dependency because the Orleans silo registers ADO.NET grain storage and reminders.

## Persistence compose

`docker-compose.persistence.yml` currently defines:

### PostgreSQL

- image: `postgres:17-alpine`
- host port: `5433`
- named volume: `theeye_postgres_data`
- health check with `pg_isready`

### Redis

- image: `redis:7-alpine`
- host port: `6380`
- named volume: `theeye_redis_data`
- custom config from `infra/redis/redis.conf`
- `redis-cli ping` health check

Source: [docker-compose.persistence.yml](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/docker-compose.persistence.yml).

The `infra` root contains dedicated folders for:

```text
infra/feature-archive
infra/orleans
infra/redis
```

## Database migration behavior

At the audited development head, local startup calls:

```text
scripts/persistence-db.sh apply-features
```

This means local startup applies the Orleans persistence schema path and the feature-archive migration path required by the current PostgreSQL feature writer setup.

## Development authentication behavior

The Galaxy web dev path uses a static development token from the gitignored `.env.local`. `run-local.sh` passes the matching symmetric signing key into the API when available. If no key is supplied, the API remains protected and Galaxy requests receive `401` rather than silently bypassing authentication.

This is a **development convenience path**, not a production identity architecture.

## Important deployment boundary

The local script is useful implementation evidence, but it is not proof of the final HA/production container topology. Production HA, secrets, network policy, replicas and operational deployment remain separate deployment-design concerns.

Related:
- [[17 - Runtime Pipeline and Orleans Implementation]]
- [[19 - Feature Store and Archive Implementation]]
- [[20 - Galaxy Implementation]]
- [[21 - Current Implementation Gaps and Known Defects]]
