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

Docker Storage Types Comparison

| Feature                            | Volumes                                                                                                                                                            | Bind Mounts                                                                                                                        |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Storage Location**               | Managed by Docker (`/var/lib/docker/volumes/`)                                                                                                                     | Anywhere on host filesystem                                                                                                        |
| **Content Population**             | New volume has contents of container image                                                                                                                         | Empty directory in the host                                                                                                        |
| **Management**                     | Docker CLI commands and APIs                                                                                                                                       | Host filesystem commands                                                                                                           |
| **Permissions**                    | Can be modified by Docker container processes                                                                                                                      | Can be modified by both host and container processes                                                                               |
| **Portability**                    | High (easily movable between containers)                                                                                                                           | Low (dependent on host filesystem structure)                                                                                       |
| **Performance**                    | Optimized for container storage                                                                                                                                    | Depends on host filesystem                                                                                                         |
| **OS Support**                     | Works the same on all platforms                                                                                                                                    | May have issues with file paths between OS                                                                                         |
| **Initialization**                 | Can be pre-populated with container content                                                                                                                        | Empty by default unless files exist at mount point                                                                                 |
| **Sharing**                        | Can be easily shared between containers                                                                                                                            | Must be manually shared via host filesystem                                                                                        |
| **Backup**                         | Can be backed up using Docker commands                                                                                                                             | Must be backed up using host filesystem tools                                                                                      |
| **Best Used For**                  | - Production database storage<br>- Application data persistence<br>- Container data sharing<br>- Docker-managed backups                                            | - Local development<br>- Source code mounting<br>- Configuration files<br>- Build artifacts                                        |
| **Example Command**                | `docker run -v mysql-data:/var/lib/mysql mysql`                                                                                                                    | `docker run -v /home/user/code:/app node`                                                                                          |
| **Access from Host**               | Requires Docker commands to access                                                                                                                                 | Direct access through host filesystem                                                                                              |
| **Creation in Dockerfile**         | Can be declared with VOLUME instruction                                                                                                                            | Cannot be declared in Dockerfile                                                                                                   |
| **Runtime Overhead**               | Minimal                                                                                                                                                            | Slightly higher due to filesystem translation                                                                                      |
| **Behavior on Container Deletion** | - Volume persists<br>- Must be explicitly deleted using `docker volume rm`<br>- Data remains until volume is manually removed<br>- Can be reused by new containers | - Mount point remains on host<br>- All data remains in host filesystem<br>- No cleanup needed<br>- Files remain accessible on host |
| **Cleanup Command**                | `docker volume rm <volume_name>` or `docker volume prune`                                                                                                          | Normal filesystem commands (rm, del, etc.)                                                                                         |

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

#### Finding VOLUME in Official Images:

- Check official image repositories on GitHub:
  - MySQL: https://github.com/docker-library/mysql/blob/master/Dockerfile
  - PostgreSQL: https://github.com/docker-library/postgres/blob/master/Dockerfile

Example VOLUME declarations:

| Image      | Volume Location                   |
| ---------- | --------------------------------- |
| MySQL      | `VOLUME /var/lib/mysql`           |
| PostgreSQL | `VOLUME /var/lib/postgresql/data` |
| MongoDB    | `VOLUME /data/db`                 |

**Note**: The VOLUME instruction creates a mount point and marks it for external storage

```dockerfile
VOLUME /var/lib/mysql
```

- Creates new volume location when container starts
- Files in this location outlive the container
- Requires manual cleanup

#### Examples:

| Command                                                                                                                                                | Description                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------- |
| `docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`                                             | Creates container with named volume         |
| `docker container run -d --name mysql3 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`                                            | Reuses existing named volume                |
| `docker container run -d --name postgres -e POSTGRES_PASSWORD=mysecretpassword -v postgres-db:/var/lib/postgresql/data postgres`                       | Creates container with named volume         |
| `docker container run -d --name postgres2 -e POSTGRES_PASSWORD=mysecretpassword -v postgres-db:/var/lib/postgresql/data postgres:15.4`                 | Reuses volume with specific version         |
| `docker container run -d --name postgres3 -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_DB=myapp -v postgres-db:/var/lib/postgresql/data postgres` | Creates container with custom database name |

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

### PostgreSQL Docker Image Notes

- **Authentication Options:**

  - Set password: `POSTGRES_PASSWORD=mypasswd`
  - Disable password: `POSTGRES_HOST_AUTH_METHOD=trust`

- **Supported Image Versions:**
  - Old version: `postgres:15.1`
  - New version: `postgres:15.2`

Note: Authentication changes were made in Docker Hub image, not in PostgreSQL itself.
