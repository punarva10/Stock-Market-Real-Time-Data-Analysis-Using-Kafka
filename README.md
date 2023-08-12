# Stock-Market-Real-Time-Data-Analysis-Using-Kafka

## Architecture: <br />
![removed](https://github.com/punarva10/Stock-Market-Real-Time-Data-Analysis-Using-Kafka/assets/80712306/154d2ab1-fe3f-4d1e-b792-ab76081dbc05)


## Steps to run:
1. Start an EC2 instance, create a new key pair, and download the pem key into a folder
2. Open 4 terminals and navigate into that folder in all 4.
3. ssh into the EC2 instance in all 4 terminals (Command found in AWS under {instance_name} -> SSH Client -> Connect ) <br />
```
   i. chmod 400 kafka-stock-market-project-key.pem <br />
   ii. ssh -i "{key-pair-name}.pem" ec2-user@ec2-{ip_address}.compute-1.amazonaws.com
```
4. Download kafka into the Ec2 Instance in one of the terminals <br />
```
   i. go to https://kafka.apache.org/downloads <br />
   ii. Right-click on the latest version of Kafka and copy the link address <br />
   iii. wget {copied address} <br />
   iv. ls (shows you the compressed kafka tgz folder inside the instance) <br />
   v. tar -xvf kafka_{version}.tgz
```
5. Download Java onto the EC2 instance <br />
```
   i. sudo yum search all  java-1.8.0 <br />
   ii. sudo yum install java-1.8.0-amazon-corretto.x86_64 (not the devel one) <br />
   iii. java -version (to check version number)
```
6. cd kafka_{version} (do ls to see kafka version) (do in all 4 terminals)
7. The kafka server by default points to a private server, change server.properties so that it can run on public IP <br />
```
   i. sudo nano config/server.properties
```
   ii. Uncomment advertised.listeners and change ip address to the public ip address of the instance (can be found in AWS under {instance_name} -> Public IPV4 Address) 
```
   advertised.listeners=PLAINTEXT://{public_ip}:9092
```
8. Change the security of the EC2 instance by adding an inbound rule to listen to our local machine <br />
    i. In AWS, go to {instance_name} -> Security -> security groups link -> Edit Inbound rules -> Add rule : Type = All traffic, Source = Anywhere - IPV4
9. Terminal 1: Start Zoo-keeper <br />
```
   i. bin/zookeeper-server-start.sh config/zookeeper.properties
```
10. Terminal 2: Start Kafka server <br />
```
   i. export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M" <br />
   ii. bin/kafka-server-start.sh config/server.properties <br />
```
   If starting this server gives an error, <br />
```
   i. cd /tmp <br />
   ii. rm -r kafka-logs
```
11. Terminal 3: Create the topic and start the producer <br />
```
   i. bin/kafka-topics.sh --create --topic {topic_name} --bootstrap-server {Public Ipv4 Addr}:9092 --replication-factor 1 --partitions 1 <br />
   ii. bin/kafka-console-producer.sh --topic {topic_name} --bootstrap-server {Public Ipv4 Addr}:9092
```
12. Terminal 4: Start consumer <br />
```
   i. bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server {Public Ipv4 Addr}:9092
```
   
