# Temporal Server docker-compose files

This repository contains the docker compose files to self-host temporal in our stack

## Prerequisites

To use these files, you must first have the following installed:

- [Docker](https://docs.docker.com/engine/installation/)
- [docker-compose](https://docs.docker.com/compose/install/)

## How to use

The following steps will run a local instance of the Temporal Server using the default configuration file (`docker-compose.yml`):

1. Clone this repository.
2. Change directory into the root of the project.
3. Generate the htpasswd file if not already generated. Note: This step will ask for password to be entered which is used to access the temporal UI
3. Run the `docker-compose up` command.

```bash
git clone git@github.com:CBX1/temporal-docker-compose.git
cd  temporal-docker-compose
htpasswd -c ./deployment/nginx/.htpasswd nginx
docker-compose up -f docker-compose-cbx1.yml
```

After the Server has started, you can open the Temporal Web UI in your browser: [http://{domain_name}:80](http://{domain_name}:80).

[//]: # (You can also interact with the Server using a preconfigured CLI &#40;tctl&#41;.)

[//]: # (First create an alias for `tctl`:)

[//]: # ()
[//]: # (```bash)

[//]: # (alias tctl="docker exec temporal-admin-tools tctl")

[//]: # (```)

[//]: # ()
[//]: # (The following is an example of how to register a new namespace `test-namespace` with 1 day of retention:)

[//]: # ()
[//]: # (```bash)

[//]: # (tctl --ns test-namespace namespace register -rd 1)

[//]: # (```)

[//]: # ()
[//]: # (You can find our `tctl` docs on [docs.temporal.io]&#40;https://docs.temporal.io/docs/system-tools/tctl/&#41;.)

[//]: # ()
[//]: # (Get started building Workflows with a [Go sample]&#40;https://github.com/temporalio/samples-go&#41;, [Java sample]&#40;https://github.com/temporalio/samples-java&#41;, or write your own using one of the [SDKs]&#40;https://docs.temporal.io/docs/sdks-introduction&#41;.)

[//]: # ()
[//]: # (### Other configuration files)

[//]: # ()
[//]: # (The default configuration file &#40;`docker-compose.yml`&#41; uses a PostgreSQL database, an Elasticsearch instance, and exposes the Temporal gRPC Frontend on port 7233.)

[//]: # (The other configuration files in the repo spin up instances of the Temporal Server using different databases and dependencies.)

[//]: # (For example you can run the Temporal Server with MySQL and Elastic Search with this command:)

[//]: # ()
[//]: # (```bash)

[//]: # (docker-compose -f docker-compose-mysql-es.yml up)

[//]: # (```)

[//]: # ()
[//]: # (Here is a list of available files and the dependencies they use.)

[//]: # ()
[//]: # (| File                                   | Description                                                   |)

[//]: # (|----------------------------------------|---------------------------------------------------------------|)

[//]: # (| docker-compose-cbx1.yml                | CBX1 version of temporal setup                                |)

[//]: # (| docker-compose.yml                     | PostgreSQL and Elasticsearch &#40;default&#41;                        |)

[//]: # (| docker-compose-tls.yml                 | PostgreSQL and Elasticsearch with TLS                         |)

[//]: # (| docker-compose-postgres.yml            | PostgreSQL                                                    |)

[//]: # (| docker-compose-cass.yml                | Cassandra                                                     |)

[//]: # (| docker-compose-cass-es.yml             | Cassandra and Elasticsearch                                   |)

[//]: # (| docker-compose-mysql.yml               | MySQL                                                         |)

[//]: # (| docker-compose-mysql-es.yml            | MySQL and Elasticsearch                                       |)

[//]: # (| docker-compose-postgres-opensearch.yml | PostgreSQL and OpenSearch                                     |)

[//]: # (| docker-compose-multirole.yml           | PostgreSQL and Elasticsearch with mult-role Server containers |)

[//]: # ()
[//]: # (### Using multi-role configuration)

[//]: # ()
[//]: # (First install the loki plugin &#40;this is one time operation&#41;)

[//]: # (```bash)

[//]: # (docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions)

[//]: # (```)

[//]: # ()
[//]: # (Start multi-role Server configuration:)

[//]: # (```)

[//]: # (docker compose -f docker-compose-multirole.yaml up)

[//]: # (```)

[//]: # ()
[//]: # (Some exposed endpoints:)

[//]: # (- http://localhost:8080 - Temporal Web UI)

[//]: # (- http://localhost:8085 - Grafana dashboards)

[//]: # (- http://localhost:9090 - Prometheus UI)

[//]: # (- http://localhost:9090/targets - Prometheus targets)

[//]: # (- http://localhost:8000/metrics - Server metrics)

[//]: # ()
[//]: # (### Using the web interface)

[//]: # ()
[//]: # (`docker-compose.yml` includes the Temporal Web UI.)

[//]: # ()
[//]: # (If you run command:)

[//]: # ()
[//]: # (```bash)

[//]: # (docker-compose up)

[//]: # (```)

[//]: # ()
[//]: # (You access the Temporal Web UI at http://localhost:8080.)

[//]: # ()
[//]: # (### Enabling metrics &#40;with Grafana and Prometheus&#41;)

[//]: # ()
[//]: # (We maintain two example docker-compose setups with server metrics enabled, and Prometheus and Grafana with [our Server and SDK dashboards]&#40;https://github.com/temporalio/dashboards&#41;:)

[//]: # ()
[//]: # (- https://github.com/tsurdilo/my-temporal-dockercompose)

[//]: # (- https://github.com/temporalio/background-checks)

[//]: # ()
[//]: # (### Use a custom image configuration)

[//]: # ()
[//]: # (If you want, you can even use a custom Docker image of the Temporal Server.)

[//]: # ()
[//]: # (Clone the main Temporal Server repo: [https://github.com/temporalio/temporal]&#40;https://github.com/temporalio/temporal&#41;:)

[//]: # ()
[//]: # (```bash)

[//]: # (git clone https://github.com/temporalio/temporal.git)

[//]: # (```)

[//]: # ()
[//]: # (In the following command, replace **<YOUR_TAG>** and **<YOUR_COMMIT>** to build the custom Docker image:)

[//]: # ()
[//]: # (```bash)

[//]: # (git checkout <YOUR_COMMIT>)

[//]: # (docker build . -t temporalio/auto-setup:<YOUR_TAG> --build-arg TARGET=auto-setup)

[//]: # (```)

[//]: # ()
[//]: # (Next, in the `docker-compose.yml` file, replace the `services.temporal.image` configuration value with **<YOUR_TAG>**.)

[//]: # ()
[//]: # (Then run the `docker-compose up` command:)

[//]: # ()
[//]: # (```bash)

[//]: # (docker-compose up)

[//]: # (```)

[//]: # ()
[//]: # (## Using Temporal docker images in production)

[//]: # ()
[//]: # (These docker-compose setups listed here do not use Temporal Server directly - they utilize [an `auto-setup` script you can read about here]&#40;https://docs.temporal.io/blog/auto-setup&#41;. You will want to familiarize yourself with this before you deploy to production.)

[//]: # ()
[//]: # (In a typical production setting, dependencies such as `cassandra` or `elasticsearch` are managed/started independently of the Temporal server. **You should use the `temporalio/server` image instead of `temporalio/auto-setup`.**)

[//]: # ()
[//]: # (To use the `temporalio/server` container in a production setting, use the following command:)

[//]: # ()
[//]: # (```plain)

[//]: # (docker run -e CASSANDRA_SEEDS=10.x.x.x                  -- csv of Cassandra server ipaddrs)

[//]: # (    -e KEYSPACE=<keyspace>                              -- Cassandra keyspace)

[//]: # (    -e VISIBILITY_KEYSPACE=<visibility_keyspace>        -- Cassandra visibility keyspace)

[//]: # (    -e SKIP_SCHEMA_SETUP=true                           -- do not setup Cassandra schema during startup)

[//]: # (    -e NUM_HISTORY_SHARDS=1024  \                       -- Number of history shards)

[//]: # (    -e SERVICES=history,matching \                      -- Spin-up only the provided services)

[//]: # (    -e LOG_LEVEL=debug,info \                           -- Logging level)

[//]: # (    -e DYNAMIC_CONFIG_FILE_PATH=config/foo.yaml         -- Dynamic config file to be watched)

[//]: # (    temporalio/server:<tag>)

[//]: # (```)

