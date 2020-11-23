# Sample microservices application #

Sample microservices application for managing products and shopping lists using:

This application consists of four different services:

- **Product service:** Provides API for managing products. By default it runs on port `8001`.
- **Shopping list service:** Provides API for managing shopping lists. By default it runs on port `8002`.
- **Service discovery:** Netflix Eureka service that discovers and registers other service instances. By default it runs on port `8761`.
- **API gateway:** Netflix Zuul API gateway that sits on the top of the product and shopping list services, providing a gateway for those services. By default it runs on port `8765`.

## External services

This application depends on external services that must be up and running before attempting to run the application:

### MongoDB

Shopping and product services use MongoDB for persistence, but different databases are used for each service.

Before running the application, ensure that you have a MongoDB instance running on `localhost` port `27017` (default port). The `product` and `shopping-list` databases will be created by the application if they don't exist.

## Building and running this application

To build and run this application, follow these steps:

1. Open a command line window or terminal.
1. Navigate to the root directory of the project, where the `pom.xml` resides.
1. Compile the project: `mvn clean compile`.
1. Package the application: `mvn package`.
1. Change into the `target` directory of the `dist` module: `cd dist/target`.
1. You should see a folder with the following or a similar name: `microservices-1.0`. Change into this folder: `cd microservices-1.0`.
1. Start the services as indicated below (the order doesn't matter).

### Running the service discovery application

1. Open a command line window or terminal.
1. Start the `service-discovery` application: `java -jar service-discovery-1.0.jar`.
1. A Netflix Eureka console will be available at `http://localhost:8761`.

### Running the product service application

1. Open a command line window or terminal.
1. Start the `product-service` application: `java -jar product-service-1.0.jar`.
1. This service will start on the port `8001` and it will automatically register itself in the service discovery. Check the Eureka console.

### Running the shopping product list service application

1. Open a command line window or terminal.
1. Start the `shopping-list-service` application: `java -jar shopping-list-service-1.0.jar`.
1. This service will start on the port `8002` and it will automatically register itself in the service discovery. Check the Eureka console.

### Running the API gateway application

1. Open a command line window or terminal.
1. Start the `api-gateway` application: `java -jar api-gateway-1.0.jar`.

### Running extra instances (optional)

If you want to, you can run extra instances of `product-service` and `shopping-list-service` applications, just use a different port: `java -DPORT=8003 -jar product-service-1.0.jar`. New instances will automatically register themselves in the service discovery.

Requests coming from the `api-gateway` service will be balanced between the instances.

## REST API overview

The application provides a REST API for managing tasks. See below with the supported operations (can use postman):

### Get all products

```
request type - GET \
url - 'http://localhost:8765/api/products' \
header - 'Accept: application/json'
```

### Create a product

```
request type - POST \
url - 'http://localhost:8765/api/products' \
header - 'Content-Type: application/json' \
'{
  "name": "Bag"
}'
```

### Get a product by id

```
request type - GET \
url -  'http://localhost:8765/api/products/{product-id}' \
header - 'Accept: application/json'
```

### Update a product

```
request type - POST \
url -   'http://localhost:8765/api/products/{product-id}' \
header - 'Content-Type: application/json' \
   '{
  "name": "Leather Bag"
}'
```

### Delete a product by id

```
request type - DELETE \
url - 'http://localhost:8765/api/products/{product-id}'
```

### Get all shopping lists

```
request type - GET \
url - 'http://localhost:8765/api/shopping-lists' \
header - 'Accept: application/json'
```

### Create a shopping list

```
request type - POST \
url - 'http://localhost:8765/api/shopping-lists' \
header - 'Content-Type: application/json' \
 '{
  "name": "My shopping list",
  "items": [
    {
      "id": "{product-id}"
    },
    {
      "id": "{product-id}"
    },
    ...
  ]
}'
```

### Get a shopping list by id

```
request type - GET \
url -  'http://localhost:8765/api/shopping-lists/{shopping-list-id}' \
header - 'Accept: application/json'
```

### Update a shopping list

```
request type - PUT \
url -  'http://localhost:8765/api/shopping-lists/{shopping-list-id}' \
header - 'Content-Type: application/json' \
'{
  "name": "Birthday party",
  "items": [
    {
      "id": "{product-id}"
    },
    {
      "id": "{product-id}"
    },
    ...
  ]
}'
```

### Delete a shopping list by id

```
request type -  DELETE \
url -  'http://localhost:8765/api/shopping-lists/{shopping-list-id}'
```