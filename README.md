# Apache Pinot


## Architecture


## Use Cases


## Examples

Creating a kafka topic for ingesting data:
```shell
docker exec \
    -t kafka-broker \
    kafka-topics \
    --bootstrap-server localhost:9092 \
    --partitions=1 \
    --replication-factor=1 \
    --create \
    --topic transcript-topic
```

Publish messages to the target topic:
```shell
docker exec \
    -i kafka-broker \
    kafka-console-producer \
    --broker-list localhost:9092 \
    --topic transcript-topic < transcript.json
```

Create a consumer in order to check topic messages:
```shell
docker exec \
    -t kafka-broker \
    kafka-console-consumer \
    --bootstrap-server localhost:9092 \
    --topic transcript-topic \
    --from-beginning
```

Create table:
```shell
docker cp transcript_schema.json pinot-controller:/opt
docker cp transcript_table.json pinot-controller:/opt

docker exec \
    -i pinot-controller \
    /opt/pinot/bin/pinot-admin.sh AddTable \
    -schemaFile /opt/transcript_schema.json \
    -tableConfigFile /opt/transcript_table.json \
    -controllerHost localhost \
    -exec
```
