# nocturne-docker
A dockerfile for building the Nocturne image and connector

## Description
This is my first swing at a Docker container to build Nocturne after experiencing dependency hell with WSL Debian. Many thanks to Nebula from the Nocturne Discord for pointing me to using voidlinux as a starting point for this dockerfile. 

## Prerequisites
This dockerfile project was created using Windows 10, with Debian running under WSL.

## How to use

1. Clone this repo. Navigate into it.
2. Run `docker build -t nocturne-build-container .` to build the container.
3. Run `docker run -it --privileged nocturne-build-container` to use the container.
4. Inside the container, run `./finish-init.sh`.

From here, you are able to use `git clone` to pull down the Nocturne Connector or Nocturne Image for building. Use `docker cp` to copy out your build images from the container to your local host directory.

⚠ Warning: When you first go to build the Nocturne Image, you must insert `sudo` before the `download.sh` command: `cd resources/stock-files && sudo ./download.sh`
