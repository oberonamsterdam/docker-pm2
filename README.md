Based on https://hub.docker.com/r/keymetrics/pm2/

# Adding git to image

Since we run the source inside out PM2 containers, we need git+ssh to update inside the container.
Mount your own .ssh/id_rsa file so the container can use your own server/workstation credentials.

Inside the start.sh you can like, do the following things:
* update code
* install dependancies
* compile code
* start pm2 app

# Example docker-compose.yml
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

# Example start.sh
```
#!/usr/bin/env sh

while [ ! -f /app/deployed.lock ]
do
  sleep 2
done

pm2-runtime /app/app.json

```

# Example deploy.sh
```
#!/usr/bin/env sh

echo "We're deploying..."

git pull

$(which npm) install --frozen-lockfile
$(which npm) run build

touch deployed.lock

pm2 reload my-app

echo ""
echo "!!! Deploy finished !!!"

```

This example will:
1. Start the container
2. The start.sh will loop untill the deployed.lock file is written
3. We execute the deploy.sh inside the container and write the deployed.lock file
4. The start.sh will continue and start pm2-runtime

OR:

1. Change the entrypoint to "pm2-runtime app/app/json" for immediate starting of the application and not wait for the build/deployment