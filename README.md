# PoCKafka

## Como Rodar esta PoC

### Montando o Ambiente de Desenvolvimento do Kafka

Para rodar esta Prova de Conceito (PoC) do Kafka, siga os passos abaixo:

1. Crie um arquivo chamado `docker-compose.yml` com o seguinte conteúdo:

```yaml
version: '2'

services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    networks:
      - net
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  kafka:
    image: confluentinc/cp-kafka:latest
    networks:
      - net
    depends_on:
      - zookeeper
    ports:
      - 9092:9092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1

networks:
  net:
    driver: bridge
```
2. Execute o comando `docker-compose up –d` para subir o Kafka.
3. Execute o comando `dotnet run "localhost:9092" "topic-teste` para rodar o Consumer.
4. Execute o comando `dotnet run "localhost:9092" "topic-teste" "Teste 1" "Teste 2"` para rodar o Producer.

Referência: https://renatogroffe.medium.com/net-core-3-1-apache-kafka-exemplos-utilizando-mensageria-21fad6e0aab
