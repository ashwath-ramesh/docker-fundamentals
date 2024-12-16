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

https://docs.docker.com

## Trying Out Basic Compose Commands

`docker compose up`

`docker compose up -d`

`docker compose logs`

`docker compose --help`

`docker compose ps`

`docker compose top`

`docker compose down`

## Assignment Answers: Build a Compose File for a Multi-Container Service

docker-compose.yml

docker pull drupal

docker image inspect drupal

docker-compose up

https://hub.docker.com

docker-compose down --help

docker-compose down -v

## Adding Image Building to Compose Files

docker-compose.yml

docker-compose up

docker-compose up --build

docker-compose down

docker image ls

docker-compose down --help

docker image rm nginx-custom

docker image ls

docker-compose up -d

docker image ls

docker-compose down --help

docker-compose down --rmi local

## Assignment Answers: Compose for Run-Time Image Building and Multi-Container Dev

docker-compose up

docker-compose down

docker-compose up
