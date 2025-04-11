# docker commands

docker build .

docker build -t goals:latest .

docker run -p 3000<Out>:3000<In> <image-id> <!-- # runs in attached mode, runs in foreground, blocks the terminal -->

docker run -p 3000<Out>:3000<In> -d <image-id> <!-- # runs in detached mode, runs in background, doesn't block the terminal -->

docker run -p 3000:80 -d --rm <image-id> <!-- --rm flag removes the container as soon as the container is stopped -->

docker run -p 3000:80 -d --rm --name goalsapp <image-id> <!-- name a container -->

docker container attach <container-name> <!-- attach to a detached container -->

docker ps <!-- list all containers -->
docker images <!-- list all images -->

docker start <container-name> <!-- runs in detached mode, runs in background, doesn't block the terminal -->
docker start -a <container-name> <!-- runs in attached mode, runs in foreground, blocks the terminal -->

docker run -it node
docker run -it <image-id>

docker stop <container-name>

docker rm <container-name> <container-name> <container-name> <!-- remove multiple containers -->
docker rmi <image-id> <!-- remove image -->

docker container prune <!-- remove all containers at once -->
docker image prune <!-- removes all images which have no tag -->
docker image prune -a <!-- remove all images including which have tag -->

docker image inspect <image-id>

docker cp <source>dummy/. <destination><container-name>:/test <!-- copy content to container -->

docker cp <source><container-name>:/test <destination>dummy/. <!-- copy content from container -->

docker tag node-demo:latest gopallborse/node-hello-world <!-- renaming an existing image -->

docker push gopallborse/node-hello-world