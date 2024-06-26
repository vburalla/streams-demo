# Streams Demo Project

This project defines how to use Kafka Streams to resolve an easy use case like counting orders by product id.

We'll use Kafka Streams, a client library for building applications, where the input and output are stored in Kafka clusters. It combines the ease of implementing applications in languages like Java, with the benefits of Kafka technology.

If you want to learn more about Kafka Streams and Processor API, please visit its documentation: https://kafka.apache.org/documentation/streams/developer-guide/dsl-api.html .

Original presentation related to this project can be found in: `./presentation/Presentación KSTs v2.pptx`

##### Requeriments

- Postman
- JDK 17+

## The use case

An online store is receiving orders with some order data: orderId, productId and order amount.
We need to count number of orders received by productId.

## The solution: streaming

The solution includes:

- Count orders by product id in real time and publish result to an output topic (orders-count)
- Expose results in real time via API.

We can represent the high-level topology with as:

![](/docs/demo_topology.png)

## How is structured this repo?

In this repo you'll find these folders inside the Spring Boot application folder:

- **docker**: it contains the *docker-compose* file to launch a demo environment to really understand how all the components works and fraud cases are detected. The platform is composed of:
    - Zookeeper
    - Kafka
    - Schema registry
    - AKHQ
    - Topics generartor
    - Rest proxy
- **docs**: this folder contains documentation about solution topology
    - *demo_topology.png*: High level representation of process
    - *STREAMS_DEMO_TOPOLOGY.png*: Complete topology generated by Kafka streams library
  - **postman**: this folder contains postman collection to interact with Kafka via rest-proxy. With this collection we can:
    - Create orders to inject new orders in input topic
    - Access in real-time to product orders count
    - Access in real-time to orders count by productId

## Executing the demo

Executing the demo is very easy.

The first step is going to docker folder and execute

```bash
docker-compose up -d
```

When the script ends, we'll see something like this:

```bash
[+] Running 6/6                                                                                                                                                                                                                                
 ⠿ Container zookeeper_container                Started                                                                                                
 ⠿ Container broker_container                   Started                                                                                                
 ⠿ Container schema-registry_container          Started                                                                                                
 ⠿ Container init                               Started                                                                                                
 ⠿ Container rest-proxy_container               Started                                                                                                
 ⠿ Container akhq_container                     Started                                                                                                
````

Once all containers are up, we can interact with Kafka in some ways:

  - Producing messages in input topic via rest-proxy (using provided postman collection)
  - Accessing AKHQ where we can:
    - manage topics (create, update, delete, change number of partitions, access messages, etc.)
    - manage consumer groups (modify offsets, check current offset, etc.)
    - manage schemas
    - etc.

When executing Spring Boot application, we can:

  - Check updates in console every time we create new product order
  - Call endpoints with postman to retrieve current order counts

In tests execution have used TopologyTestDriver, to test our topology.
In the setUp method we have added a method called describe() that executes the describe Topology method. 
This method provides us a text description of the complete generated topology.
With this description, we can generate the topology visualization thanks to [topology visualizer](https://zz85.github.io/kafka-streams-viz/)

