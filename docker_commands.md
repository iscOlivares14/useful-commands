## Docker commands

### build

Once you have defined a [Dockerfile](https://docs.docker.com/build/guide/intro/#the-dockerfile) you can build your image as follows:

```bash
$ docker build -t <image-name> <dockerfile-path>

# Dockerfile is in the directory from where we executes the command
$ docker build -t reporeporter .

# dockerfile named different than default
$ docker build --file server.Dockerfile --tag first-server .
```

### run

Run a container in detached mode (running in background)
```bash
$ docker run -d  <image-name>
```

Run the image adding a mount pointed to a local directory
```bash
$ docker run -v <local-directory-path>:<container-directory-path> <image-name>

$ docker run -v $(pwd)/tmp:/tmp reporeporter
# this will persist the files and you could see it in your local
$ docker run --rm --entrypoint sh -v /tmp/container:/tmp ubuntu -c "echo 'Hello there.' > /tmp/file && cat /tmp/file"
```

Binds local port to container ports
```bash
$ docker run -d -p <host_port>:<container_port> <image-name>

$ docker run -d -p 5001:5000 first-web-server
```

### stop

Simply gracefully stop the container without removing it
```bash
$ docker stop <Container_ID>

# indicating termination of the container
$ docker stp -t 0h <Container_ID>
```

### kill

Kill one or more running containers
```bash
$ docker kill <Image_ID>
```

### rm & rmi

Remove all stopped containers
```bash

$ docker ps -aq | xargs docker rm
```

### tags

This movements

```bash
$ docker tag <image-name> <DockerHub_ID>/<container_name>:<container__tags>

$ docker tag first-web-server veolivaresr/first-web-server:0.0.1
```


