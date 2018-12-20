
## This file has few main steps in the history file

## Create kafka directory, copy the docker-compose file into kafka directory
#### mkdir kafka
#### cd kafka
#### cp ../course-content/06-Transforming-Data/docker-compose.yml docker-compose.yml

## Get the .json file
#### curl -L -o assessment-attempts-20180128-121051-nested.json https://goo.gl/ME6hjp

## Spin up the container
#### docker-compose up -d

## Create a topic foo
#### docker-compose exec kafka   kafka-topics     --create   --topic foo   --partitions 1   --replication-factor 1   --if-not-exists   --zookeeper zookeeper:32181

## Publish messages to kafka
#### docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100 messages.'"

## Consume the message
#### docker-compose exec mids bash -c "kafkacat -C -b kafka:29092 -t foo -o beginning -e"

## Spin down the container, spin up the container, check the log file, spin down the container, save to history file
#### docker-compose down
#### docker-compose up -d
#### docker-compose ps
#### docker-compose logs -f kafka
#### docker-compose down
#### history > hong-history.txt
