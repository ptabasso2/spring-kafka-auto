# spring-kafka-auto

[![Sonarcloud Status](https://sonarcloud.io/api/project_badges/measure?project=com.codenotfound%3Aspring-kafka-hello-world&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.codenotfound%3Aspring-kafka-hello-world)


A detailed step-by-step tutorial on how to implement an Apache Kafka Consumer and Producer using Spring Kafka and Spring Boot.
<br>In this tutorial we will also enable distributed tracing using the Datadog java agent (automatic instrumentation) 

### _Preliminary tasks and first time steps_


**Install Kafka on mac OSX (High Sierra)**

```
COMP10619:Kafka pejman.tabassomi$ brew install kafka
```

**Start Zookeeper**

```
COMP10619:Kafka pejman.tabassomi$ zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```

**Start kafka server**

```
COMP10619:Kafka pejman.tabassomi$ kafka-server-start /usr/local/etc/kafka/server.properties
```

**Create a Topic (First time)**

```
COMP10619:Kafka pejman.tabassomi$ kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic users
```


### _Administration tasks_

**Start Zookeeper**

```
COMP10619:Kafka pejman.tabassomi$ zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```

**Start kafka server**

```
COMP10619:Kafka pejman.tabassomi$ kafka-server-start /usr/local/etc/kafka/server.properties
```

**List all Topics**

```
COMP10619:Kafka pejman.tabassomi$ kafka-topics --list --zookeeper localhost:2181
```

**To check if data is landing in kafka**

```
COMP10619:Kafka pejman.tabassomi$ kafka-console-consumer --bootstrap-server localhost:9092 --topic users --from-beginning
```


### _Clone the repository and cd into it_

```
COMP10619:Kafka pejman.tabassomi$ git clone https://https://github.com/ptabasso2/spring-kafka-auto.git
COMP10619:Kafka pejman.tabassomi$ cd spring-kafka-auto
```

### _Download the java tracer_

```
COMP10619:spring-kafka-auto pejman.tabassomi$ wget -O dd-java-agent.jar 'https://repository.sonatype.org/service/local/artifact/maven/redirect?r=central-proxy&g=com.datadoghq&a=dd-java-agent&v=LATEST'
```


### _Spin up the Datadog Agent (Provide your API key  to the  belown command_


```
COMP10619:spring-kafka-auto pejman.tabassomi$ docker run -d --name datadog_agent -v /var/run/docker.sock:/var/run/docker.sock:ro -v /proc/:/host/proc/:ro -v /sys/fs/cgroup/:/host/sys/fs/cgroup:ro -p 127.0.0.1:8126:8126/tcp -e DD_API_KEY=<Your API key> -e DD_APM_ENABLED=true datadog/agent:latest
```


### _Start the spring boot app with the Datadog Java agent_

```
COMP10619:spring-kafka-auto pejman.tabassomi$ java -javaagent:./dd-java-agent.jar -Ddd.service.name=springkafkaauto -Ddd.trace.http.client.split-by-domain=true  -Ddd.trace.global.tags=env:datadoghq.com -jar build/libs/spring-kafka-auto.jar --server.port=8081
```


### _Run the tests_

Open a new terminal window and run the following curl command:

```
COMP10619:Kafka pejman.tabassomi$ curl localhost:8081/test
```
