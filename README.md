# Pulsechain Validator Metrics
## Note: This is a fork of the original [Lighthouse Metrics](https://github.com/sigp/lighthouse-metrics), created by Sigma Prime.

If you are running a Lighthouse node in docker and would like visibility of your docker logs within Grafana, this is for you!


[![metrics.png](https://i.postimg.cc/Jh7rxtgp/metrics.png)](https://postimg.cc/4YMRN4Xc)

Provides a `docker-compose` environment which scrapes metrics from Lighthouse
nodes using Prometheus and presents them in a browser-based Grafana GUI.

This also uses Grafana's [Loki](https://grafana.com/oss/loki/) and [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/) for aggregating docker logs for your running containers.

[![logs.png](https://i.postimg.cc/8Cj3jPKc/image.png)](https://postimg.cc/McJtLx8J)

## Usage

1. Install the docker plugin for Loki on your validator node:
    ```sh 
    docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
    ```
2. Start a lighthouse node (beacon or validator) with Docker
    - If using `docker run` set the log-driver to `loki` and set the `loki-url`. Example: 
        ```sh
        docker run --name validator \
            --log-driver=loki \
            --log-opt loki-url="http://localhost:3100/loki/api/v1/push" \
        ```
    - If using Docker Compose it would look something like this:
        ```yaml
        logging:
            driver: loki
            options:
                loki-url: "http://localhost:3100/loki/api/v1/push"
        ```
    - Note: The `--metrics` flag is required in both cases for Prometheus metrics.
3. Bring the environment up with `$ docker-compose up --build -d`.
4. Ensure that Prometheus can access your Lighthouse node by ensuring it is in
   the `UP` state at [http://localhost:9090/targets](http://localhost:9090/targets).
5. Browse to [http://localhost:3000](http://localhost:3000)
    - Username: `admin`
    - Password: `changeme`
6. Add Loki as a new datasource in Grafana. Just set the URL to http://localhost:3100 and click "Save & Test"
7. You can now create your own dashboards to show your container logs. See the example /dashboards/ValidatorLogs.json for a starting example or inspiration.
8. Import some other dashboards from the `dashboards` directory in this repo:
    - In the Grafana UI, go to `Dashboards` -> `Manage` -> `Import` -> `Upload .json file`.
    - The `Summary.json` dashboard is a good place to start.

   
## Hosting Publicly

By default Prometheus and Grafana will only bind to localhost (127.0.0.1), in
order to protect you from accidentally exposing them to the public internet. If
you would like to change this you must edit the `http_addr` in `grafana.ini`.

## Maintenance and Contributing

Feel free to create your own dashboards, export them and submit them here as
PRs.
