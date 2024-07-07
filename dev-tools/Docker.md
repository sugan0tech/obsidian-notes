#### Docker File
#### DockerIgnore
#### Layering
#### Volumes
```bash
sudo docker run --name sampleapp_1 -v .:/app -v node_modules/  -p 3232:3232 sampleapp
```
#### Pruning