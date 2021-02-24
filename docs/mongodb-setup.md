# Setup MongoDB

How to setup MongoDB using Docker and local CLI to interact.

## Install & run server & Web-UI

Both MongoDB and Web-UI are started within the project [docker-compose.yml](../docker-compose.yml).

* Server is available at `mongodb://localhost:27017`
* UI is available at: [http://localhost:3300](http://localhost:3300)

## MongoDB CLI

First we need to install & configure the CLI to connect to our newly started cluster.

~~~bash
# Install
$ brew tap mongodb/brew
$ brew install mongodb-community@4.4

# Connect to the server
$ mongo --host localhost --port 27017

# List databases
> db
test

# Exit
> exit
~~~
