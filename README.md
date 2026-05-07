# Teamscale Simple Docker Setup                                                                                                                                          
> [!IMPORTANT]
> This is a simplified setup to run Teamscale on your local machine. It is not recommended for production usage. For production deployments see [cqse/teamscale-docker-deployment](https://github.com/cqse/teamscale-docker-deployment) instead.       


## Quickstart

1. Get an evaluation license at [https://teamscale.com/try](https://teamscale.com/try) and save it to `./data/config/teamscale.license`.
2. Start the container:
   ```sh
   docker compose up -d
   ```
3. Open <http://localhost:8080>.

## Tweaking the setup

All changes go in `docker-compose.yml`, nothing in `data/config/` needs to be touched.

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
