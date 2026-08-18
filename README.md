# Coolify Templates

A small collection of Docker Compose templates for deploying self-hosted services with [Coolify](https://coolify.io/).

Each template is self-contained: it includes its Compose definition and deployment-specific documentation.

## Templates

| Template | Purpose |
| --- | --- |
| [Grafana LGTM](./templates/grafana-lgtm/) | All-in-one Grafana, Loki, Tempo, Prometheus, Pyroscope, and OpenTelemetry Collector stack for development and testing. |
| [Telegram Bot API server](./templates/telegram-bot-api/) | Run a local Telegram Bot API server behind Coolify's Traefik proxy. |

## Using a template

1. Open the template's directory and read its `README.md`.
2. Copy its `docker-compose.yml` into a new Coolify Docker Compose service.
3. Supply all required variables in Coolify, then deploy.

Templates are designed for Coolify's wildcard-domain routing. They normally do not publish application ports directly to the host.

## Contributing

See [AGENTS.md](./AGENTS.md) for the repository conventions used when adding or changing templates.
