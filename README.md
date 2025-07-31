# nocturne-docker
A dockerfile for building the Nocturne image and connector

## Description
This is my first swing at a Docker container to build Nocturne after experiencing dependency hell with WSL Debian. Many thanks to Nebula from the Nocturne Discord for pointing me to using voidlinux as a starting point for this dockerfile. 

## Prerequisites
This dockerfile project was created using Windows 10, with Debian running under WSL.

## How to use locally

1. Clone this repo. Navigate into it.
2. Run `docker build -t nocturne-build-container .` to build the container.
3. Run `docker run -it --privileged nocturne-build-container` to use the container.
4. Inside the container, run `./finish-init.sh`.

From here, you are able to use `git clone` to pull down the Nocturne Connector or Nocturne Image for building. Use `docker cp` to copy out your build images from the container to your local host directory.

⚠ Warning: When you first go to build the Nocturne Image, you must insert `sudo` before the `download.sh` command: `cd resources/stock-files && sudo ./download.sh`

## How to use for GitHub actions

GitHub actions does not allow running an image in a privledged state unless you self-host. So to use GitHub Actions, you must pre-build your image and capture the changes of the finish-init.sh and push it.

Please make sure you have the GitHub Actions runner installed on your builder machine, and that it is active. Also make sure your runner is tagged against your fork/repo.

1. Build your base image: `docker build --platform=linux/arm64 -t nocturne-builder-base .`
2. Finalize installation of the dependencies that cannot be installed via dockerfile by running the finishing script: `docker run --platform=linux/arm64 --privileged --name nocturne-builder-container nocturne-builder-base /home/builder/finish-init.sh`
3. Commit the changes made to the container: `docker commit nocturne-builder-container nocturne-builder-prebuilt`

If you wish to publish this package/container to GitHub, use these commands:
1. `docker login ghcr.io -u <your-github>`
    > enter in your token where `write:packages` is granted
2. `docker tag nocturne-builder-prebuilt ghcr.io/<your-github>/nocturne-builder:latest`
3. `docker push ghcr.io/<your-github>/nocturne-builder:latest`
