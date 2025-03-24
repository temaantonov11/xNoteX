
## CUSTOM HTML PAGE

```shell
docker run \
-d \
--rm \
--name chtml \
-v /home/temaantonov11/docker_/nginx:/usr/share/docker/ \
-p 2077:80 \
nginx
```

## BUILDING ENVIRONMENT

```dockerfile
FROM ubuntu:22.04
RUN apt-get update && \
    apt-get install -y \
        python3 \
        python3-pip \
        ninja-build \
        pkg-config \
        nasm \
        g++\
        gcc \
        git
RUN pip3 install meson
```



```shell
 docker run -v ${PWD}:/building-sys/builddir openh264-build:v4
```

`pwd = /home/temaantonov11/docker_/openh264/build`
`/h264/builddir`

#### Openh264
https://github.com/cisco/openh264.git
#### Building

```shell
meson setup builddir openh264
ninja -C builddir
```

