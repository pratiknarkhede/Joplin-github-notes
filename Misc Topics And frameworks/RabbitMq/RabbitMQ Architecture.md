## RabbitMQ Architecture

At its core, RabbitMQ consists of several key components working together:

### 1\. Core Server (Broker)

The RabbitMQ server is written in Erlang, which gives it excellent concurrency support and fault tolerance. The broker:

- Accepts and processes AMQP commands
- Manages connections and channels
- Handles exchange-to-queue routing
- Maintains the message store
- Manages clustering and high availability

### 2\. vHosts (Virtual Hosts)

Virtual hosts provide logical separation within a broker, similar to virtual hosts in web servers. Each vHost:

- Has its own exchanges, queues, and bindings
- Has separate user permissions
- Provides resource isolation
- Allows a single RabbitMQ installation to serve multiple applications

### 3\. Node Architecture

In a clustered environment, RabbitMQ uses a distributed Erlang architecture where:

- Every node connects to every other node (full mesh)
- Nodes share user, vHost, queue, exchange, and binding definitions
- Queue contents can be mirrored across nodes for high availability
- Node communications use Erlang's distribution protocol

### 4\. Storage Subsystems

RabbitMQ maintains several types of data:

- **Metadata**: Exchange, queue definitions, bindings (stored in Mnesia database)
- **Messages**: Actual message content (stored in message store)
- **Indices**: References to messages (for queue ordering)

Messages can be stored in:

- **Memory**: Faster but volatile
- **Disk**: Slower but persistent
- **Both**: For durability with caching

### 5\. Connection Architecture

Client applications connect to RabbitMQ using:

- **Connections**: Long-lived TCP connections
- **Channels**: Lightweight virtual connections that share a single TCP connection

This multiplexing approach reduces operating system overhead while providing logical separation.