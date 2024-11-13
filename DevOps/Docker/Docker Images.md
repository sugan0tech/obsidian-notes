# Docker Images: The Foundation of Containers 🐳

A **Docker Image** is an ordered collection of **root filesystem changes** and **execution parameters** used by the container runtime. It’s essentially a blueprint that defines:

- **Application Binaries and Dependencies**: Contains all the required binaries, libraries, and dependencies for the application to run.
- **Metadata**: Includes information about the image data and how the image should be executed.

### Key Components of a Docker Image

| Component                 | Description                                                        |
|---------------------------|--------------------------------------------------------------------|
| **Root Filesystem**       | Collection of files, libraries, and binaries needed for the app.  |
| **Execution Parameters**  | Instructions on how to run the image (e.g., default command).     |
| **Metadata**              | Contains details like environment variables, ports, and volumes.  |

---

### Image Layers 🧱

Docker images are built in **layers** using a **union file system**. Each instruction in a Dockerfile creates a new layer on top of the previous one, enabling:

- **Efficient Storage**: Layers are shared among images, so unchanged layers are reused across images.
- **Faster Builds**: Layers that haven't changed are cached, improving build times.

To view the layer history of an image, including the size change and commands used:

```bash
docker history <image>:<tag>
```

For example:
```bash
docker history nginx:latest
```

| Command                                      | Description                           |
|----------------------------------------------|---------------------------------------|
| `docker history <image>:<tag>`               | Shows image layer history             |
| **Example Output**: `nginx:latest`           | Lists layer sizes and commands        |

> Also in docker file each ==RUN== stands on it's own layer, so grouping them into a layer will be better of practice
---

### Inspecting Docker Images

The `docker image inspect` command provides detailed information about an image, including configuration, layers, and environment variables.

#### Example Command:

```bash
docker image inspect nginx
```

**Output Details**:

| Field                 | Description                                                 |
|-----------------------|-------------------------------------------------------------|
| **Id**                | Unique identifier for the image                             |
| **RepoTags**          | Tags associated with the image (e.g., `nginx:latest`)       |
| **Created**           | Timestamp of image creation                                 |
| **Size**              | Total image size                                            |
| **RootFS**            | List of layers in the image, with unique layer IDs          |
| **Config**            | Default execution parameters (e.g., command, environment)   |

---

### Common Docker Image Commands

- **View Layer History**:
  ```bash
  docker history <image>:<tag>
  ```
- **Inspect Image Details**:
  ```bash
  docker image inspect <image>
  ```

---

Docker images, through **layered architecture** and efficient storage with **union file systems**, provide the backbone for containers. Using commands like `docker history` and `docker image inspect` allows for detailed insights into how images are built and managed.


### dockerfile
```dockerfile
# NOTE: this example is taken from the default Dockerfile for the official nginx Docker Hub Repo
# https://hub.docker.com/_/nginx/
# NOTE: This file is slightly different than the video, because nginx versions have been updated 
#       to match the latest standards from docker hub... but it's doing the same thing as the video
#       describes
FROM debian:stretch-slim
# all images must have a FROM
# usually from a minimal Linux distribution like debian or (even better) alpine
# if you truly want to start with an empty container, use FROM scratch

ENV NGINX_VERSION 1.13.6-1~stretch
ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
# optional environment variable that's used in later lines and set as envvar when container is running

# RUN script will be executed inside of container
RUN apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
	&& \
	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
	found=''; \
	for server in \
		ha.pool.sks-keyservers.net \
		hkp://keyserver.ubuntu.com:80 \
		hkp://p80.pool.sks-keyservers.net:80 \
		pgp.mit.edu \
	; do \
		echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
	done; \
	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \
	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y \
						nginx=${NGINX_VERSION} \
						nginx-module-xslt=${NGINX_VERSION} \
						nginx-module-geoip=${NGINX_VERSION} \
						nginx-module-image-filter=${NGINX_VERSION} \
						nginx-module-njs=${NJS_VERSION} \
						gettext-base \
	&& rm -rf /var/lib/apt/lists/*
# optional commands to run at shell inside container at build time
# this one adds package repo for nginx from nginx.org and installs it

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log
# forward request and error logs to docker log collector
# docker has the log colleting feature

EXPOSE 80 443
# expose these ports on the docker virtual network
# you still need to use -p or -P to open/forward these ports on host

CMD ["nginx", "-g", "daemon off;"]
# required: run this command when container is launched
# only one CMD allowed, so if there are multiple, last one wins
```