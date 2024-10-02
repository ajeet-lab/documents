## STEP 1: GET KAFKA OR DOWNLOAD KAFKA
<a href="https://kafka.apache.org/downloads" target="_blank">Download</a> the latest Kafka release and extract it:


* **For Linux/Ubuntu and Macos**
```
tar -xzf kafka_2.13-3.7.0.tgz //Rename 'kafka_2.13-3.7.0' to 'kafka'
cd kafka
```
* **For Windows**
```
Extract file manually and rename 'kafka_2.13-3.7.0' to 'kafka'
```

## Step 2: START THE KAFKA ENVIRONMENT
First of all, we should navigate to the Kafka directory and execute the commands below in the terminal.
### # Start the ZooKeeper service
* **For Linux/Ubuntu and Macos**
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```
* **For Windows**
```
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```


### # Start the Kafka broker service
* **For Linux/Ubuntu and Macos**
```
bin/kafka-server-start.sh config/server.properties
```
* **For Windows**
```
bin\windows\kafka-server-start.bat config/server.properties
```
Once all services have successfully launched, you will have a basic Kafka environment up and running, ready to be used

## STEP 3: CREATE A TOPIC TO STORE YOUR EVENTS
So, before you can write your first events, you must create a topic. Open another terminal session and execute the following command:

* **For Linux/Ubuntu and Macos**
```
bin/kafka-topics.sh --create --topic your-topic-name --bootstrap-server localhost:9092
```

* **For Windows**
```
bin\windows\kafka-topics.bat --create --topic your-topic-name --bootstrap-server localhost:9092
```

## STEP 4: WRITE SOME EVENTS INTO THE TOPIC
A Kafka client communicates with the Kafka brokers over the network for writing (or reading) events. Once received, the brokers store the events in a durable and fault-tolerant manner for as long as necessary, even indefinitely.

Execute the console producer client to write a few events into your topic. By default, each line you enter will produce a separate event in the topic.
* **For Linux/Ubuntu and Macos**
```
bin/kafka-console-producer.sh --topic your-topic-name --bootstrap-server localhost:9092
```

* **For Windows**
```
bin\windows\kafka-console-producer.bat --topic your-topic-name --bootstrap-server localhost:9092
```

## STEP 5: READ THE EVENTS
Open another terminal session and execute the console consumer client to read the events you just created:
* **For Linux/Ubuntu and Macos**
```
bin/kafka-console-consumer.sh --topic your-topic-name --from-beginning --bootstrap-server localhost:9092
```

* **For Windows**
```
bin\windows\kafka-console-consumer.bat --topic your-topic-name --from-beginning --bootstrap-server localhost:9092
```
You can stop the consumer client with Ctrl-C at any time.


# KAFKA INTEGRATION WITH SPRINGBOOT PROJECT
**GIT Code URL:** <a href="https://github.com/ajeet-lab/springboot-microservices/tree/master/KafkaIntegration" target="_blank">Click Here for Implementation code</a>
## For Produce service

Inside the Config package, we will create two java classes **(AppConstant and KafkaConfig)**.</br>

**AppConstant:** In this class, we will create constant variables and their respective values.</br>
**KafkaConfig:** In this class, We will create topics.

#### AppConstant.class
```
package com.apache.kafka.config;
public class AppConstant {
    public static final String LOCATION_TOPIC_NAME="location-update-topic";
}
```

#### KafkaConfig.class
```
@Configuration
public class KafkaConfig {
    @Bean
    public NewTopic topic(){
        return TopicBuilder.name(AppConstant.LOCATION_TOPIC_NAME).build();
    }
}
```


#### KafkaService.class
We will create a **KafkaService** class in service package to produce messages.
```
@Service
public class KafkaService {
    private Logger log = LoggerFactory.getLogger(KafkaService.class);

    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    public boolean updateLocation(String location){
        this.kafkaTemplate.send(AppConstant.LOCATION_TOPIC_NAME, location);
        this.log.info("Message produced");
        return true;
    }
}
```

#### KafkaController.class
We will create a **KafkaController** class in the controller package. Inside this class, we will define REST APIs using the **@RestController** annotation to send messages.
```
@RestController
@RequestMapping("/location")
public class KafkaController {

    @Autowired
    private KafkaService kafkaService;

    @PostMapping("/update")
    public ResponseEntity<?> updateLocation(){
        this.kafkaService.updateLocation("("+Math.round(Math.random() * 100)+", "+Math.round(Math.random() * 100)+")");
        return new ResponseEntity<>(Map.of("message","Message updated"), HttpStatus.OK);
    }
}
```

### application.properties for producer
```
# Kafka Producer Configuration
spring.kafka.producer.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```
In this example:

* **spring.kafka.producer.bootstrap-servers** specifies the Kafka brokers to which the producer will connect.
* **spring.kafka.producer.key-serializer** specifies the serializer for keys.
* **spring.kafka.producer.value-serializer** specifies the serializer for values.


## For Consume service
Inside the Config package, we will create two java classes **(AppConstant and KafkaConfig)**.</br>

**AppConstant:** In this class, we will create constant variables and their respective values.</br>
**KafkaConfig:** In this class, We will listen topics.

#### AppConstant.class
```
package com.apache.kafka.config;
public class AppConstant {
    public static final String LOCATION_TOPIC_NAME="location-update-topic";
    public static final String LOCATION_GROUP_ID="group-1";
}
```
#### KafkaConfig.class
```
package com.apache.kafka.config;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Configuration;
import org.springframework.kafka.annotation.KafkaListener;

@Configuration
public class KafkaConfig {
    private Logger log = LoggerFactory.getLogger(KafkaConfig.class);
    @KafkaListener(topics = AppConstant.LOCATION_TOPIC_NAME, groupId = AppConstant.LOCATION_GROUP_ID)
    public void updateLocation(String value){
        log.info(value);
    }
}
```
### application.properties for consumer
```
# Kafka Consumer Configuration
spring.kafka.consumer.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
In this configuration:

* **spring.kafka.consumer.bootstrap-servers** specifies the Kafka brokers to which the consumer will connect.
* **spring.kafka.consumer.group-id** specifies the consumer group id.
* **spring.kafka.consumer.auto-offset-reset** specifies the offset reset behavior.
* **spring.kafka.consumer.key-deserializer** specifies the deserializer for keys.
* **spring.kafka.consumer.value-deserializer** specifies the deserializer for values.



# KAFKA INTEGRATION WITH SPRINGBOOT-CAMEL PROJECT
**GIT Code URL:** <a href="https://github.com/ajeet-lab/springboot-microservices/tree/master/KafkaIntegration" target="_blank">Click Here for Implementation code</a>

To implement **all Kafka options** in a **Spring Boot-Camel project**, you need to configure multiple options such as Kafka producers, consumers, serialization, deserialization, topics, error handling, and Kafka-specific settings like partitions, offsets, retries, etc.

Here’s an in-depth breakdown on how to handle all these configurations in your **Spring Boot-Camel** project.

### 1. Kafka Options Overview
Apache Camel provides a Kafka component with many options such as:

- **brokers**: The Kafka brokers to connect to.
- **groupId**: Group ID for Kafka consumers.
- **topics**: Kafka topics to consume from or produce to.
- **autoOffsetReset**: How to handle offsets when no previous offset is available.
- **keySerializer / keyDeserializer**: Serializer and deserializer for keys.
- **valueSerializer / valueDeserializer**: Serializer and deserializer for message values.
- **acknowledgment**: Configure producer acknowledgment.
- **partition**: Partition-specific routing.

Let’s cover each of these areas in a Camel Spring Boot project:

### Step 1: Add Required Dependencies

Make sure you have the following dependencies in your `pom.xml` file to integrate **Spring Boot**, **Apache Camel**, and **Kafka**:

```
<dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-kafka-starter</artifactId>
</dependency>

<!-- Kafka client -->
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
</dependency>
```

### Step 2: Kafka Configuration

Configure Kafka properties in the `application.properties` or `application.yml` file to provide the Kafka broker, consumer, producer, and other details.

**For `application.properties`:**

```properties
# Kafka settings
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

### Step 3: Simple Camel Routes for Kafka
```
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.dataformat.JsonLibrary;
import org.apache.camel.model.rest.RestBindingMode;
import org.springframework.stereotype.Component;

@Component
public class KafkaIntegrationRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {
        restConfiguration()
                .component("jetty")
                .host("localhost").port("9902")
                .bindingMode(RestBindingMode.auto);

        rest("/rest/api/v1").post("/kafka/push-into-kafka").to("direct:push-into-kafka");

      // Producer
      from("direct:push-into-kafka")
              .log("Hit received at ${date:now} and body -> ${body}")
              .marshal().json(JsonLibrary.Jackson)
              .to("kafka:my-topic?brokers=localhost:9092")
              .setBody(simple("{\"message\":\"data pushed into kafka queue\"}"))
              .unmarshal().json(JsonLibrary.Jackson);
        
        // Consumer
        from("kafka:my-topic?brokers=localhost:9092&groupId=demo-group")
                .log("Message received from Kafka: ${body}");
    }
}

```

### Step 4: Camel Routes for Kafka with All Options

You can use different Kafka options in your Camel route configuration. Here are examples of how to apply various Kafka options:

#### **Kafka Consumer with Additional Options**
In this example, the route listens to a Kafka topic with options for offsets, partitions, and error handling.

```
import org.apache.camel.builder.RouteBuilder;
import org.springframework.stereotype.Component;

@Component
public class KafkaConsumerRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("kafka:my-topic?brokers=localhost:9092&groupId=my-group"
             + "&autoOffsetReset=earliest" // Reset offset to earliest if no offset is found
             + "&consumersCount=3"         // Number of consumers
             + "&pollTimeoutMs=1000"       // Timeout for polling Kafka
             + "&seekTo=beginning"         // Start from beginning for a new group
        )
        .log("Received message from Kafka: ${body}")
        .onException(Exception.class)
            .log("Error processing Kafka message: ${exception.message}")
            .handled(true)
        .end();
    }
}
```

#### **Kafka Producer with Additional Options**
The following example configures a producer route with key/value serializers, partition-specific sending, acknowledgment, and more.

```java
@Component
public class KafkaProducerRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("timer://myTimer?fixedRate=true&period=5000") // Trigger every 5 seconds
            .setBody(simple("Message generated at ${header.firedTime}"))
            .setHeader("key", simple("uniqueKey"))
            .to("kafka:my-topic?brokers=localhost:9092"
                + "&keySerializer=org.apache.kafka.common.serialization.StringSerializer"
                + "&valueSerializer=org.apache.kafka.common.serialization.StringSerializer"
                + "&partitioner=org.apache.kafka.clients.producer.internals.DefaultPartitioner"  // Partition strategy
                + "&requestRequiredAcks=1" // Acknowledge once the message is written
                + "&retries=3"  // Retry 3 times if message fails to send
            );
    }
}
```

### Step 5: Advanced Kafka Options

#### 1. **Partitioning**
Kafka allows you to target specific partitions based on a key. If you want to produce to a specific partition:

```
@Component
public class KafkaPartitionProducerRoute extends RouteBuilder {
    
    @Override
    public void configure() throws Exception {
        from("direct:start")
            .setBody(simple("Message to partition 1"))
            .setHeader("CamelKafkaPartitionKey", constant(1)) // Sending to partition 1
            .to("kafka:my-topic?brokers=localhost:9092");
    }
}
```

#### 2. **Serialization and Deserialization**
You can use custom serializers and deserializers for key and value. Define your own serializer class like this:

```
spring.kafka.producer.key-serializer=com.mycompany.CustomKeySerializer
spring.kafka.producer.value-serializer=com.mycompany.CustomValueSerializer
```

Similarly, for consumers, use custom deserializers.

#### 3. **Consumer Seek**
To seek to a particular offset:

```
from("kafka:my-topic?brokers=localhost:9092"
     + "&groupId=my-group&seekTo=42")
    .log("Message from Kafka at offset 42: ${body}");
```

#### 4. **Retry and Backoff**
You can add retry strategies for both producer and consumer sides in Kafka.

```
from("kafka:my-topic?brokers=localhost:9092&retries=5&retryBackoffMs=1000")
    .log("Message from Kafka with retry logic: ${body}");
```

### Step 6: Error Handling and Dead Letter Topic

You can configure **dead-letter topics** (DLT) for messages that fail to process after retries.

```
@Component
public class KafkaErrorHandlingRoute extends RouteBuilder {

    @Override
    public void configure() throws Exception {
        from("kafka:my-topic?brokers=localhost:9092&groupId=my-group"
             + "&autoCommitEnable=false" // Disable auto-commit
        )
        .onException(Exception.class)
            .log("Error processing message, sending to dead-letter topic")
            .to("kafka:my-dead-letter-topic?brokers=localhost:9092")
            .handled(true)
        .end()
        .log("Processing message: ${body}");
    }
}
```

### Step 7: Monitoring and Metrics
For monitoring Kafka in Camel, you can use **Camel Metrics** or integrate **Spring Actuator** with custom Kafka metrics to track Kafka consumer and producer metrics like message lag, throughput, etc.

```
<dependency>
    <groupId>org.apache.camel.springboot</groupId>
    <artifactId>camel-metrics-starter</artifactId>
</dependency>
```

### Summary of Kafka Options in Camel
1. **brokers**: Kafka broker addresses.
2. **groupId**: Consumer group ID.
3. **autoOffsetReset**: How offsets should be managed.
4. **consumersCount**: Number of consumer threads.
5. **keySerializer/valueSerializer**: Custom serializers.
6. **partitioner**: Partitioning logic.
7. **seekTo**: Offset management (e.g., seek to beginning or specific offsets).
8. **retries**: Number of retries for sending/receiving messages.
9. **acknowledgment**: Configuring Kafka acknowledgment.
10. **pollTimeoutMs**: Polling timeout.

By configuring these options, you can fine-tune your Kafka consumer and producer behavior in a **Spring Boot-Camel** project.
=======
>>>>>>> 49435a652837d863a107db63173f01ca4fea0a58
