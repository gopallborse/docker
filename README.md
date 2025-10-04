# Commands in the Dockerfile

RUN npm install <!-- This and other commands are for setting up the image only. Will be executed whenever the image is being built. -->
RUN node server.js <!-- This is incorrect as it will try to start the server in the image itself. -->
CMD ["node", "server.js"] <!-- Correct way. Will not be executed when the image is being built, but when a container is started based on the image. -->

EXPOSE 80 <!-- This port expose is just for documentation purpose. Doesn't do much. -->
docker run -p 3000:80 <image-id>

<!-- docker uses layer based architecture. So all the instructions in the dockerfile are executed layer by layer. If nothing changes, it uses previous layers from cache. If one layer changes, all the subsequent layers also re-executed. -->

# docker commands

docker build . <!-- build a new image from dockerfile -->
docker build -t goals:latest . <!-- build a new image from dockerfile with a custom tag -->
docker build -t feedback-node:dev --build-arg DEFAULT_PORT=8000 <!-- uses ARG DEFAULT_PORT=80 from dockerfile if not provided in CLI -->

<!-- default -- (run - new container in attached mode), (start - existing container in detached mode) -->

docker run -p 3000<Ext>:3000<Int> <image-id> <!-- # runs in attached mode, runs in foreground, blocks the terminal -->
docker run -p 3000<Ext>:3000<Int> -d <image-id> <!-- # runs in detached mode, runs in background, doesn't block the terminal -->
docker run -p 3000:80 -d --rm <image-id> <!-- --rm flag removes the container as soon as the container is stopped -->
docker run -p 3000:80 -d --rm --name goalsapp <image-id> <!-- name a container -->

docker start <container-name> <!-- runs in detached mode, runs in background, doesn't block the terminal -->
docker start -a <container-name> <!-- runs in attached mode, runs in foreground, blocks the terminal -->

docker attach <container-name> <!-- get terminal attached to a running detached container -->
docker logs <container-name> <!-- get existing/past logs from the container -->

docker images <!-- list all images -->
docker ps <!-- list all containers -->

docker run -it node
docker run -it <image-id>

docker stop <container-name>

docker rmi <image-id> <!-- remove image to which no container exists, either running or stopped. So first remove all the containers created from an image, only then remove the image -->
docker rm <container-name> <container-name> <container-name> <!-- remove multiple stopped containers -->

docker container prune <!-- remove all containers at once -->
docker image prune <!-- removes all images which have no tag -->
docker image prune -a <!-- remove all images including which have tag -->

docker image inspect <image-id> <!-- information about the image -->

docker cp <!--source --> dummy/. <!-- destination --> <container-name>:/test <!-- copy content to container -->
docker cp <!-- source --> <container-name>:/test <!-- destination --> dummy/. <!-- copy content from container -->

<!-- tag for image and name for container -->

docker tag node-demo:latest gopallborse/node-hello-world <!-- renaming an existing image -->

<!-- sharing docker images, the image name should be same as the repository name -->

docker login --->>> enter username and password
docker push gopallborse/node-hello-world
docker pull gopallborse/node-hello-world

# docker volumes

<!-- Anonymous volumes are removed automatically, when a container is removed.
This happens when you start / run a container with the --rm option.
If we start a container without this option, the anonymous volume would NOT be removed, even if we remove the container (with docker rm ...).
Still, if you then re-create and re-run the container (i.e. you run docker run ... again), a new anonymous volume will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).
Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or docker volume prune. -->

docker volume ls <!-- list all the volumes -->
docker volume create feedback-files <!-- create custom volumes manually -->

<!-- named volumes cannot be created through dockerfile. they need to be created through cli while running the container -->
docker run -d -p 3000:80 --rm --name feedback-app -v feedback<!-- stored under this name -->:/app/feedback<!-- path in the container --> feedback-node:volumes

docker volume rm <volume-id> <!-- remove unnamed unused volume -->
docker volume prune <!-- remove all unnamed unused volumes -->

<!-- ro flag in the bind mount is to specify for read only, as we want to make bind mounts read only, the container should be able to only read data from the bind mount, and not write -->
<!-- volumes with deep path (../../../) overwrite and take precedence, e.g. even though we have the read only bind mount down here, still /app/temp and /app/node_modules are not read only, it should be configured in the CLI and not in dockerfile-->
docker run -d --rm
-p 3000:80
--env PORT=80 <!-- or -e PORT=80 --> <!-- add multiple --env or -e for multiple environment variables -->
--env-file ./.env <!-- for configuring the .env file -->
--name feedback-app
-v feedback:/app/feedback
-v "D:\WebDev\Docker & Kubernetes The Practical Guide [2025 Edition]:/app:ro"
-v /app/node_modules
-v /app/temp
feedback-node:volumes

<!-- If we don't always want to copy and use the full path, we can use the shortcut: -v "%cd%":/app -->

# docker network

docker network ls
docker network create <network name>
docker network rm <network name>

<!-- Docker Networks support different kinds of "Drivers" which influence the behavior of the Network.
The default driver is the "bridge" driver - it provides the behavior i.e. containers can find each other by name, if they are in the same Network.
The driver can be set when a Network is created, simply by adding the --driver option.
Of course, if we want to use the "bridge" driver, we can simply omit the entire option, since "bridge" is the default anyways.
Docker also supports alternative drivers (e.g. host, overlay, macvlan, Third-party plugins, none), but we will use the "bridge" driver in most cases as it makes most sense in the vast majority of scenarios. -->

docker network create --driver bridge <network name>

# Multi-container app CLI commands

## Create network
docker network create goals-net

## For MongoDB database container
docker run -d --rm
--name mongodb
--network goals-net
-e MONGO_INITDB_ROOT_USERNAME=gopallborse
-e MONGO_INITDB_ROOT_PASSWORD=secret
-e MONGO_INITDB_DATABASE=course-goals
-v data:/data/db
mongo

## For NodeJS backend container
docker build -t goals-node .

docker run -d --rm
--name goals-backend
--network goals-net
-e MONGODB_USERNAME=gopallborse
-e MONGODB_PASSWORD=secret
-e MONGODB_DATABASE=course-goals
-v "D:/WebDev/Docker & Kubernetes The Practical Guide [2025 Edition]/07-multi-container-app/backend:/app"
-v logs:/app/logs
-v /app/node_modules
-p 80:80
goals-node

## For React frontend container
docker build -t goals-react .

docker run -d --rm
--name goals-frontend
-v "D:/WebDev/Docker & Kubernetes The Practical Guide [2025 Edition]/07-multi-container-app/frontend/src:/app/src"
-p 3000:3000
goals-react

# Docker Compose commands
docker-compose up
docker-compose up -d
docker-compose up --build <!-- force building images every time -->
docker-compose build <!-- if we want to only build the images, and not start the containers -->

docker-compose down
docker-compose down -v <!-- to delete volumes as well -->