# Persist data in Weaviate

## (1) CHECKLIST

### (1.a.) Main checklist 
- [ ] ```docker-compose.yml```: Configure the docker compose file. Reference: https://weaviate.io/developers/weaviate/installation/docker-compose
- [ ] ```sudo systemctl status docker```: Check if docker is running.
- [ ] ```sudo systemctl enable docker```: Enable docker to start automatically when we start the server.
- [ ] ```sudo systemctl start docker```: Start docker.
- [ ] ```sudo systemctl restart docker```: Restart docker.
- [ ] Important: Go to the folder where the docker compose file is located.
- [ ] ```$ docker-compose up -d```: Run docker compose file in detached mode.
- [ ] How do I know when Weaviate is up and ready? https://weaviate.io/blog/docker-and-containers-with-weaviate#what-do-i-need-to-install-before-running-weaviate
- [ ] ```$ docker-compose down```: Make sure to use when shutting down. This writes all the files from memory to disk.
- [ ] Logs: 
- [ ] Monitoring: 

### (1.b.) 2 important configurations:
- [ ] ```sudo systemctl enable docker```: Enable docker to start automatically when we start the server.
- [ ] ```restart: always```: In the docker compose file, make sure to add this line to the weaviate service. This ensures that weaviate restarts automatically when the server restarts.

To ensure that a service defined in Docker Compose is automatically restarted if it crashes or closes unexpectedly, you can use the restart configuration option in the Compose file. The restart option allows you to specify the restart policy for each service.

In your docker-compose.yml file, for the specific service you want to automatically restart, you can add the restart option and set it to the desired restart policy. 

```
version: '3'
services:
  myservice:
    image: myimage:latest
    restart: always
```
In the above example, the myservice container will always be automatically restarted if it crashes or exits for any reason.

There are different restart policies you can use:

- ```no```: Do not automatically restart the container. This is the default if the restart option is not specified.
- ```always```: Always restart the container regardless of the exit status.
- ```on-failure```: Restart the container only if it exits with a non-zero exit status.
- ```unless-stopped```: Always restart the container unless it is explicitly stopped by the user.
You can choose the appropriate restart policy based on your specific requirements.


## (2) Docker compose file

Reference: https://weaviate.io/developers/weaviate/installation/docker-compose#persistent-volume 

### About the volumes

```/var/weaviate``` is the location where you want to store the data on the local machine
```/var/lib/weaviate``` (after the colon) is the location inside the container, don't change this
About the hostname
The ```CLUSTER_HOSTNAME``` can be any arbitrarily chosen name

Configuration of environment variables that can be set are: https://weaviate.io/developers/weaviate/config-refs/env-vars

```
services:
  weaviate:
    volumes:
      - /var/weaviate:/var/lib/weaviate
    environment:
      CLUSTER_HOSTNAME: 'node1'
```

### Complete working example of a docker compose file

```
---
version: '3.4'
services:
  weaviate:
    volumes:
    - /Users/home/Projects/xyz/weaviate_data:/var/lib/weaviate
    command:
    - --host
    - 0.0.0.0
    - --port
    - '8080'
    - --scheme
    - http
    image: semitechnologies/weaviate:1.19.1
    ports:
    - 8080:8080
    restart: on-failure:0
    environment:
      IMAGE_INFERENCE_API: 'http://i2v-neural:8080'
      OPENAI_APIKEY: $OPENAI_APIKEY
      QUERY_DEFAULTS_LIMIT: 25
      AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED: 'true'
      PERSISTENCE_DATA_PATH: '/var/lib/weaviate'
      DEFAULT_VECTORIZER_MODULE: 'text2vec-openai'
      ENABLE_MODULES: 'text2vec-openai,img2vec-neural'
      CLUSTER_HOSTNAME: 'node1'
      AUTHENTICATION_APIKEY_ENABLED: 'true'
      AUTHENTICATION_APIKEY_ALLOWED_KEYS: 'xyz, xyz'
      AUTHENTICATION_APIKEY_USERS: 'Prewar, test'
      AUTHORIZATION_ADMINLIST_ENABLED: 'true'
      AUTHORIZATION_ADMINLIST_USERS: 'test'
      AUTHORIZATION_ADMINLIST_READONLY_USERS: 'test'
  i2v-neural:
    image: semitechnologies/img2vec-pytorch:resnet50
    environment:
      ENABLE_CUDA: '0'
...
```

## (3) FAQ

Q: What happens when the Weaviate Docker container restarts? Is my data in the Weaviate database lost?

Reference: https://weaviate.io/developers/weaviate/more-resources/faq#q-what-happens-when-the-weaviate-docker-container-restarts-is-my-data-in-the-weaviate-database-lost

A: There are three levels:

- You have no volume configured (the default in our docker-compose files), if the container restarts (e.g. due to a crash, or because of docker stop/start) your data is kept
- You have no volume configured (the default in our docker-compose files), if the container is removed (e.g. from docker-compose down or docker rm) your data is gone
- If a volume is configured, your data is persisted regardless of what happens to the container. They can be completely removed or replaced, next time they start up with a volume, all your data will be there