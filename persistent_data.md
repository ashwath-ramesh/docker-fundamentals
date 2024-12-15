# Container Lifetime & Persistent Data

# Container Lifetime & Persistent Data: A Complete Guide

## Table of Contents

1. [Core Concepts](#1-core-concepts)

   1.1. [Key Principles](#11-key-principles)

   1.2. [Storage Solutions](#12-storage-solutions)

2. [Data Volumes](#2-data-volumes)

   2.1. [Volume Management](#21-volume-management)

   2.2. [Named Volumes](#22-named-volumes)

   2.3. [Volume Commands](#23-volume-commands)

3. [Bind Mounts](#3-bind-mounts)

   3.1. [Basic Usage](#31-basic-usage)

   3.2. [Development Workflow](#32-development-workflow)

   3.3. [Examples and Use Cases](#33-examples-and-use-cases)

## 1. Core Concepts

### 1.1 Key Principles

- **Immutable Infrastructure**

  - Containers should never be changed, only re-deployed
  - Ensures consistency and reproducibility
  - Simplifies rollback and version control

- **Separation of Concerns**
  - Application binaries should be separate from data
  - Unique data should not be mixed with application code
  - Enables independent lifecycle management

### 1.2 Storage Solutions

Docker provides two main solutions for persistent data:

| Solution     | Description                                                 |
| ------------ | ----------------------------------------------------------- |
| Data Volumes | Special location outside container UFS (unique file system) |
| Bind Mounts  | Direct mapping between container path and host path         |

## 2. Data Volumes

### 2.1 Volume Management

- Volumes persist after container removal
- Require manual deletion
- Managed through dedicated volume commands
- Can be shared between containers

#### Examples:

| Command                               | Description            |
| ------------------------------------- | ---------------------- |
| `docker volume ls`                    | Lists all volumes      |
| `docker volume inspect <volume-name>` | Shows volume details   |
| `docker volume prune`                 | Removes unused volumes |

### 2.2 Named Volumes

#### Dockerfile Configuration:

```dockerfile
VOLUME /var/lib/mysql
```

- Creates new volume location when container starts
- Files in this location outlive the container
- Requires manual cleanup

#### Examples:

| Command                                                                                                     | Description                         |
| ----------------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`  | Creates container with named volume |
| `docker container run -d --name mysql3 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql` | Reuses existing named volume        |

### 2.3 Volume Commands

#### Basic Volume Operations:

| Command                 | Description                 |
| ----------------------- | --------------------------- |
| `docker volume create`  | Creates a volume            |
| `docker volume ls`      | Lists volumes               |
| `docker volume inspect` | Displays volume information |
| `docker volume rm`      | Removes volume              |

## 3. Bind Mounts

### 3.1 Basic Usage

- Maps host file/directory to container file/directory
- Cannot be used in Dockerfile
- Must be specified at container runtime
- Changes in host reflect immediately in container

### 3.2 Development Workflow

- Ideal for local development
- Enables real-time code changes
- Supports rapid testing and debugging

### 3.3 Examples and Use Cases

#### Basic Bind Mount:

| Command                                                                               | Description                                    |
| ------------------------------------------------------------------------------------- | ---------------------------------------------- |
| `docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx` | Maps current directory to nginx html directory |
| `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`                       | Maps current directory for Jekyll development  |

#### Development Setup:

| Command                                                  | Description                          |
| -------------------------------------------------------- | ------------------------------------ |
| `docker container exec -it nginx bash`                   | Access container shell for debugging |
| `docker container run -d --name nginx2 -p 8080:80 nginx` | Run secondary container for testing  |

Best Practices:

- Use named volumes for production data persistence
- Use bind mounts for development environments
- Always backup important data volumes
- Use volume drivers for cloud storage when needed

---

For docker run, and the forthcoming Docker Compose sections, you need to either set a password with the environment variable:

POSTGRES_PASSWORD=mypasswd

Or tell it to ignore passwords with the environment variable:

POSTGRES_HOST_AUTH_METHOD=trust

Note this change was in the Docker Hub image, and not a change in postgres itself.

So, when you see the "old" and "new" versions of the Postgres SQL image in the Assignment video, you can replace them with these versions:

postgres:15.1

postgres:15.2
