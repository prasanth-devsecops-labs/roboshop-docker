# 🚀 RoboShop:  Docker Infrastructure

* This repository contains the Dockerized setup for the RoboShop microservices application stack. It utilizes a mix of official database images and custom-built microservices, all interconnected via a dedicated Docker network.
# 🛠️ Prerequisites
* Docker installed and running.
* A custom Docker network named roboshop:
* docker network create roboshop


# 🗄️ Database Layer (Official Images)
* The following databases are deployed using official images with pre-loaded master data.
# MongoDB 7.0

> docker run -d --name mongodb --network roboshop -p 27017:27017 -v "$(pwd)/db:/tmp/db" mongo:7.0
# Load Master Data
> docker exec -it mongodb mongosh --file /tmp/db/master-data.js

# MySQL 8.0

> docker run -d --name mysql --network roboshop -e MYSQL_ROOT_PASSWORD=RoboShop@1 -v "$(pwd)/shipping/db:/tmp/sql_files" mysql:8.0
# Initialize Schema and Data
> docker exec -i mysql sh -c '
  mysql -u root -p"RoboShop@1" < /tmp/sql_files/schema.sql && \
  mysql -u root -p"RoboShop@1" < /tmp/sql_files/app-user.sql && \
  mysql -u root -p"RoboShop@1" < /tmp/sql_files/master-data.sql'

# Redis 7 & RabbitMQ 3.x

# Redis
> docker run -d --network roboshop --name redis redis:7

# RabbitMQ (with Management Plugin)
> docker run -d --network roboshop --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management sh -c "rabbitmq-server & sleep 10; \
  rabbitmqctl add_user roboshop roboshop123; \
  rabbitmqctl set_permissions -p / roboshop '.*' '.*' '.*'; \
  wait"

# ⚙️ Microservices (Custom Builds)
* Backend Components
* Replace component with the specific service name (e.g., user, cart, catalogue).
> docker build -t component:1.0.0 .
> docker run -d --name component --network roboshop component:1.0.0


# Frontend (Nginx)
* Exposes the application on port 80.

> docker build -t frontend:1.0.0 .
> docker run -d --name frontend --network roboshop -p 80:80 frontend:1.0.0


# ✅ Verification
* All components are healthy and communicating. You can verify the status of the containers by running:
* docker ps
* Ready for testing! 🧪