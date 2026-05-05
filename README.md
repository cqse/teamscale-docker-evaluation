# Teamscale Docker Evaluation

Minimal Docker Compose setup to evaluate Teamscale locally.
For production deployments with HTTPS, multi-user reverse-proxy topology, and zero-downtime feature-version upgrades, see [cqse/teamscale-docker-deployment](https://github.com/cqse/teamscale-docker-deployment) instead.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- The [Docker Compose plugin](https://docs.docker.com/compose/install/) (bundled with Docker Desktop; on Linux install `docker-compose-plugin`)

## Quickstart

1. Get an evaluation license at [teamscale.com/demo](https://teamscale.com/demo) and save it to `./data/config/teamscale.license` (create the `config` folder yourself).
2. Start the container:
   ```sh
   docker compose up -d
   ```
3. Open <http://localhost:8080>. The first-time setup wizard runs in the browser.

## Common tasks

Stop:
```sh
docker compose down
```

View logs:
```sh
docker compose logs -f
```

Update to the latest patch release:
```sh
docker compose pull && docker compose up -d
```

Wipe and start over (the license file in `data/config/` is preserved):
```sh
docker compose down && rm -rf data/storage data/logs && docker compose up -d
```

## Tweaking the setup

All changes go in `docker-compose.yml`; nothing in `data/config/` needs to be touched.

### Increase memory

```yaml
services:
  teamscale:
    environment:
      TEAMSCALE_MEMORY: 8G
```

### Change the host port (when 8080 is in use)

```yaml
services:
  teamscale:
    ports:
      - "9090:8080"
```

### Override any other Teamscale property

```yaml
services:
  teamscale:
    environment:
      TS_PROPERTIES: |-
        engine.workers=4
        instance.name=my-eval
```

Full property list: [Configuring Teamscale](https://docs.teamscale.com/reference/administration-ts-installation/#configuring-teamscale).

## Custom logging

Logs go to stdout (`docker compose logs -f`) and to `data/logs/teamscale.log` using the image's bundled defaults.
To customize log routing (file rotation, Splunk forwarding, JSON output, etc.) drop a `log4j2.yaml` into `data/config/` and restart.
The production repo ships a working example to copy from: [`v2025.1/config/log4j2.yaml`](https://github.com/cqse/teamscale-docker-deployment/blob/master/v2025.1/config/log4j2.yaml).

## Pinning a different version

Edit the `image:` line in `docker-compose.yml`. Available tags are listed at [hub.docker.com/r/cqse/teamscale/tags](https://hub.docker.com/r/cqse/teamscale/tags).

There is no `latest` tag because feature-version upgrades require re-analysis. Pin to a specific feature line: `2026.3.latest` rolls forward across patches automatically; `2026.3.2` is frozen to one patch release.
