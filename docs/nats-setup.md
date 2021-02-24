# Setup NATS

How to setup NATS using Docker and local CLI to interact.

## Install & run server & Web-UI

Both NATS and Web-UI are started with the `docker-compose.yml`.

* Server is available at `nats://localhost:4222`
* UI is available at: [http://localhost:3301](http://localhost:3301)
* Monitoring is available at [http://localhost:8222/](http://localhost:8222/)

> **Note**: the monitoring endpoint allows you to to retrieve information considering:
>   * variables
>   * connections
>   * routes
>   * gateways
>   * leafs
>   * subscriptions

You might want to test your **cluster** with a simple sender / subscriber:

~~~bash
# Startup a local container in same network
$ docker run --network nats --rm -it synadia/nats-box
           _             _               
_ __   __ _| |_ ___      | |__   _____  __
| '_ \ / _` | __/ __|_____| '_ \ / _ \ \/ /
| | | | (_| | |_\__ \_____| |_) | (_) >  < 
|_| |_|\__,_|\__|___/     |_.__/ \___/_/\_\
                                          
nats-box v0.4.0
# Register a listener on primary node
$ nats-sub -s nats://nats:4222 hello &
Listening on [hello]

# Send message to topic on 2nd node
$ nats-pub -s "nats://nats-1:4222" hello first
[#1] Received on [hello]: 'first'

# Send message to topic on 3rd node
$ nats-pub -s "nats://nats-2:4222" hello second
[#2] Received on [hello]: 'second'
~~~

## NATS CLI

First we need to install & configure the CLI to connect to our newly started cluster.

~~~bash
# Install
$ brew tap nats-io/nats-tools
$ brew install nats-io/nats-tools/nats

# Add context to server
$ nats context add local --description "My Docker Cluster"

# Check the context selected
$ nats context ls
Known contexts:

   local           My Docker Cluster
~~~

### Publish - Subscribe

Now, you can publish and subscribe:

~~~bash
# Subscribe (terminal 1)
$ nats sub cli.demo 
12:30:25 Subscribing on cli.demo

# Send a message (terminal 2)
$ nats pub cli.demo "hello world"

# You can use Go templates :-)
$ nats pub cli.demo "message {{.TimeStamp}}" --count=10
~~~

### Request - Reply

NATS supports a RPC mechanism where a service received Requests and replies with data in response.

~~~bash
# Start the reply (terminal 1)
$ nats reply 'cli.weather.>' "Weather Service"
12:43:28 Listening on "cli.weather.>" in group "NATS-RPLY-22"

# Perform a request (terminal 2)
$ nats request cli.weather.london '' --raw
12:46:34 Sending request on "cli.weather.london"
12:46:35 Received on "_INBOX.BJoZpwsshQM5cKUj8KAkT6.HF9jslpP" rtt 404.76854ms
Weather Service
~~~

To make this a bit more interesting we can interact with the wttr.in web service:

~~~bash
# Execute a curl command and return the output as reply
$ nats reply 'cli.weather.>' --command "curl -s wttr.in/{{2}}?format=3"
12:47:03 Listening on "cli.weather.>" in group "NATS-RPLY-22"

# We can perform the same request again:
$ nats request "cli.weather.{london,newyork}" '' --raw
london: 🌦 +7°C
newyork: ☀️ +2°C
~~~

## NATS Web-UI

1. In the [NATS Web UI](http://localhost:3301), add a server with Hostname `nats` - leave ports default
2. To trace messages in the server:
   * In the UI, create on server `nats` add the subject `cli.demo` under **Server Subject Hierarchy** as shown below:

      ~~~text
      cli
          demo
      ~~~

   * Create a client, connected to server `nats`, click on "Subscribe"
   * Assure the `cli.demo` topic is selected under **Server Subject Hierarchy**
   * Publish a message as shown in the CLI section.

## Benthos

Benthos is awesome. Really.

~~~bash
# Start and listen to the subject cli.demo
$ benthos -c ./benthos/nats-incoming-cli-demo.yaml
~~~

Next, publish a message as shown in the CLI section.