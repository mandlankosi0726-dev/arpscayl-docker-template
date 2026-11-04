# ArpScayl Production Docker Application Template

A reusable FastAPI, PostgreSQL, and Docker foundation. This repository is a starting point, not a claim that a deployed service is production-ready. Example CRUD routes have no authentication or authorization and must not be exposed to untrusted users until those controls and rate limits are added.

## Start locally

Prerequisites: Python 3.12, Docker Engine with Compose v2, and Make. The checked-in `.env.example` and Compose development database credentials are disposable local-only values.

```sh
cp .env.example .env
make up-dev
make migrate
curl http://localhost:8000/health/ready
```

Visit `http://localhost:8000/docs` in development. Create and list an example with `POST` and `GET /api/v1/example-resources`. Run `make install`, `make lint`, `make test`, and `make validate` for local checks.

## Production use

Review `docs/production-readiness.md`, then the deployment, configuration, migration, and recovery guides before adapting this template. Production Compose expects immutable `APP_IMAGE`, explicit `ALLOWED_HOSTS`, and host-managed secret files. Example: `docker compose -f compose.yaml -f compose.prod.yaml config` reviews the merged configuration; run production migrations as a separate controlled operation using that same production configuration. `make migrate` is for the local development database only. Never run production commands until target, backup, and change authorization have been checked.

This single-host Compose model is not highly available. TLS termination is expected at a trusted external load balancer or reverse proxy. See `docs/architecture.md` for boundaries and limitations.

## Implemented versus operator work

The repository includes API routes, input schemas, SQLAlchemy persistence, Alembic migration, health/readiness, structured stdout logs, non-root image settings, Compose samples, CI workflows, and operational documentation. Operators still need to provision and harden hosts, establish secrets and TLS, implement business-specific identity and authorization, configure external monitoring, verify backups/restores, and authorize releases. The ArpScayl CI/CD and BDR toolkit integrations are documented interfaces only; no toolkit repository or reusable workflow was provided.
# arpscayl-docker-template
