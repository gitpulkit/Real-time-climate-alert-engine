--Real-Time Climate Alert Engine

A distributed, real-time weather alert system built using Apache Kafka and AWS services. The system ingests live weather data, processes it through a scalable event-driven pipeline, and delivers personalized alerts to users based on their subscriptions.

--Overview

This project implements a real-time data streaming pipeline that fetches weather data from an external API, processes it using Kafka, and delivers notifications to users through AWS services.

The system demonstrates strong fundamentals in distributed systems, real-time processing, and scalable architecture design.

--Architecture

The system follows a modular, distributed design:

Weather data is fetched from an external API
A Kafka producer publishes data to topics
Kafka brokers handle message storage and distribution
Consumers process events and trigger notifications
User subscriptions are stored in DynamoDB
Alerts are delivered via AWS SNS

--Key Components

Kafka Producer
Fetches real-time weather data and publishes it to topic-specific streams such as weather_sf and weather_ny.

Kafka Broker Layer
Handles message partitioning, replication, and enables horizontal scalability.

Zookeeper
Coordinates Kafka brokers and manages cluster state.

Consumer Application
Consumes messages from Kafka, processes events, queries DynamoDB for user subscriptions, and triggers alerts.

DynamoDB
Stores user-topic mappings for personalized alert delivery.

AWS SNS
Sends notifications via email, SMS, or push.

--Data Flow
Weather data is fetched from an external API
Producer publishes data to Kafka topics
Kafka brokers distribute messages across partitions
Consumers read and process events
Subscriptions are queried from DynamoDB
Alerts are sent via SNS


--Distributed Systems Concepts
Event-driven architecture
Publish-subscribe model
Horizontal scaling using Kafka partitions
Leader election for coordination
Gossip protocol for state propagation
Fault tolerance through decoupled services


--Setup Instructions
Launch EC2 Instance

Create an EC2 instance and configure security groups to allow:

SSH (port 22)
Kafka (port 9092)
Zookeeper (port 2181)
SSH into EC2

ssh -i "EC2_for_Kafka.pem" ec2-user@<public-ip>
chmod 400 EC2_for_Kafka.pem

Install Kafka

wget https://dlcdn.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz

tar -xzf kafka_2.13-3.9.0.tgz
sudo mv kafka_2.13-3.9.0 /opt/kafka
cd /opt/kafka

Start Zookeeper and Kafka

/opt/kafka/bin/zookeeper-server-start.sh -daemon /opt/kafka/config/zookeeper.properties
/opt/kafka/bin/kafka-server-start.sh -daemon /opt/kafka/config/server.properties

Verify:
ps aux | grep zookeeper
ps aux | grep kafka

Create Topics

/opt/kafka/bin/kafka-topics.sh --create --topic weather_sf --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1
/opt/kafka/bin/kafka-topics.sh --create --topic weather_ny --bootstrap-server localhost:9092 --partitions 3 --replication-factor 1

Verify:
/opt/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092

Setup Jupyter for Producer/Consumer

sudo yum install -y python3-pip
pip3 install jupyter

Run:
nohup jupyter notebook --ip=0.0.0.0 --no-browser --allow-root > jupyter.log 2>&1 &
tail -f jupyter.log

Open the generated URL in your browser using your EC2 public IP.
Ensure port 8889 is open in the EC2 security group.

--Scalability and Design Considerations

Kafka topics are partitioned for parallel consumption
Broker layer can scale horizontally using auto scaling groups
Consumers scale independently based on load
Decoupled architecture enables independent service evolution

--Future Improvements
Real-time anomaly detection using streaming ML models
Integration with stream processing frameworks such as Apache Flink or Spark
Multi-region deployment for higher availability
Monitoring dashboard for real-time analytics

--Takeaway

This project demonstrates how to design and build a production-style real-time data pipeline using distributed systems principles. It highlights the ability to handle streaming data, scale horizontally, and deliver reliable event-driven applications.
