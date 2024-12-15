# Creating and Using Docker Images Like a Boss

## Table of Contents

1. [Docker Hub Registry Images](#1-docker-hub-registry-images)

   1.1. [Using Docker Hub](#11-using-docker-hub)

   1.2. [Basic Image Commands](#12-basic-image-commands)

2. [Image Inspection and Layers](#2-image-inspection-and-layers)

   2.1. [View Image Information](#21-view-image-information)

   2.2. [Examine Image Layers](#22-examine-image-layers)

3. [Image Tagging and Registry Management](#3-image-tagging-and-registry-management)

   3.1. [Tag Images](#31-tag-images)

   3.2. [Push Images to Registry](#32-push-images-to-registry)

4. [Building Images with Dockerfile](#4-building-images-with-dockerfile)

   4.1. [Dockerfile Components](#41-dockerfile-components)

   4.2. [Build Best Practices](#42-build-best-practices)

   4.3. [Building Custom Images](#43-building-custom-images)

   4.4. [Extending Official Images](#44-extending-official-images)

5. [Image Cleanup and Maintenance](#5-image-cleanup-and-maintenance)

   5.1. [Pruning Commands](#51-pruning-commands)

## 1. Docker Hub Registry Images

### 1.1 Using Docker Hub

- Access Docker Hub at `http://hub.docker.com`
- Official repository for Docker images
- Contains both official and community images

### 1.2 Basic Image Commands

#### Examples:

| Command                          | Description                      |
| -------------------------------- | -------------------------------- |
| `docker pull nginx`              | Downloads latest nginx image     |
| `docker pull nginx:1.11.9`       | Downloads specific nginx version |
| `docker pull mysql/mysql-server` | Downloads MySQL server image     |

## 2. Image Inspection and Layers

### 2.1 View Image Information

- `docker image ls`: Lists all locally stored Docker images
- `docker image inspect nginx`: Shows detailed information about nginx image

### 2.2 Examine Image Layers

- `docker image history nginx:latest`: Shows layer history of nginx image
- `docker image history mysql`: Shows layer history of MySQL image

## 3. Image Tagging and Registry Management

### 3.1 Tag Images

- `docker image tag --help`: Shows help for image tagging
- `docker image tag nginx username/nginx`: Tags nginx image with username
- `docker image tag username/nginx username/nginx:testing`: Creates new tag for existing image

### 3.2 Push Images to Registry

#### Examples:

| Command                                    | Description                       |
| ------------------------------------------ | --------------------------------- |
| `docker login -u username`                 | Logs into Docker Hub              |
| `docker image push username/nginx`         | Pushes tagged image to Docker Hub |
| `docker image push username/nginx:testing` | Pushes specific image tag         |

## 4. Building Images with Dockerfile

### 4.1 Dockerfile Components

| Instruction | Purpose                                             |
| ----------- | --------------------------------------------------- |
| `FROM`      | Specifies base image (e.g., debian, ubuntu, alpine) |
| `ENV`       | Sets environment variables                          |
| `WORKDIR`   | Sets working directory for subsequent instructions  |
| `RUN`       | Executes commands in new layer                      |
| `EXPOSE`    | Documents which ports should be published           |
| `COPY`      | Copies files from host into container filesystem    |
| `CMD`       | Specifies default command to run container          |

### 4.2 Build Best Practices

- Order matters in Dockerfile
- Place least-changed instructions at top
- Place frequently-changed instructions at bottom
- For logging, output to stdout/stderr instead of files
- Only one CMD instruction allowed per Dockerfile

### 4.3 Building Custom Images

#### Examples:

| Command                                             | Description                   |
| --------------------------------------------------- | ----------------------------- |
| `docker image build -t customnginx -f Dockerfile .` | Builds image from Dockerfile  |
| `docker image build -t nginx-with-html .`           | Builds image with custom HTML |

### 4.4 Extending Official Images

#### Examples:

| Command                                              | Description               |
| ---------------------------------------------------- | ------------------------- |
| `docker container run -p 80:80 --rm nginx`           | Runs official nginx image |
| `docker container run -p 80:80 --rm nginx-with-html` | Runs custom nginx image   |

## 5. Image Cleanup and Maintenance

### 5.1 Pruning Commands

#### Examples:

| Command                  | Description                                         |
| ------------------------ | --------------------------------------------------- |
| `docker system df`       | Shows Docker disk usage                             |
| `docker image prune`     | Removes dangling images                             |
| `docker image prune -a`  | Removes all unused images                           |
| `docker system prune`    | Removes all unused containers, networks, and images |
| `docker system prune -a` | Removes all unused resources including build cache  |

**Note**: Use `-f` or `--force` flag with prune commands to skip confirmation prompt.

**Warning**: Be careful with `prune -a` commands as they remove all unused resources, which might be needed later.
