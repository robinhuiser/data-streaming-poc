# Data streaming PoC

PoC of streaming financial data into a query-able event-store

1. Start nats and ui using `docker-compose up --detach --remove-orphans`
2. In [nats web UI](http://localhost:8080), connect to server `nats` - leave ports default
3. Test with a simple sender / subscriber:

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

4. To trace messages in the server:
   * In the UI, create on server `nats` add a subject `hello` under **Server Subject Hierarchy**
   * Create a client, connected to server `nats`, click on "Subscribe"
   * Assure the `hello` topic is selected under **Server Subject Hierarchy**
   * Send a message as shown in previous step

