# kaniko-project

kaniko-project mirrors

### Docker Image

```bash
# executor
chinayin/kaniko-project:latest
chinayin/kaniko-project:executor
# debug
chinayin/kaniko-project:debug
# slim
chinayin/kaniko-project:slim
```

### Using Standard Input

If running kaniko and using Standard Input build context, you will need to add the docker or kubernetes -i,
--interactive flag. Once running, kaniko will then get the data from STDIN and create the build context as a compressed
tar. It will then unpack the compressed tar of the build context before starting the image build. If no data is piped
during the interactive run, you will need to send the EOF signal by yourself by pressing Ctrl+D.

Complete example of how to interactively run kaniko with .tar.gz Standard Input data, using docker:

```bash
echo -e 'FROM alpine \nRUN echo "created from standard input"' > Dockerfile | tar -cf - Dockerfile | gzip -9 | docker
run \
--interactive -v $(pwd):/workspace chinayin/kaniko-project:latest \
--context tar://stdin \
--destination=<gcr.io/$project/$image:$tag>#
```

### Debug Image

The kaniko executor image is based on scratch and doesn't contain a shell. We provide gcr.io/kaniko-project/executor:
debug, a debug image which consists of the kaniko executor image along with a busybox shell to enter.

You can launch the debug image with a shell entrypoint:

```bash
docker run -it --entrypoint=/busybox/sh chinayin/kaniko-project:debug
```
