Based on https://hub.docker.com/r/keymetrics/pm2/

# Adding git to image

Since we run the source inside out PM2 containers, we need git+ssh to update inside the container.
Mount your own .ssh/id_rsa file so the container can use your own server/workstation credentials.

Inside the start.sh you can like, do the following things:
* update code
* install dependancies
* compile code
* start pm2 app

# Example
```
version: '3'
services:
    web:
        container_name: "my-pm2-container"
        image: oberonamsterdam/pm2-git:8-alpine
        restart: always
        network_mode: "bridge"
        volumes:
            - .:/app/:delegated
            - $HOME/.ssh/known_hosts:/root/.ssh/known_hosts
            - $HOME/.ssh/id_rsa:/root/.ssh/id_rsa
        entrypoint: ["/app/start.sh"]
```