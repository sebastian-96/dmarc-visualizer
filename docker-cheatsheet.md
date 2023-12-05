# Docker

## Overview

Docker is a platform for developing, shipping, and running applications in containers. Containers allow you to package an application with its dependencies and run it consistently across different environments.

## Images

- Build a Docker image from a Dockerfile: `docker build -t your-image-name .`
- List all Docker images: `docker images`
- Remove a Docker image: `docker rmi your-image-name`

**Export/Import Images:**

- Export a Docker image as a tarball: `docker save -o your-image.tar your-image-name`
- Export a Docker container as a tarball: `docker export your-container-name > your-container.tar`
- Import/Load a Docker image from a tarball: `docker load -i your-image.tar`
- Import a Docker container from a tarball: `docker import your-container.tar`

## Getting Container Status

- List all running Docker containers: `docker ps`
- List all Docker containers (including stopped): `docker ps -a`
- Show detailed information about a container: `docker inspect your-container-name`
- Show logs of a running container: `docker logs your-container-name`

## Running Containers

- Run a Docker container in detached mode: `docker run -d --name your-container-name your-image-name`
- Run a Docker container and enter (interactive session): `docker run -it --name your-container-name your-image-name`

## Interacting with Containers

- Enter a running Docker container (interactive session): `docker exec -it your-container-name bash`
- Show environment variables in a Docker container: `docker exec your-container-name env`

## Stopping/Removing Containers

- Stop a running Docker container: `docker stop your-container-name`
- Remove a stopped Docker container: `docker rm your-container-name`

## Mounting Files

- Run a Docker container with a mounted volume using `-v`: `docker run -it -v /host/path:/container/path your-image-name`
- Run a Docker container with a mounted volume using `--mount`: `docker run -it --mount src=/host/path,target=/container/path,type=bind your-image-name`

   **Note:**
   - The `-v` method uses a concise syntax and is considered legacy but still widely used.
   - The `--mount` method provides more options and is recommended for complex use cases.
   - Changes made outside the container directory will reflect inside and vice versa.

## System Status and Information

- Display system-wide information: `docker info`
- Show Docker disk usage: `docker system df`
- Display Docker version and info: `docker version`


# Docker-Compose

## Overview

Docker Compose simplifies the management of multi-container applications, allowing you to define, configure, and deploy complex setups with a single command.
When you run docker-compose with a YAML file, it defines services, networks, and volumes for your application. 
Each service corresponds to a containerized application or component. 

Docker Compose automatically creates networks for communication between containers and uses volumes for data persistence.
Containers started with Docker Compose are treated as services with hostnames, enabling inter-service communication. 
This is different from using Docker alone, where containers may not have meaningful hostnames.

Docker Compose provides an easy way to orchestrate multiple containers, making it a powerful tool for developing and deploying complex applications.

## Images in Docker-Compose

- Define services using images or build from Dockerfile in the docker-compose.yml file.

   **Note:**
   - When defining services, you can use existing Docker images or specify a build context to build images from a Dockerfile.
   - Using `image` allows you to use pre-built Docker images available on a registry.
   - Using `build` allows you to specify a build context, typically the path to a Dockerfile, to build a custom image for the service.

    This following example defines two services in the docker-compose.yml file.
    The first service (`service-with-image`) uses an existing Docker image (`your-image-name:tag`). 
    The second service (`service-with-build`) is configured to build its image from a Dockerfile located at `./path/to/Dockerfile`.

    ```yaml
    services:
        service-with-image:
            image: your-image-name:tag

        service-with-build:
            build: ./path/to/Dockerfile
    ```

## Running Containers with Docker-Compose

- Start services defined in the docker-compose.yml file in detached mode: `docker-compose up -d`

   **Note:**
   - `docker-compose up` starts the services defined in the YAML file.
   - The command also creates a network for communication between the services, and any defined volumes for data persistence.

   **Detached Mode:**
   - The `-d` option in `docker-compose up -d` runs the containers in the background (detached mode), allowing you to continue using the terminal.
   - This means the services will run independently in the background, and you regain control of your terminal for other commands.
   
   Additional Options:
   - To rebuild images before starting services, use `docker-compose up -d --build`.
   - To start specific services, you can provide their names as arguments, for example, `docker-compose up -d service1 service2`.
   - To rebuild and start a specific service, use `docker-compose up -d --build service-name`.

## Stopping/Removing Containers with Docker-Compose

- Stop and remove all services, networks, and volumes defined in the docker-compose.yml file: `docker-compose down`
- Stop services defined in the docker-compose.yml file: `docker-compose stop`
- Remove services defined in the docker-compose.yml file: `docker-compose rm`

   **Note:**
   - `docker-compose down` stops and removes all containers, networks, and volumes defined in the YAML file.
   - `docker-compose stop` stops the running services without removing them.
   - `docker-compose rm` removes stopped services.

## Interacting with Containers in Docker-Compose

- Enter a running Docker-Compose service (interactive session): `docker-compose exec your-service-name bash`
- Show logs for all services: `docker-compose logs`
- Show logs for a specific service: `docker-compose logs your-service-name`
- Show logs continuously (follow) for all services: `docker-compose logs -f`
- Show logs continuously (follow) for a specific service: `docker-compose logs -f your-service-name`

# Mounting Files in Docker-Compose

- Specify file mounts between the host and containers using the volumes section in the docker-compose.yml file.

  **Note:**
  - The `volumes` section in the YAML file specifies how files are shared between the host and the container.
  - The line `- /host/path:/container/path` mounts the `/host/path` directory on the host to `/container/path` in the container.
  - The line `- volume-name:/path/in/container` mounts a named volume (`volume-name`) to the specified path (`/path/in/container`) in the container.
  - Changes made outside the container directory will reflect inside and vice versa.
  
  This following example illustrates file mounts in the docker-compose.yml file.

  ```yaml
  # Mounting Files

  volumes:
    - /host/path:/container/path
    - volume-name:/path/in/container
  ```

## Scaling Services in Docker-Compose

- Scale a service (replica count): `docker-compose up -d --scale your-service-name=3`

## System Status and Information in Docker-Compose

- Display Docker-Compose version information: `docker-compose version`