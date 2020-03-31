# Rocket.Chat.Metrics

![](https://i.imgur.com/EatD1VO.png)

This repository contains a basic Monitoring stack for dockerized [Rocket.Chat](https://github.com/RocketChat/Rocket.Chat) setups, based on Grafana, Prometheus, cadvisor and node-exporter. For exemplary purposes it also comes with a "default" Rocket.Chat [`docker-compose.yml`](docker-compose.yml) file - of course, this can be adjusted as needed.

## Installation

1. Clone this repository:

    ```shell
    git clone https://github.com/RocketChat/Rocket.Chat.Metrics /opt/docker/Rocket.Chat.Metrics
    cd /opt/docker/Rocket.Chat.Metrics
    ```

2. Copy and adjust the default environment variables from the `.env.sample` file:

    ```shell
    cp .env.sample .env
    vi .env
    ```

3. Create and start up containers using `docker-compose`. Make sure to include *both* compose files:

    ```
    docker-compose -f docker-compose.yml -f docker-compose.monitoring.yml up -d
    ```

4. Access your Grafana instance via `http://${HOST_IP}:3300`.

## Usage

As soon as the containers are up and running, you can access the Grafana UI via the configured port (`${GRAFANA_PORT}`, defaults to `3300`) and select the autoprovisioned dashboard:

- _Rocket.Chat Metrics Simple_
- _Rocket.Chat Metrics_
- _Docker and system monitoring_

## Troubleshooting

### My Rocket.Chat compose setup uses a different container name for the `rocketchat` container.

Make sure to update the `depends_on` option in the monitoring compose file within the `prometheus` section as well as update the hostname in Prometheus' [scrape config](config/prometheus/prometheus.yml) (`./config/prometheus/prometheus.yml`).

### I want to access the Prometheus Web UI.

By default the port for Prometheus' Web UI is not exposed. To enable it simple add a `ports` mapping in the `prometheus` section of the monitoring compose file:

```
    ports:
     - 9090:9090
```

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

* Docker

## License

[MIT](LICENSE)
