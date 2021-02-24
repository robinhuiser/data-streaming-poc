# Metadata streaming PoC

Proof of concept for streaming bank [portfolio metadata](./reference/portfolio-01.json) using event sourcing.

## Goals

* Create / update portfolios in the system
* Track over time changes made to portfolios
* Compress aggregates over time in the event store (snapshots)

## Design

![design-solution-1](./imgs/design-solution-1.png)

Detailed information per module considering responsibility, configuration and deployment can be found below:

* [backoffice-client](./apps/backoffice-client/README.md)
* [discovery-service](./apps/discovery-service/README.md)
* [event-handler](./apps/event-handler/README.md)
* [event-store](./apps/event-store/README.md)
* [notification-worker](./apps/notification-worker/README.md)
* [portfolio](./apps/portfolio/README.md)
* [portfolio-service](./apps/portfolio-service/README.md)

## Build and runtime prerequisites

You need to have installed:

* Go (v1.15+)
* [Task](https://taskfile.dev/#/)
* [Benthos](https://www.benthos.dev/)
* [Docker](https://www.docker.com/products/docker-desktop) and `docker-compose`
* [Google Ko](https://github.com/google/ko)
* [NATS](https://nats.io/) - follow [these direction](./docs/nats-setup.md)
* [MongoDB](https://www.mongodb.com/) - follow [these direction](./docs/mongodb-setup.md)

## Start the middleware

~~~bash
# This will start NATS, MongoDB & Web-UIs
$ docker-compose up --detach --remove-orphans
~~~

## Spin up the microservices

WIP.

## Create and query portfolios

WIP.

## Compress aggregates and measure performance gain

WIP.

## Conclusion

WIP.