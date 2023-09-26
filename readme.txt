Here is a small implementation of kafka queuing using golang.

1. initialise container of kafka with wget installed on it using dockerfile.
2. install kafka and jre, java jdk using wget
3. verify installation of java
4. extract kafka using command 
    tar -xvzf kafka_2.13-3.0.0.tgz // replace your version
5. run the zookeeper using below command inside the extracted kafka folder
    bin/zookeeper-server-start.sh config/zookeeper.properties
6. if u face error in above command saying 
    Classpath is empty. Please build the project first e.g. by running './gradlew jar -PscalaVersion=2.13.10' , follow below NOTE

7. to start kafka brokers, refer to file /config/server.properties
8. make a duplicate for each broker u want to create // follow cp command
9. Follow NOTE2 for each broker u create // the idea is to have different config file for each broker
10. make logs directory for each broker you created, same way you have mentioned in the config file
11. start each broker by following command
    bin/kafka-server-start.sh config/server.1.properties
12. to create a topic for data that it belongs to, follow below command
    bin/kafka-topics.sh --create --topic my-kafka-topic --bootstrap-server localhost:9093 --partitions 3 --replication-factor 2
    partitions are how many brokers information is split between, replication factor is how many copies of your data you want 
13. to list available topics
    bin/kafka-topics.sh --list --bootstrap-server localhost:9093
14. create a network and add this kafka container to it. Make note of the ipv4 address of container inside the instance. command below
    docker network inspect network_name
15. replace the ipv4 address in the golang code. build the image using the docker file
16. run the image using below command
    docker run --rm --net network_name --name name_of_service_to_be_inside_network -d image_name 




NOTE: run this command as a prerequisite to running gradle
        ./gradlew jar -PscalaVersion=2.13.10

NOTE2:  broker.id=1
        listeners=PLAINTEXT://:9093
        log.dirs=/tmp/kafka-logs1