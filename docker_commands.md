# Docker handling

## Operational Commands

### List docker items

Images

```bash
docker image ls
```

### build

Once you have defined a [Dockerfile](https://docs.docker.com/build/guide/intro/#the-dockerfile) you can build your image as follows:

```bash
docker build -t <image-name> <dockerfile-path>
# Dockerfile is in the directory from where we executes the command
docker build -t reporeporter .
# dockerfile named different than default
docker build --file server.Dockerfile --tag first-server .
```

### pull

Pull objects from public and private sources as standar but also using tags.

```bash
docker pull <image-name>
docker pull <image-name>:<tag>
```

### run

Run a container in detached mode (running in background)

```bash
docker run -d  <image-name>
# interactive mode and telnet open
docker run -it  <image-name>
# naming the container
docker run --name <container-name> -d  <image-name>
```

Run the image adding a mount pointed to a local directory

```bash
docker run -v <local-directory-path>:<container-directory-path> <image-name>

docker run -v $(pwd)/tmp:/tmp reporeporter
# this will persist the files and you could see it in your local
docker run --rm --entrypoint sh -v /tmp/container:/tmp ubuntu -c "echo 'Hello there.' > /tmp/file && cat /tmp/file"
```

Binds local port to container ports

```bash
docker run -d -p <host_port>:<container_port> <image-name>

docker run -d -p 5001:5000 first-web-server
```

Running the cointainers with mounted volumes

```bash
docker run -it --name <container-name> --mount type=tmpfs, target=/tmpfs-mount <image-name> <command>
docker run -it --name tmpfs-test --mount type=tmpfs, target=/tmpfs-mount ubuntu bash

docker run -it --name <container-name> --mount type=tmpfs, target=/tmpfs-mount <image-name> <command>
docker run -it --name tmpfs-test --mount type=tmpfs, target=/tmpfs-mount ubuntu bash
```

### stop

Simply gracefully stop the container without removing it

```bash
docker stop <Container_ID>

# indicating termination of the container
docker stop -t 0h <Container_ID>
```

### kill

Kill one or more running containers

```bash
docker kill <Image_ID>
```

### rm & rmi

Remove a container by Id

```bash
docker rm <CONTAINER_ID>
```

Remove all stopped containers

```bash
docker ps -aq | xargs docker rm
```

Remove images

```bash
docker rmi <containers-name>
```

### tags

```bash
docker tag <image_id> <tag-name>
# change tag for an image pointing it to a dockerHUb account
docker tag <image-name> <DockerHub_ID>/<container_name>:<container_tags>

docker tag first-web-server veolivaresr/first-web-server:0.0.1
```

### push

```bash
docker push   veolivaresr/first-web-server:0.0.1
```

### prune

This command is usefull whe you look for freeing up disk space.

```bash
docker system prune
```

## Debugging commands

### stats

```bash
docker stats
```

### top

```bash
docker top
```

### inspect

```bash
# for checking env, cmd and Layers properties
docker image inspect <image_name>

# look for environment variables
docker image inspect <image_name> | jq .[].Config.Env
docker image inspect <image_name> | jq .[].Config.Cmd
docker image inspect <image_name> | jq .[].RootFS.Layers

docker inspect <container-name>

# look for mounts
docker inspect <container-name> | jq .[0].Mounts
# for better visualization use less
docker inspect <container-name> | less
```

## Best practices

- Always use verified images
- Avoid tagging as latest
- Use non-root users
- Containers should be ephemeral
- Keep the build context minimal
- Use multi-stage builds
- Skip unwanted packages
- Minimize the number of layers