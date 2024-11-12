#### Docker File

> Docker is so good, docker file syntax became standard for container config [opencontainers](https://opencontainers.org)
#### DockerIgnore
#### Layering
#### Volumes
```bash
sudo docker run --name sampleapp_1 -v .:/app -v node_modules/  -p 3232:3232 sampleapp
```
#### Pruning