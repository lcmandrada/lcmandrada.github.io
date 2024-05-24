# Container

## Docker

[Setup](https://docs.docker.com/desktop/windows/wsl/) 
[Best Practices](https://testdriven.io/blog/docker-best-practices) 

[Dockerfile with Poetry](https://github.com/python-poetry/poetry/issues/1178) 
- no need for a requirements file
- the virtualenv is managed by Poetry
- no Poetry in the final image
- application and venv contained in one folder
- Python application cannot write to its files or the virtualenv

[Dockerfile Templates](https://www.mktr.ai/the-data-scientists-quick-guide-to-dockerfiles-with-examples/) 

**Commands**
```sh
docker build . -t tag:latest

docker images
docker run -it image_id

# overrides CMD
docker run image_id uvicorn config.asgi
# overrides ENTRYPOINT
docker run -entrypoint uvicorn config.asgi image_id

# run a terminal inside container
docker run -it -entrypoint /bin/sh image_id

docker ps
docker ps -a

docker stop container_id
docker kill container_id

docker rm container_id
docker rmi image_id

# clean resources
docker image prune
docker rm $(docker ps -a -f status=exited -q)
docker volume prune

# dangling only
docker system prune
# includes unused
docker system prune -a
```


## Kubectl

### Logs
```bash
k logs -f deploy/<deploy>
k logs -fl app=<label>
```

### Logs of previous deploy
```bash
kubectl logs podname -c containername --previous

k logs deploy/<deploy> --previous
```

### Execute into a pod
```bash
k get pods
k exec -ti <pod> -- sh
```

### Run cronjob as job
```bash
k create job --from=cj/<cronjob> <name>
```

### Get pods and sort by memory
```bash
k top pod --sort-by=memory
```
