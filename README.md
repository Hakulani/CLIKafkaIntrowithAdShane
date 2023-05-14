# CLIKafkaIntrowithAdShane
CLI Special Live: Kafka Intro with Ad Shane

#Clone git
<pre><code>gh repo clone nuttatunc/kafka-r2de-demo</code></pre>
#VM CLI
<pre><code>ls -lrt</code></pre>
<pre><code>cd kafka-r2de-demo/</code></pre>
<pre><code>ls -lrt</code></pre>
#Docker compose confluent</code></pre>
<pre><code>docker compose up -d</code></pre>
#show images 
<pre><code>docker ps -a</code></pre>
#Web UI kafka external IP:port
<external IP>:9021
#SQL Database in GCP CLI using connection name
<pre><code>gcloud sql connect kafka-class-db-001 --user=root --quiet</code></pre>
Enter password:

mysql> <pre><code>use demo;</code></pre>
mysql> <pre><code>select * form movies;</code></pre>





#Create source connector
curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-source.json
#Create sink connector
curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-sink-kafka.json


#Start ksqlDB
docker exce -it ksqldb http://localhost:8088
#Set offset 
ksql> SET 'auto.offset.reset'='earliest';
ksql> show topics;
ksql>show connectors;
#CREATE STREAM <stram name >
ksql> CREATE STREAM ksqlstream WITH 
(KAFKA_TOPIC='kafka-class-db-001.demo.movies', VALUE_FORMAT = 'AVRO');
ksql> show streams;
#Create stream
ksql> SELECT * FROM ksqlstream EMIT CHANGES LIMIT 10;
#Create stream trasform data
ksql> CREATE STREAM ksqlstream_processed AS 
SELECT 
after->movie_id AS movie_id,
after->title AS title,
2023 - CAST(after->release_year AS INT) AS num_year_release
FROM ksqlstream
WHERE op in ('c','u','r');
#where op:create uodate remove
ksql> show streams;
ksql> SELECT * FROM ksqlstream_processed EMIT CHANGES LIMIT 10;

ksql>exit;

#Create sink connector
curl -X GET  -H "Accept: application/json" -H "Content-Type: application/json" http://localhost:8083/connectors/ -d @mysql-sink-ksqldb.json

