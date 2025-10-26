# iris-wazuh
This repository deploys Wazuh with DFIR IRIS integration 

At least... that's the goal

# Deploy Wazuh Docker in single node configuration with DFIR IRIS integration.

This deployment is defined in the `docker-compose.yml` file with one Wazuh manager containers, one Wazuh indexer containers, one Wazuh dashboardcontainer and DFIR IRIS.

Iris is split on 5 Docker services, each with a different role.

- ``app``: The core, including web server, DB management, module management etc.
- ``db``: A PostgresSQL database
- ``RabbitMQ``: A RabbitMQ engine to handle jobs queuing and processing
- ``worker``: Jobs handler relying on RabbitMQ
- ``nginx``: A NGINX reverse proxy

It can be deployed by following these steps: 

1) Increase max_map_count on your host (Linux). This command must be run with root permissions:
```
$ sysctl -w vm.max_map_count=262144
```

2)  Clone the iris-web repository

``` bash
git clone https://github.com/Cyber-MKO/iris-wazuh2.git
cd iris-wazuh2
```

3) Run the certificate creation script:
```
$ docker compose -f generate-indexer-certs.yml run --rm generator
```
4) Start the environment with docker compose:

- In the foregroud:
```
$ docker compose up
```
- In the background:
```
$ docker compose up -d
```

The environment takes about 1 minute to get up (depending on your Docker host) for the first time since Wazuh Indexer must be started for the first time and the indexes and index patterns must be generated.