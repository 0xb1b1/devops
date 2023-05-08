# Beginner's guide

## What is Docker

Docker is a containization platform. Its purpose is to provide developers with an easy way to make environments consistent and reproducible, this reducing the reliance on the OS installed on bare-metal machines and VMs. Also, by isolating applications from host operating systems, we reduce the attack surface for potential instruders.

## Containers vs virtual machines (VMs)

Virtual machines are a good way to isolate services running on your hardware. And although they do provide a huge security barrier, being entirely separate "machines" running on your host system, they do come with a set of challenges. Here's a bunch of them:

- VMs usually take a heftier toll on your system's resources
- They provide less flexibility; this includes:
- - Inability to easily mount system directories and edit data contained in virtual machines
- - A harder time setting up networking and volume sharing
- - A typical VM takes up way more storage due to it not sharing the host's kernel

And so that's why developers tend to prefer Docker.

## Install Docker

The way you install Docker depends on the operating system you're using. However, you should use Unix-like systems (GNU/Linux or \*BSD) because it tends to run faster there while consuming less resources. It's because Docker on MacOS runs under [LinuxKit VM](https://github.com/linuxkit/linuxkit) (read more on this topic [here](https://www.paolomainardi.com/posts/docker-performance-macos/)) and under [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/) on Windows.

### GNU/Linux

Just run the following command in your terminal:

```bash
curl -fsSL https://get.docker.com/ | sh
```

If you prefer manual installs, [here](https://docs.docker.com/engine/install/ "Docker Engine official documentation")'s the official manual.

!!! warning

    Docker Desktop runs a virtual machine and uses a custom docker context `docker-linux` on startup. Read more about Docker Desktop on GNU/Linux [here](https://docs.docker.com/desktop/install/linux-install/ "Docker Desktop for Linux — official documentation"). For more information regarding this warning, visit [this page](https://docs.docker.com/desktop/faqs/linuxfaqs/#what-is-the-difference-between-docker-desktop-for-linux-and-docker-engine "Linux: Docker Desktop vs Docker Engine — official documentation"). Prefer Docker Engine if possible as it offers the best performance and only use Docker Desktop on GNU/Linux if you absolutely need a GUI.

### MacOS

On MacOS, the fastest way to install Docker is [Homebrew](https://brew.sh/):

```bash
brew install --cask docker
```

If you don't have `brew` installed on your system, run this command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

If you want to drag-and-drop Docker Desktop without touching the terminal, [follow the official instructions](https://docs.docker.com/desktop/install/mac-install/ "Docker Desktop for OSX — official documentation").

### Windows

[**Ensure that WSL2 is installed!**](https://learn.microsoft.com/en-us/windows/wsl/install "Microsoft WSL official documentation") Docker won't run without this Windows component.

TL;DR: Run the following command under Powershell with administrative privileges:

```powershell
wsl --install
```


If you have [Chocolatey](), install Docker using the `choco` command and reboot your PC:

```powershell
choco install docker-desktop
```

If you don't have `choco` installed on your system, [follow the official documentation](https://docs.docker.com/desktop/install/windows-install/ "Docker Desktop for Windows — official documentation").

## Check your installation

To check if your Docker installation is healthy, run one of the following commands:

```bash
docker ps
# or
docker info
```
 
If output contains no error messages, you are good to go!

## Run a container

Hurray, it's time to create your first containerized environment!

To do that, you can use either `docker run` or `docker compose`. Let's tinker with the first option.

`docker run` is used to procedurally create containers. Other commands, including `docker rm`, `docker exec`, will be shown in this section. Let's run a container from the official Ubuntu image!

```bash
docker run -it --rm ubuntu:latest
```

!!! note "Normal behavior"

    Seeing `Unable to find image 'ubuntu:latest' locally` after running `docker run` against an image you have never used before is to be considered normal. Docker will attempt pulling the image you requested (in this case, `ubuntu`) from [Docker Hub](https://hub.docker.com)

!!! warning

    If you forgot to pass the `--rm` flag, remove your newly created container manually after you're done with it! Otherwise, it'll indefinitely wait for you to do something with it. Although useful when you need to restart containers later, this feature has the potential to clutter your Docker installation. To remove the now obsolete container, run `docker ps -a`. obtain the container's ID (listed under the `CONTAINER ID` column) and remove it with `docker rm <CONTAINER ID>`.

??? note "What's ":latest"?"

    It is a version of the Ubuntu image. Versions in Docker are in a way similar to Git branches, without the hassle of merging them. The most common strategy is to publish the latest stable release under the `latest` tag and all major/minor releases under tags with names corresponding to their release version. Developers sometimes also have `testing`, `stable` tags, along others. If you do not specify a tag (i.e. `docker run -it --rm ubuntu`), tag `latest` will be used by default.

??? note "What's "-it"?"

    It is a flag passed to the `docker run` command. It tells Docker to run this container in interactive mode, meaning that you can interact with it (in this case, you get a bash shell!)

??? note "What's "--rm"?"

    It is a flag passed to the `docker run` command. This particular flag tells Docker that the container is to be destroyed when it ceases running. We are using this flag to prevent crowding our container list and consuming precious system resources (remember: images are immutable, but application running inside containers can write data to)

