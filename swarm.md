# Swarm Intro and Creating a 3-Node Swarm Cluster

## Create Your First Service and Scale it Locally

Official docs: https://docs.docker.com/engine/swarm/

### Swarm Management (Highest Level)

- `docker swarm --help` - Show swarm command help
- `docker swarm init` - Initialize a new swarm
- `docker info` - Check swarm status (active/inactive)
- `docker swarm leave` - Leave the swarm (worker nodes)
- `docker swarm leave --force` - Force leave swarm (manager nodes)

                                SWARM
                                  │
                                  ▼
                               NODES
                                  │
            ┌────────────────────┴─────────────────────┐
            ▼                    ▼                     ▼

  Manager Node 1 Manager Node 2 Worker Node 3
  │ │ │
  │ │ │
  ├─► Service_A ├─► Service_A ├─► Service_A
  │ │ │ │ │ │
  │ ├─► Task_1 │ ├─► Task_2 │ ├─► Task_3
  │ │ (Container) │ │ (Container) │ │ (Container)
  │ │ │ │ │ │
  │ │ │
  ├─► Service_B ├─► Service_B ├─► Service_B
  │ │ │ │ │ │
  │ ├─► Task_1 │ ├─► Task_2 │ ├─► Task_3
  │ │ (Container) │ │ (Container) │ │ (Container)
  │ ├─► Task_4 │ │ │ │
  │ │ (Container) │ │ │ │
  │ │ │
  └─► Overlay Network └─► Overlay Network └─► Overlay Network
  (swarm-net) (swarm-net) (swarm-net)

### Node Management

- `docker node --help` - Show node command help (manager only)
- `docker node ls` - List all nodes in swarm (manager only)
- `docker node update --role manager <node_name>` - Promote a worker node to manager
- `docker node update --role worker <node_name>` - Demote a manager node to worker
- `docker node update --availability active|pause|drain <node_name>` - Change node availability state
- `docker node inspect <node_name>` - View detailed node information
- `docker swarm join-token manager` - Display the join token for adding manager nodes to the swarm

### Service Management

- `docker service --help` - Show service commands (swarm replacement for docker run). service shows the main service, while container show individual containers
- `docker service create alpine ping 8.8.8.8` - Create a new service running alpine container pinging Google DNS
- `docker service ls` - List all services in the swarm
- `docker service update --help` - Show service update options
- `docker service update <service_name> --replicas <number>` - Scale service to specified number of replicas
- `docker service rm <service_name>` - Remove the service and all its containers

### Task/Container Management (Lowest Level)

- `docker service ps <service_name>` - Show tasks/containers in the service
- `docker container ls` - List all containers running on this node
- `docker update --help` - Show container update options
- `docker container rm -f <container_name>.<task_number>` - Force remove a specific task container

## Creating a 3-Node Swarm Cluster

http://play-with-docker.com

docker info

docker-machine

docker-machine create node1

docker-machine ssh node1

docker-machine env node1

docker info

http://get.docker.com

docker swarm init

docker swarm init --advertise-addr TAB COMPLETION

docker node ls

docker node update --role manager node2

docker node ls

docker swarm join-token manager

docker node ls

docker service create --replicas 3 alpine ping 8.8.8.8

docker service ls

docker node ps

docker node ps node2

docker service ps sleepy_brown

## Scaling Out with Overlay Networking

docker network create --driver overlay mydrupal

docker network ls

docker service create --name psql --netowrk mydrupal -e POSTGRES_PASSWORD=mypass postgres

docker service ls

docker service ps psql

docker container logs psql TAB COMPLETION

docker service create --name drupal --network mydrupal -p 80:80 drupal

docker service ls

watch docker service ls

docker service ps drupal

docker service inspect drupal
