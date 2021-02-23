# Data streaming PoC

PoC for streaming financial data into a query-able event-store.

## Prerequisites

You need to have installed:

* Go (v1.15+)
* [Task](https://taskfile.dev/#/)
* [Benthos](https://www.benthos.dev/)
* [NATS](https://nats.io/) - follow [these direction](./docs/nats-setup.md) to configure, test & monitor
* [Docker](https://www.docker.com/products/docker-desktop) and `docker-compose`

## Start the middleware

~~~bash
# This will start NATS & Web-UI, ...
$ docker-compose up --detach --remove-orphans
~~~
