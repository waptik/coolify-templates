# GhanaPostGPS REST API

Deploys
[jayluxferro/ghanapostgps-api](https://hub.docker.com/r/jayluxferro/ghanapostgps-api),
a REST API wrapper for Ghana's official digital addressing system
(GhanaPostGPS). The service converts between GhanaPostGPS addresses and
geographic coordinates (latitude/longitude).

The API is exposed through Coolify's wildcard-domain proxy on port 9091.

The template also includes Watchtower, configured to update only the
GhanaPostGPS service each day at midnight.

## Prerequisites

- A Coolify server with a wildcard domain configured.

## Deploy with Coolify

1. Create a **Docker Compose** service in Coolify.
2. Paste the contents of [`docker-compose.yaml`](./docker-compose.yaml).
3. Deploy. Coolify generates the `SERVICE_URL_GHANAPOSTGPS_9091` URL.
4. Use the generated URL to access the API endpoints.

There is no host `ports` mapping: the API is available only through the Coolify
URL.

## API Endpoints

The service provides two main endpoints:

### Get Location from Address

- **Endpoint:** `POST /get-location`
- **Request body:** `{"address": "AK-484-9321"}` or `address=AK-484-9321`
- **Response:** JSON with latitude/longitude coordinates and location details

### Get Address from Coordinates

- **Endpoint:** `POST /get-address`
- **Request body:** `{"lat": "6.6500", "long": "-1.6487"}` or
  `lat=6.6500&long=-1.6487`
- **Response:** JSON with GhanaPostGPS address and location information

## Health check

The image includes Alpine's `wget` for health checks. The template checks the
root endpoint on port 9091. Coolify/Traefik routes traffic only after this
Docker health check succeeds.

## Persistence

This template does not include persistent volumes. The GhanaPostGPS API is
stateless and does not require data persistence.

## Security notes

- The service runs as the `nobody` user for security.
- Ensure your Coolify instance uses HTTPS to protect API traffic.
- Watchtower has access to Docker's socket so it can update the labeled
  GhanaPostGPS container. Remove the `watchtower` service and the GhanaPostGPS
  label if automatic image updates are not appropriate for your deployment
  process.

## References

- [GhanaPostGPS API documentation](https://ghanapostgps.sperixlabs.org/)
- [GhanaPostGPS-REST-API source](https://github.com/jayluxferro/GhanaPostGPS-REST-API)
- [Coolify Docker Compose services](https://coolify.io/docs/knowledge-base/docker/compose)
