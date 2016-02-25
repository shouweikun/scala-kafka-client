# Scala wrapper for Apache Kafka's Java client library 0.9.x.x

This project comprises a few helper modules for operating the [Kafka Java Client Driver](https://kafka.apache.org/090/javadoc/index.html) in a Scala codebase.

## Status
This project is currently a work-in-progress and not yet ready for proper use!

## Modules

The following modules are provided
 - **scaka-kafka-client.** A thin wrapper around the Java client API, providing some helpers for configuring the client.
 - **scala-kafka-client-akka.** Provides a Consumer Actor that can be convenient when developing applications with Akka.  The Akka consumer has buffering capabilities to increase throughput as well as some helpers to provide easy configuration.
 - **scaka-kafka-client-testkit.** Supports integration testing of Kafka client code by providing helpers that can start an in-process Kafka and Zookeeper server.

### Latest release
To include, add the following to the build.sbt

    resolvers += Resolver.bintrayRepo("simonsouter", "maven")

    For basic kafka client support:
    libraryDependencies += "net.cakesolutions" %% "scala-kafka-client" % "0.5.0"

    For kafka integration test support:
    libraryDependencies += "net.cakesolutions" %% "scala-kafka-client-tesktkit" % "0.5.0" % "test"

    For akka support:
    libraryDependencies += "net.cakesolutions" %% "scala-kafka-client-akka" % "0.5.0"

## Version Compatibility

 scala-kafka-client | Kafka Java Driver
 ------------------ | -----------------
 0.5 | 0.9.0.1
 0.4  | 0.9.0.0

## scaka-kafka-client
The scala-kafka-client is a thin wrapper around the Java client API, providing some helpers for configuring the client.

### Producer

### Consumer
The [KafkaConsumer](https://kafka.apache.org/090/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html) is typically setup by creating a Java Properties with required consumer configuration.
The Scala wrapper provides some convenience functions for creating the consumer configuration either directly in code, via Typesafe config or a combination of both.

#### Direct Configuration
The cakesolutions.kafka.KafkaConsumer.Conf case class models the configuration required for the KafkaConsumer and can be constructed and passed to the cakesolutions.kafka.KafkaConsumer factory method to produce a configured [KafkaConsumer](https://kafka.apache.org/090/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html)
Key and Value Deserialisers are also required as documented in the Kafka client docs.

```
val consumer = KafkaConsumer(Conf(new StringDeserializer(), new StringDeserializer(), bootstrapServers = "localhost:8082"))
```

The conf class provides additional properties with defaults.

#### Typesafe Configuration
A cakesolutions.kafka.KafkaConsumer.Conf can also take a [Typesafe Config file](https://github.com/typesafehub/config):

```
val conf = ConfigFactory.parseString(
        s"""
           | bootstrap.servers = "localhost:8082",
           | group.id = "group1"
        """.stripMargin)

val conf = Conf(new StringDeserializer(), new StringDeserializer(), groupId = "group1", enableAutoCommit = false).withConf(conf)
val consumer = KafkaConsumer(conf)
```

#### Additional Config Options
A combination of static properties and Typesafe config properties is also an option:

application.conf:

```
{
      bootstrap.servers = "localhost:8082"
}

val conf = ConfigFactory.load

val consumer = KafkaConsumer(Conf(new StringDeserializer(), new StringDeserializer(), conf))
```

## scala-kafka-client-akka
The Akka module provides a Consumer Actor that can be convenient when developing applications with Akka.  The Akka consumer has buffering capabilities to increase throughput as well as some helpers to provide easy configuration.

## scaka-kafka-client-testkit
The scala-kafka-client-tesktkit supports integration testing of Kafka client code by providing helpers that can start an in-process Kafka and Zookeeper server. 

## License
    
 Copyright 2016, Cake Solutions.
    
 Licensed under the MIT License
