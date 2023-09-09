# Rocket.Chat.Metrics

![](https://i.imgur.com/EatD1VO.png)

This repository contains a basic Monitoring setup [Rocket.Chat](https://github.com/RocketChat/Rocket.Chat) setups, based on Grafana and Prometheus. It currently shows and visualizes the following application metrics/information:

- _General totals_ (users online/away, DDP users, diffs based on time range)
- _Metrics_ (requests and total size per instance)
- _NodeJS_ (active handles, requests, event loop lag, heap used, per instance heap, garbage collector)
- _DDP rate limiter_ (by method, by type, by userId, by connectionId)
- _Rocket.Chat Data_ (messages sent, total sent, user presence, users & sessions, Oplog, push queue, Meteor facts, total users, total rooms, notification per minute per notification type)
- _Meteor_ (methods total time, methods time, method calls per minute)
- _Subscriptions_ (subscription total time, subscription time, subscription calls per minute)
- _Callbacks & Hooks_ (callbacks total time, callbacks time, callback calls per minute, hooks total time, hooks time, hook calls per minute)
- _REST API_ (REST total time, REST time, REST calls per minute)

## Preparation

1. Make sure you meet all [requirements](#requirements--dependencies).

2. Clone this repository:

    ```shell
    git clone https://github.com/RocketChat/Rocket.Chat.Metrics /opt/docker/Rocket.Chat.Metrics
    cd /opt/docker/Rocket.Chat.Metrics
    ```

3. Adjust the default Prometheus configuration according the [usage information below](#prometheus) (either for Docker-based or static Rocket.Chat setups).

4. Create and start up containers using `docker-compose`. Make sure to include *both* compose files:

    ```shell
    docker-compose up -d
    ```

5. Access your Grafana instance via `http://${HOST_IP}:3000`.

    Note: If you host this metrics setup on the same system as your Rocket.Chat server, you might run into port collisions when trying to bind Grafana to port 3000 as it is already used by Rocket.Chat. Feel free to adjust it in the [`docker-compose.yml`](./docker-compose.yml) as needed.

## Initial configuration

### Rocket.Chat

To allow Prometheus to scrape data from Rocket.Chat you need to make sure to enable the exporter within the Rocket.Chat admin UI → Logs → Prometheus:

![](https://i.imgur.com/b5zJOLB.png)

### Prometheus

#### Monitor a single Rocket.Chat server

To monitor one or multiple static Rocket.Chat systems ("static" in terms of the Prometheus metrics endpoint doesn't change and is always available under the same address) you just have to add each of your Rocket.Chat hostnames in the [`./config/prometheus/prometheus.yml`](config/prometheus/prometheus.yml) file like this:

```yaml
- job_name: rocketchat_static
  static_configs:
  - targets:
    - rocketchat1.company.com:9458
    - rocketchat2.company.com:9458
```

Where `rocketchat1.company.com` and `rocketchat2.company.com` are reachable endpoints from within the Prometheus container.

#### Monitor a multi-instance or Docker-based Rocket.Chat server/setup

In case you have a Docker-based setup that might scale additional application instances in and out on the fly, you can use Prometheus DNS discovery method. Please make sure that the Prometheus container is within the same Docker network as your Rocket.Chat application containers. Use a configuration like this:

```yaml
- job_name: rocketchat_docker
  scrape_interval: 30s
  dns_sd_configs:
  - names: ["rocketchat"]
    type: A
    port: 9458
```

Make sure to update `rocketchat` with the container name of your Rocket.Chat application container.

### Grafana

#### Using Grafana

As soon as the containers are up and running, you can access the Grafana UI via the configured port and select the autoprovisioned dashboard "_Rocket.Chat Metrics_".

By default Grafana uses `admin`/`admin` as login credentials.

#### Default credentials

#### Additional system and Docker monitoring dashboards (optional)

- cadvisor
- node-exporter

## Troubleshooting

### I do not want to access the Prometheus Web UI.

By default the port for Prometheus' Web UI is exposed for easier metric troubleshooting. To disable it simple remove the `ports` mapping in the `prometheus` section of the compose file:

```yaml
    ports:
     - 9090:9090
```

### How to increase the persistent storage/history of Prometheus to more than 12 weeks?

You can adjust this by setting the `storage.tsdb.retention.time` command argument in the Prometheus container. For example:

```yaml
  prometheus:
    image: quay.io/prometheus/prometheus:v2.16.0
    restart: unless-stopped
    command:
      - --config.file=/etc/prometheus/prometheus.yml
      - '--storage.tsdb.retention.time=1y'
      - '--storage.tsdb.path=/prometheus'
```

### Prometheus container doesn't start when running by non-root Docker user

If you [manage your Docker containers as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user), you might encounter the following error when trying to start the Prometheus container for the first time:

```
level=error ts=2019-08-30T05:14:43.160Z caller=query_logger.go:82 component=activeQueryTracker msg="Error opening query log file" file=/prometheus/queries.active err="open /prometheus/queries.active: permission denied"
panic: Unable to create mmap-ed active query log
```

To fix this, adjust the permissions of the Prometheus data directory to UID/GID `65534`:

```shell
sudo chown 65534:65534 /opt/docker/Rocket.Chat.Metrics/data/prometheus/
```

More information about this can be found [in the following GitHub comment](https://github.com/prometheus/prometheus/issues/5976#issuecomment-532942295) from the official Prometheus repo.

## Metrics in detail

To help reading and interpreting the graphs of this Grafana dashboard, you can find a document that explains most of our metrics and graphs that are included here:

[📒 ./metrics.md](./metrics.md)

## Contributing

1. Fork it
2. Create your feature branch:

    ```shell
    git checkout -b feature/my-new-feature
    ```

3. Commit your changes:

    ```shell
    git commit -am 'Add some feature'
    ```

4. Push to the branch:

    ```shell
    git push origin feature/my-new-feature
    ```

5. Submit a pull request

## Requirements / Dependencies

* Up and running Rocket.Chat system
* Docker

## License

[MIT](LICENSE)

<!-- TODO -->
