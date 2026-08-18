# Grafana LGTM

Deploys Grafana's [`grafana/otel-lgtm`](https://github.com/grafana/docker-otel-lgtm) image with PostgreSQL for Grafana configuration. The image bundles Grafana, Loki, Tempo, Prometheus, Pyroscope, and an OpenTelemetry Collector (the “LGTM” observability stack) in one container.

> This is an all-in-one image intended by Grafana for development, demo, and testing environments. It is not a production observability architecture.

The Grafana UI is exposed through Coolify's wildcard-domain proxy. The stack uses GitHub OAuth plus Grafana's local admin login.

## Prerequisites

- A Coolify server with a wildcard domain configured.
- A GitHub OAuth app. Set its authorization callback URL to:

  ```text
  https://<generated-grafana-domain>/login/github
  ```

  The generated domain is available as `SERVICE_FQDN_GRAFANA` after Coolify processes the Compose file.

## Deploy with Coolify

1. Create a **Docker Compose** service in Coolify.
2. Paste the contents of [`docker-compose.yml`](./docker-compose.yml).
3. Set these required environment variables in Coolify:

   - `GITHUB_CLIENT_ID` — GitHub OAuth app client ID.
   - `GITHUB_CLIENT_SECRET` — GitHub OAuth app client secret.

4. Deploy. Coolify generates the remaining `SERVICE_*` variables, including database credentials, the Grafana admin password, and the public Grafana URL.
5. Open the generated `SERVICE_URL_GRAFANA_3000` URL. Sign in through GitHub or as `admin` with the generated `SERVICE_PASSWORD_GRAFANA` value.

There is no host `ports` mapping: the dashboard is available only through the Coolify URL. Services in the same Coolify Compose network can send telemetry to `http://grafana:4318` (OTLP/HTTP) or `grafana:4317` (OTLP/gRPC).

## Persistence and health

- `grafana-data` stores Grafana data.
- `postgresql-data` stores Grafana's PostgreSQL database.
- `lgtm-data` stores Loki, Tempo, Pyroscope, and Prometheus data under `/data`.

The upstream image includes its own Docker health check, which checks the bundled services. Coolify/Traefik will route to the dashboard only when the container is healthy.

## Configuration notes

- GitHub OAuth is enabled and anonymous access is disabled. Remove or change the `GF_AUTH_GITHUB_*` variables if you use a different authentication provider.
- `GF_LOG_LEVEL` defaults to `info`; set it in Coolify to `debug` only while troubleshooting.
- The default OpenTelemetry Collector endpoints are `4317` for gRPC and `4318` for HTTP/protobuf.
- Pin `grafana/otel-lgtm` to a tested image version before depending on a deployment long term; this template uses `latest` to match the upstream quick-start behavior.

## References

- [Grafana docker-otel-lgtm documentation](https://grafana.com/docs/opentelemetry/docker-lgtm/)
- [Grafana docker-otel-lgtm source](https://github.com/grafana/docker-otel-lgtm)
- [Coolify Docker Compose services](https://coolify.io/docs/knowledge-base/docker/compose)
