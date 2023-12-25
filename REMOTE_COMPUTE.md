# Remote Compute Docker Usage

The Dockerfile file uses multiple build stages to support 4 different applications CPU-Only, NVIDIA-GPU, Intel-GPU, and AMD-GPU.

## CPU-Only

Docker Run Example:

`docker run --rm -it -p 11989:11989 ghcr.io/madmax43v3r/chia-recompute:latest`

Docker Compose Example:
```yml
version: '3'
services:
  chia:
    image: ghcr.io/madmax43v3r/chia-recompute:latest
    restart: unless-stopped
    ports:
      - "11989:11989"
    environment:
      TZ: 'America/Chicago'
#      CHIA_RECOMPUTE_PORT: 12000
### Enable Proxy Mode ###
#      CHIA_RECOMPUTE_PROXY: 'true'
#      CHIA_RECOMPUTE_NODES: 192.168.1.1,192.168.1.2:11990  
```
## NVIDIA-GPU

Docker Run Example:

`docker run --rm -it --runtime=nvidia -p 11989:11989 ghcr.io/madmax43v3r/chia-recompute:latest-nvidia`

Docker Compose Example:
```yml
version: '3'
services:
  chia:
    image: ghcr.io/madmax43v3r/chia-recompute:latest-nvidia
    restart: unless-stopped
    runtime: nvidia
    ports:
      - "11989:11989"
    environment:
      TZ: 'America/Chicago'
#      CHIA_RECOMPUTE_PORT: 12000
### Enable Proxy Mode ###
#      CHIA_RECOMPUTE_PROXY: 'true'
#      CHIA_RECOMPUTE_NODES: 192.168.1.1,192.168.1.2:11990
### GPU Specific Options ###
#      NVIDIA_VISIBLE_DEVICES: 0,3
```
Note: for nvidia you also need the `NVIDIA Container Toolkit` installed on the host, for more info please see: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

## Intel-GPU

Docker Run Example:

`docker run --rm -it --device=/dev/dri -p 11989:11989 ghcr.io/madmax43v3r/chia-recompute:latest-intel`

Docker Compose Example:
```yml
version: '3'
services:
  chia:
    image: ghcr.io/madmax43v3r/chia-recompute:latest-intel
    restart: unless-stopped
    devices:
      - /dev/dri:/dev/dri
    ports:
      - "11989:11989"
    environment:
      TZ: 'America/Chicago'
#      CHIA_RECOMPUTE_PORT: 12000
### Enable Proxy Mode ###
#      CHIA_RECOMPUTE_PROXY: 'true'
#      CHIA_RECOMPUTE_NODES: 192.168.1.1,192.168.1.2:11990
### GPU Specific Options ###
#      CHIAPOS_MAX_OPENCL_DEVICES: 0
```
Note: for ARC GPU's you will need to be running kernel 6.2+ on your docker host

## AMD-GPU

Docker Run Example:

`docker run --rm -it --device=/dev/kfd --device=/dev/dri -p 11989:11989 ghcr.io/madmax43v3r/chia-recompute:latest-amd`

Docker Compose Example:
```yml
version: '3'
services:
  chia:
    image: ghcr.io/madmax43v3r/chia-recompute:latest-amd
    restart: unless-stopped
    devices:
      - /dev/dri:/dev/dri
      - /dev/kfd:/dev/kfd
    ports:
      - "11989:11989"
    environment:
      TZ: 'America/Chicago'
#      CHIA_RECOMPUTE_PORT: 12000
### Enable Proxy Mode ###
#      CHIA_RECOMPUTE_PROXY: 'true'
#      CHIA_RECOMPUTE_NODES: 192.168.1.1,192.168.1.2:11990
### GPU Specific Options ###
#      CHIAPOS_MAX_OPENCL_DEVICES: 0
```
