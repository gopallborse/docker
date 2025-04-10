# docker commands

docker build .

docker run -p 3000<Out>:3000<In> <image-id> <!-- # runs in attached mode, runs in foreground, blocks the terminal -->

docker run -p 3000<Out>:3000<In> -d <image-id> <!-- # runs in detached mode, runs in background, doesn't block the terminal -->

docker container attach <container-name> <!-- attach to a detached container -->

docker ps

docker start <container-name> <!-- runs in detached mode, runs in background, doesn't block the terminal -->
docker start -a <container-name> <!-- runs in attached mode, runs in foreground, blocks the terminal -->

docker stop <container-name>

docker run -it node
docker run -it <image-id>