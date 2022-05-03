# Building all containers


```bash
docker build -t classificador-produtos ./classificador-produtos

docker build -t http-api-classificacao-produtos-container-unico ./http-api-classificacao-produtos-container-unico

docker build -f ./http-api-classificacao-produtos-dois-containers/Dockerfile-wsgi -t wsgi-app ./http-api-classificacao-produtos-dois-containers/

docker build -f ./http-api-classificacao-produtos-dois-containers/Dockerfile-nginx -t custom-nginx ./http-api-classificacao-produtos-dois-containers/

docker build -t analise-sentimentos-consumer ./analise-sentimentos
```



# Running all containers

First need to stop all containers:
```bash
docker stop http-api-classificacao-produtos-container-unico-container
docker stop wsgi-app-container
docker stop my-custom-nginx-container
docker stop zookeeper
docker stop broker
docker stop analise-sentimentos-consumer-container
```


```bash
docker run -d -p 8080:80 --network minharede --rm --name http-api-classificacao-produtos-container-unico-container http-api-classificacao-produtos-container-unico

docker run -d --rm --name wsgi-app-container --network minharede wsgi-app

docker run -d -p 8081:80 --rm --name my-custom-nginx-container --network minharede custom-nginx

docker run -d --rm --name zookeeper --network minharede -e ZOOKEEPER_CLIENT_PORT=2181 -e ZOOKEEPER_TICK_TIME=2000 confluentinc/cp-zookeeper:7.0.1

docker run -d --rm --name broker --network minharede -p 9092:9092 -e KAFKA_BROKER_ID=1 -e KAFKA_ZOOKEEPER_CONNECT='zookeeper:2181' -e KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=PLAINTEXT:PLAINTEXT,PLAINTEXT_INTERNAL:PLAINTEXT -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092,PLAINTEXT_INTERNAL://broker:29092 -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 -e KAFKA_TRANSACTION_STATE_LOG_MIN_ISR=1 -e KAFKA_TRANSACTION_STATE_LOG_REPLICATION_FACTOR=1 confluentinc/cp-kafka:7.0.1

docker run -d --rm --name analise-sentimentos-consumer-container --network minharede analise-sentimentos-consumer
```




# Building Nagios image

```
docker build -t custom-nagios-server ./nagios
```


# Running Nagios container
```bash
docker run -p 80:80 --rm --name nagios-server --network minharede custom-nagios-server
```

# References
[Pratica devops com docker para machine learning](https://aurimrv.gitbook.io/pratica-devops-com-docker-para-machine-learning/)(base) 
