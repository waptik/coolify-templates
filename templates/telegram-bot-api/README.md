# Telegram Bot API server

Deploys
[aiogram/telegram-bot-api](https://hub.docker.com/r/aiogram/telegram-bot-api), a
self-hosted implementation of Telegram's Bot API, behind Coolify's Traefik
proxy. It is useful when you need a local Bot API server, including support for
local file access and larger uploads.

The template also includes Watchtower, configured to update only the Telegram
service each day at midnight.

## Prerequisites

- A Coolify server with a wildcard domain configured.
- A Telegram `api_id` and `api_hash`, created at
  [my.telegram.org](https://my.telegram.org/apps).

## Deploy with Coolify

1. Create a **Docker Compose** service in Coolify.
2. Paste the contents of [`docker-compose.yaml`](./docker-compose.yaml).
3. In Coolify's environment-variable UI, set:

   - `TELEGRAM_API_ID` — your numeric Telegram API ID.
   - `TELEGRAM_API_HASH` — your Telegram API hash.

4. Deploy the service.
5. Copy the generated `SERVICE_URL_TELEGRAM_8984` value from Coolify. Use it as
   the Bot API base URL, for example:

   ```text
   https://telegram-<resource-id>.<your-domain>/bot<BOT_TOKEN>/getMe
   ```

Coolify routes the generated subdomain to port `8984` inside the container. The
Compose file deliberately has no `ports` mapping, so the API is not separately
exposed at `server-ip:8984`.

## Health check

The image does not include `curl`, and `/` is not an appropriate Bot API health
endpoint. The template enables Telegram's internal statistics listener on port
`8082` and checks it with BusyBox `wget`. Coolify/Traefik routes traffic only
after this Docker health check succeeds.

## Persistent data

The `telegram-data` named volume is mounted at `/var/lib/telegram-bot-api`. Keep
this volume when redeploying so Telegram Bot API state persists.

## Security notes

- Treat the generated Coolify URL and every bot token as sensitive. Anyone with
  a bot token can call that bot's API methods through this server.
- `TELEGRAM_LOCAL=1` enables local-mode behavior. Use it only when your bot
  needs local Bot API features.
- Watchtower has access to Docker's socket so it can update the labeled Telegram
  container. Remove the `watchtower` service and the Telegram label if automatic
  image updates are not appropriate for your deployment process.

## References

- [Coolify: Docker Compose services](https://coolify.io/docs/knowledge-base/docker/compose)
- [Coolify: health checks](https://coolify.io/docs/knowledge-base/health-checks)
- [Telegram Bot API server](https://github.com/tdlib/telegram-bot-api)
