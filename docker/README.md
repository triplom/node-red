# Node-RED Docker Deployment

This guide provides instructions on how to deploy Node-RED using Docker and Docker Compose.

## Prerequisites

- Docker installed on your machine
- Docker Compose installed on your machine

## Deployment Instructions

### Using Dockerfile

1. **Build the Docker image:**

    ```bash
    docker build -t node-red-image .
    ```

2. **Run the Docker container:**

    ```bash
    docker run -d -p 1880:1880 --name node-red-container node-red-image
    ```

### Using Docker Compose

1. **Create a `docker-compose.yml` file in the same directory with the following content:**

    ```yaml
    version: '3.9'
    services:
      node-red:
    image: nodered/node-red:latest
    environment:
      - TZ=Europe/Lisbon
    ports:
      - "1880:1880"
    container_name: node-red-container
    networks:
      - node-red-net
    volumes:
      - node-red-data:/data
    ```

2. **Deploy the stack:**

    ```bash
    docker-compose up -d
    ```

## Accessing Node-RED

Once the container is running, you can access the Node-RED interface by navigating to `http://localhost:1880` in your web browser.

## Stopping the Container

To stop the container, use the following command:

```bash
docker-compose down
```

Or if you are using Docker directly:

```bash
docker stop node-red-container
docker rm node-red-container
```

## Additional Information

For more details on Node-RED, visit the [official Node-RED documentation](https://nodered.org/docs/).
