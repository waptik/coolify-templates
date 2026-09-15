# Repository instructions

## Scope

This repository contains public, copy-ready Docker Compose templates for
Coolify. A template lives in `templates/<template-slug>/` and must be
independently understandable.

## Adding or changing a template

- Include `docker-compose.yaml` and `README.md` in every template directory.
- Add the template to the table in the root `README.md`.
- Use descriptive, lowercase, hyphen-separated directory names.
- Keep secrets out of the repository. Use required Compose interpolation such as
  `${VARIABLE:?}` for required values and document every required variable.
- Prefer Coolify's `SERVICE_URL_<SERVICE>_<PORT>` variables for public HTTP
  services. Do not add host `ports` mappings unless direct host access is an
  explicit part of the template's purpose.
- Include a Docker health check when the service supports one. The check must
  only use executables present in the image.
- Always include a Watchtower service for automatic image updates. Add the
  `com.centurylinklabs.watchtower.enable: "true"` label to the main service and
  configure Watchtower to update only labeled services daily at midnight.
- Document required prerequisites, how to deploy, the public URL format,
  persistent data, and any important security implications.
- Use current official documentation for version-sensitive Coolify behavior and
  link to the source in the template README.

## Validation

Before committing a Compose change, validate its syntax with
`docker compose -f templates/<template-slug>/docker-compose.yaml config` after
supplying non-secret placeholder values for required variables. Do not run
commands that expose or print real secrets.
