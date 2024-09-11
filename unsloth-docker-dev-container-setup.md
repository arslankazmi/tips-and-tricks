# Setup Unsloth Development Environment Using a Docker Image and a VSCode Dev Container

## Why

Setting up unsloth via a traditional python virtual environment can be a pain in the ass as the dependencies have to be installed in a very specific order depending on what version of CUDA and PyTorch you want tor run. There are issues with using PyTroch 2.4 right not, and the best combo seems to be CUDA 12.1 and PyTorch 2.3.1.


## Pre-Requisites

- Ubuntu 22.04
- Docker
- VSCode
- VSCode Docker Plugin
- VSCode Dev Container Pluging
- VSCode jupyter plugin
- VSCode python plugin
- Nvidia Container Toolkit

## Installing Pre-Reqs ( Skip if not needed )

### Docker

```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```shell
sudo groupadd docker
sudo usermod -aG docker $USER
```

Logout, then log back in to re-apply new user permissions.

Verify that docker command runs without sudo:

```
docker ps
```

### Nvidia Container Toolkit

```shell
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

```shell
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
```

## Building Docker Container

I found this docker container provided by zytedata. It's the most recently updated one (in the last three weeks at the time of writing).

The idea is to have unsloth installed in the exact correct environment, with the exact dependencies, in the exact order.

```shell
git clone https://github.com/zytedata/unsloth_docker.git
cd unsloth docker
```

```shell
docker build -t unsloth .
```

Building the image will take a while, as it has to pull full installations of CUDA and Pytorch as well as all dependencies. In my experience th amount of storage it eventually ends up using is around 30GB, so make sure you have enough disk space.

Once complete, we can run this container.

## Using Usefully

Now, let's set up a more useful development environment using VSCode instead of the default Ipykernel terminal session the container provides by default.

The idea is we want to be able to connect to the running development-ready unsloth environment in a fresh VSCode window attached to the running container using the Dev Containers extension. We also want to mount/attach a local folder within the container so we can access any artifacts prodcued like notebooks, python files, model files, etc.


### Run Container with Attached Storage

We'll modify the run command to attach local storage:

```shell
docker run --gpus all -v /path/to/local/storage:/root/ -it unsloth
```

In my case I created a folder called "storage" in the unsloth_docker git repo I cloned.

![image](https://github.com/user-attachments/assets/bc966c84-9f22-4cf8-a5d3-80e40cfec1a2)

Once the command is run, you should see an Ipykernel session started.

Now, go to the docker side panel, select the running container and click "Attach Visual Studio Code"

<div align="center">
  <img src="https://github.com/user-attachments/assets/8b4fc539-46e2-41d0-9812-8b0266c450aa" width="50%">
</div>

If you open the folder /root/ within your newly opened Dev Container session, and add/remove any files, you will also see them appear in the host storage folder you mapped when running the container.

> At this point, the container will keep running even if you close the terminal window in which the Ipykernel session is running, because the container is begin used via the Dev Containers session.

## Conclusion

Now , you can create an python or notebook files within this dev container and use the unsloth library for finetuning LoRA's , or performing efficient GPU-patched inference on supported models. Later, you can copy out any artifacts to use elsewhere as well, though you will probably need a working unsloth environment to use them there as well.

> Use the "docker cp" command to copy files into and out of the mapped storage, as otherwise the correct folder permissions won't be set. See the last reference for how to use the "docker cp" command.

# References

- [Ubuntu | Docker Docs](https://docs.docker.com/engine/install/ubuntu/)
- [Post-installation steps | Docker Docs](https://docs.docker.com/engine/install/linux-postinstall/)
- [Installing the NVIDIA Container Toolkit — NVIDIA Container Toolkit 1.16.0 documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
- [GitHub - zytedata/unsloth\_docker: A working Dockerfile that has unsloth with all the other dependencies](https://github.com/zytedata/unsloth_docker)
- [Pip Install | Unsloth Documentation](https://docs.unsloth.ai/get-started/installation/pip-install)
- [GitHub - unslothai/unsloth: Finetune Llama 3.1, Mistral, Phi & Gemma LLMs 2-5x faster with 80% less memory](https://github.com/unslothai/unsloth)
- [How To Copy File(s) To a Docker Container - 2 Approaches](https://www.warp.dev/terminus/copy-file-to-docker-container)
