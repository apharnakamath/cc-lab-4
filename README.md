# Docker Advanced Lab

### Objective

Demonstrate Docker networking, Docker Compose orchestration, and volume persistence using a RabbitMQ-based chat system.

### Project Files

- Dockerfile – Builds the chat application image

- chat.py – Python chat client using RabbitMQ

- docker-compose.yml – Multi-container configuration

#### Build Image
```
sudo docker build -t chat-app .
```

#### Run Without Docker Compose

- Create network:
```
sudo docker network create chat-net
```

- Start RabbitMQ:
```
sudo docker run -d --name rabbitmq --network chat-net rabbitmq:3-management
```

- Run User A (Terminal 1):
```
sudo docker run -it --name user-a \
--network chat-net \
-e RABBIT_HOST=rabbitmq \
-e QUEUE_NAME=queue_a \
-e TARGET_QUEUE=queue_b \
chat-app
```

- Run User B (Terminal 2):
```
sudo docker run -it --name user-b \
--network chat-net \
-e RABBIT_HOST=rabbitmq \
-e QUEUE_NAME=queue_b \
-e TARGET_QUEUE=queue_a \
chat-app
```

#### Run With Docker Compose

- Terminal 1:
```
sudo docker compose up rabbitmq
```

- Terminal 2:
```
sudo docker compose run --rm user-a
```

- Terminal 3:
```
sudo docker compose run --rm user-b
```

#### Persistence

- Docker volumes are used to store chat history in /app/data.
- Chat history remains available even after containers are stopped and restarted.
