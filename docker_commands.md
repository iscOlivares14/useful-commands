### Docker commands

Once you have defined a [Dockerfile](https://docs.docker.com/build/guide/intro/#the-dockerfile) you can build your image as follows:

```bash
docker build -t <image-name> <dockerfile-path>

# Dockerfile is in the directory from where we executes the command
docker build -t reporeporter .

# dockerfile named different than default
docker build --file server.Dockerfile --tag first-server .
```

Run the image adding a mount pointed to a local directory

```bash
> docker run -v <local-directory-path>:<container-directory-path> <image-name>

> docker run -v $(pwd)/tmp:/tmp reporeporter
```


```bash
> docker kill <Image_ID>
```

Run a container in detached mode (running in background)

```bash
# run a container in dettached mode
> docker run -d  <image-name>
```