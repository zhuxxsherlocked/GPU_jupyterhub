# GPU-jupyterhub

This jupyterhub implementation allows for Nvidia GPU access using the nvidia-docker-2 container runtime.

## Requirements

- A cuda driver must be installed on the host system, you can check this by running `nvidia-smi` in the terminal.
- Docker 18.09.5 or higher.
- Docker compose 1.25.5 or higher.
I've personally found the [DigitalOcean Tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04) to be the most reliable. Make sure to change the version number to `1.25.5`!
- The nvidia-container-runtime needs to be installed:
```bash
sudo apt-get install nvidia-container-runtime
```
- Nvidia docker2 needs to be installed see their [Github](https://github.com/NVIDIA/nvidia-docker) for instructions.


## Installation

### Preparation

To make `runtime: nvidia` work we need to change our `/etc/docker/daemon.json` to the following:
```json
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}

```

### Build the Image

Build and tag the image using the `Dockerfile` in this directory.

```bash
#cd notebooks/{notebook-folder}
#docker build -t {notebook-folder-name} .

# Example
cd notebooks/base-notebook
docker build -t "base-notebook" .

cd ..
cd notebooks/minimal-notebook
docker build -t "minimal-notebook" .

# And so on
```

### Building the hub

**Note:** Make sure to change the `userlist` file to include your Github username.

```bash
# Make sure to do this in the root of the repo*
docker-compose up --build
```

## Run JupyterHub Container

Then run the following **from the root directory** of this repository:

```
# bring down the JupyterHub container, if running
docker-compose down

# bring it back up
docker-compose up -d
```

### Common Issues
- Volume `jupyterhub-db-data` or `jupyterhub-data` not found.
```bash
docker volume create --name="jupyterhub-db-data"
docker volume create --name="jupyterhub-data"
```
- Network `jupyterhub-network` not found.
```bash
docker network create "jupyterhub-network"
```

## Reference
* [FAU-DLM/GPU-Jupyterhub](https://github.com/FAU-DLM/GPU-Jupyterhub)
* [selenecodes/GPU-jupyterhub](https://github.com/selenecodes/GPU-jupyterhub)
* [jupyterhub-deploy-docker](https://github.com/jupyterhub/jupyterhub-deploy-docker)