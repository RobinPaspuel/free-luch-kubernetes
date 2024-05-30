# Free lunch day!

The restaurant manager must be able to tell the kitchen that a dish must be prepared, the kitchen randomly selects the dish to prepare and asks the food warehouse for the required ingredients, if the warehouse has availability it delivers the ingredients to the kitchen, if not You must buy them in the market place. When the kitchen receives the ingredients, it prepares the dish and delivers the prepared dish.


App developed with microservices based back-end and NextJS front-end.




## Technology Stack
- [Node.js (Express)](https://expressjs.com/)
- [MongoDB](https://www.mongodb.com/)
- [RabbitMQ](https://www.rabbitmq.com/)
- [Nginx](https://www.nginx.com/)
- [NextJS](https://nextjs.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/es/)
- [Helm](https://helm.sh/)
- [Prometheus](https://prometheus.io)

## Get Started
> Docker and Docker Compose are required.

> Helm is required for Kubernetes deployment.
- Clone the project
- Run (If using docker-compose) :
  ```bash
  $ docker-compose build && docker-compose up
  ```
- Wait for all the services to start.
    > The process of connecting to **rabbitMQ** service may cause the other services to restart a couple of times.
- The app is available at 
    > `http://localhost/`

- Kubernetes deployment

  Place in free-luch directory and run:

  ```bash
  $ helm install free-luch .
  ```

  > Make shure you have Helm installed previously.

  The app now runs on `http://<Cluster IP>:30080` instead of localhost.
## App Services

### Kitchen

The kitchen service is in charge of recieving orders and assigning random dishes to it based on the number requested by the user, finally the order is passed to the inventory service. The kitchen service is connected to its own MongoDB Database (kitchen) with two collections: Orders and Recipes. 

### Inventory

The inventory service is in charge of checking the availability of the products specified in the order passed by the kitchen, reducing the required quantity and buying products if needed. The inventory service registers all the purchases and finally updates the state of the order from "received" to "dispatched". The inventory service is connected to its own MongoDB Database (inventory) with two collections: Products and Purchases.

### Services Communication
The communication between the servies is done through message broker, this allow to handle the unavailability of a service. In this project RabbitMQ is used as the message broker.


# Additional Information

To access to the RabbitMQ manager follow the link `http://<Cluster IP>:15672/`. In the case of using docker-compose it is running in `localhost`.

- Username: guest
- Password: guest

The connection URL to MongoDB is `mongodb://<Cluster IP>:27017`. In the case of using docker-compose it is running in `localhost`.

## Known Problems

> The Nginx server may not start correctly (502 badway responses) using docker-compose, to solve reload the container.

# Kubernetes Deployment Details

## Helm

It is a package manager for Kubernetes. It helps to manage Kubernetes applications. It introduces the concept of Charts, which are a collection of resources like deployments and services which together forms an application (package).

## Prometheus

It allows to monitor and set alarms based on the resources of out cluster. Prometheus is installed separate from the application.

