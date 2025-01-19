# Docker Compose: The Multi-Container Tool

## Docker Compose and The Docker-compose.yml File

Comprised of 2 separate but related things
• 1. YAML-formatted file that describes our solution options for:
• containers
• networks
• volumes
• 2. A CLI tool docker-compose used for local dev/test
automation with those YAML files

[Official Reference](https://docs.docker.com/compose/)
[Template file](compose/template.yml)
[Sample File 1](compose/docker-compose.yml)
[Sample File 2](compose/compose-2.yml)
[Sample File 3](compose/compose-3.yml)

docker-compose.yml

## Components of a docker compose yml file

• `version`: Specifies the Compose file format version (optional in recent versions)

• `services`: Main section defining the containers

- Each service gets a name (e.g. web, db, cache)
- Contains container configuration like:
  - image
  - ports
  - volumes
  - environment variables
  - networks
  - etc.

• `networks`: Optional section to define custom networks

- Allows configuration of network drivers
- Can set up network aliases
- Enables container isolation

• `volumes`: Optional section for persistent data storage

- Named volumes that can be reused
- Bind mounts to host filesystem
- Configuration options for volume drivers

## Trying Out Basic Compose Commands

`docker compose up` - Start containers defined in docker-compose.yml

`docker compose up -d` - Start containers in detached (background) mode

`docker compose down` - Stop and remove containers, networks, and volumes

`docker compose logs` - View output logs from containers

`docker compose --help` - Display help and available commands

`docker compose ps` - List running containers in the compose project

`docker compose top` - Display running processes in containers

## Assignment Answers: Build a Compose File for a Multi-Container Service

docker-compose down -v

## Adding Image Building to Compose Files

Adding Image Building to Compose Files - Enables building custom images directly from Compose rather than pulling pre-built ones
