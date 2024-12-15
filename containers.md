# Creating and Using Containers Like a Boss

# Creating and Using Containers Like a Boss

## Table of Contents

1. [Install Docker](#1-install-docker)

2. [Docker Networks Setup](#2-docker-networks-setup)

   2.1. [Create a new network](#21-create-a-new-network)

   2.2. [Verify network configs](#22-verify-network-configs)

   2.3. [Connect / dis-connect containers to networks](#23-connect--dis-connect-containers-to-networks)

   2.4. [Misc](#24-misc)

3. [Container Creation and Management](#3-container-creation-and-management)

   3.1. [Create and run containers](#31-create-and-run-containers)

   3.2. [Create and run containers within project network](#32-create-and-run-containers-within-project-network)

   3.3. [Monitor container status & logs](#33-monitor-container-status--logs)

   3.4. [Examine container details](#34-examine-container-details)

   3.5. [Start a stopped container](#35-start-a-stopped-container)

4. [Container Shell Access](#4-container-shell-access)

   4.1. [Creates new container with shell access](#41-creates-new-container-with-shell-access)

   4.2. [Access running containers for debugging](#42-access-running-containers-for-debugging)

5. [Container Network Verification](#5-container-network-verification)

   5.1. [Verify port mapping](#51-verify-port-mapping)

   5.2. [Checks network connections](#52-checks-network-connections)

   5.3. [Tests inter-container communication](#53-tests-inter-container-communication)

6. [Container Cleanup](#6-container-cleanup)

## 1. Install Docker

Docker command format: `docker <command> <sub-command> (options)`

- `docker version`: Returns version of client and server
- `docker info`: Shows system-wide information about Docker installation
- `docker`: Shows a list of all available Docker commands and their descriptions
- `docker ps -a`: Lists all containers (running and stopped)
- `docker image ls`: Lists all locally stored Docker images
- `docker pull alpine`: Downloads Alpine Linux image from Docker Hub

## 2. Docker Networks Setup

- `docker network <command> <options>`: Create and manage container networks for application communication
- `docker network ls`: Lists all Docker networks

### 2.1 Create a new network

`docker network create <network-name>`: Creates isolated network for project containers

#### Examples:

| Command                            | Description                              |
| ---------------------------------- | ---------------------------------------- |
| `docker network create my_app_net` | Creates a new network named 'my_app_net' |

### 2.2 Verify network configs

- `docker network inspect <network-name>`: Verifies network configuration

#### Examples:

| Command                             | Description                                                            |
| ----------------------------------- | ---------------------------------------------------------------------- |
| `docker network inspect my_app_net` | Shows detailed information about the custom network named 'my_app_net' |

### 2.3 Connect / dis-connect containers to networks

- `docker network connect <network> <container>`: Connects existing containers to network. Use IDs of each.
- `docker network disconnect <network> <container>`: Disconnects a container from a network

#### Examples:

| Command                                       | Description                                             |
| --------------------------------------------- | ------------------------------------------------------- |
| `docker network connect my_app_net webserver` | Connects 'webserver' container to 'my_app_net' network  |
| `docker network disconnect my_app_net proxy`  | Disconnects 'proxy' container from 'my_app_net' network |
| `docker network connect my_app_net db`        | Connects 'db' container to 'my_app_net' network         |

### 2.4 Misc

- `docker network --help`: Shows help for all network commands
- `docker network create --help`: Shows help for network create command
- `--network bridge`: Default Docker virtual network that provides NAT (Network Address Translation) behind the host IP. Containers on this network can communicate with each other and access external networks through the host.
- `--network host`: Removes network isolation between container and host - container uses host's network directly. Provides best networking performance but less security isolation.
- `--network none`: Completely isolates container from network access. Container has only loopback interface. Useful for maximum security when no network access is needed.

## 3. Container Creation and Management

### 3.1 Create and run containers

- `docker container run <options> <image>`: Main template for creating and running containers

#### Examples:

| Command                                                              | Description                                                                                                                                             |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docker container run --publish 80:80 nginx`                         | Starts a new nginx container and publishes port 80 from the container to port 80 on the host. Publish exposes and sends traffic from port 80 to port 80 |
| `docker container run --publish 80:80 --detach nginx`                | Run in background mode                                                                                                                                  |
| `docker container run --publish 80:80 --detach --name webhost nginx` | Starts a detached nginx container named 'webhost' with port 80 mapped                                                                                   |

### 3.2 Create and run containers within project network

- `docker container run -d --name <name> --network <network> <image>`: Creates containers within project network

#### Examples:

| Command                                                     | Description                                                                  |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `docker container run -d --name webserver -p 8080:80 httpd` | Starts a detached Apache web server container named 'webserver' on port 8080 |
| `docker container run -d --name proxy -p 80:80 nginx`       | Starts a detached Nginx proxy container named 'proxy' on port 80             |

### 3.3 Monitor container status & logs

- `docker container ls`: List running containers.
- `docker container ls -a`: List all containers, including stopped ones.
- `docker container ls docker ps -a`: Monitors container status
- `docker container logs <container>`: Views container logs
- `docker container top <container>`: Shows running processes inside a container
- `docker container stats`: Shows live performance stats for all running containers

#### Examples:

| Command                         | Description                                              |
| ------------------------------- | -------------------------------------------------------- |
| `docker container logs webhost` | Shows logs for the container named 'webhost'             |
| `docker container top webhost`  | Shows running processes in the container named 'webhost' |

### 3.4 Examine container details

- `docker container inspect <container>`: Examines container details

#### Examples:

| Command                          | Description                                          |
| -------------------------------- | ---------------------------------------------------- |
| `docker container inspect mysql` | Shows detailed information about the MySQL container |

### 3.5 Start a stopped container

- `docker start <container>`: Starts a stopped container

#### Examples:

| Command              | Description                        |
| -------------------- | ---------------------------------- |
| `docker start mongo` | Starts the stopped mongo container |

## 4. Container Shell Access

### 4.1 Creates new container with shell access

- `docker container run -it --name <name> <image> <shell>`: Creates new container with shell access

#### Examples:

| Command                                            | Description                                                                |
| -------------------------------------------------- | -------------------------------------------------------------------------- |
| `docker container run -it --name proxy nginx bash` | Starts an interactive nginx container named 'proxy' with bash shell access |
| `docker container run -it --name ubuntu ubuntu`    | Creates and starts an interactive Ubuntu container                         |

### 4.2 Access running containers for debugging

- `docker container exec -it <container> <shell>`: Accesses running containers for debugging

#### Examples:

| Command                                | Description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| `docker container exec --help`         | Shows help for container exec command                        |
| `docker container exec -it mysql bash` | Runs an additional bash process in a running MySQL container |

## 5. Container Network Verification

### 5.1 Verify port mapping

- `docker container port <container>`: Verifies port mappings

#### Examples:

| Command                         | Description                                 |
| ------------------------------- | ------------------------------------------- |
| `docker container port webhost` | Shows port mappings for container 'webhost' |

### 5.2 Checks network connections

- `docker network inspect <network>`: Checks network connections

#### Examples:

| Command                                                                        | Description                                             |
| ------------------------------------------------------------------------------ | ------------------------------------------------------- |
| `docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost` | Displays the internal IP address of container 'webhost' |

### 5.3 Tests inter-container communication

- `docker container exec -it <container1> ping <container2>`: Tests inter-container communication

#### Examples:

| Command                                             | Description                                                                                    |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `docker container exec -it my_nginx ping new_nginx` | Tests network connectivity from my_nginx container to new_nginx container using DNS resolution |
| `docker container exec -it new_nginx ping my_nginx` | Tests network connectivity from new_nginx container to my_nginx container using DNS resolution |

### 6. Container Cleanup

- `docker container stop <container>`: Stops running containers
- `docker container rm <container>`: Removes stopped containers
- `docker container rm -f <container>`: Force removes containers

#### Examples:

| Command                           | Description                                              |
| --------------------------------- | -------------------------------------------------------- |
| `docker stop mongo`               | Stops the mongo container                                |
| `docker container rm 63f 690 ode` | Removes stopped containers with IDs 63f, 690, and ode    |
| `docker container rm -f 63f`      | Force removes container with ID 63f even if it's running |
