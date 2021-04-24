# Project 4

## Project Overview

In this project I set up a docker file and github workflow to build and push the docker container to docker hub

## Run Project Locally

To build and run the project locally you can follow the following steps

### how you installed docker + dependencies (WSL2, for example)

Docker can be installed [here](https://www.docker.com/products/docker-desktop). I had WSL installed already, but you may be required to follow instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package) if you need to install WSL or upgrade to WSL2

### how to build the container

To build the container, start powershell in the directory containing this repo, run the following command:

```sh
docker build -t { container name }:{ tag name } .
```

### how to run the container

To run the container you can use the following command:

```sh
docker run -d -p 80:80 { container name }:{ tag name }
```

### how to view the project (open a browser...go to ip and port...)

Now the container should be running on port 80, so you can go to [localhost:80](localhost:80) to check if it is working.

## Configure AWS CLI

### how you installed

To configure the AWS cli, start by grabbing the installer [here](https://aws.amazon.com/cli/)

### how to configure

After installation start the configuration process by running `aws configure`

You should be prompted to fill in information as follows:

```
AWS Access Key ID [None]: { Access Key ID }
AWS Secret Access Key [None]: { Secret Access Key }
Default region name [None]: { Region Name }
Default output format [None]: { Output Format }
```

## Create DockerHub public repo

### process to create

On [DockerHub](https://hub.docker.com/) you can create a new repository [here](https://hub.docker.com/repositories/create)

Give it a name, make it public, and hit `Create`

## Configure GitHub Secrets

### what credentials are needed - DockerHub credentials (do not state your credentials)

You'll need to add login information for DockerHub to your Github Repository so that the workflow can build and push to DockerHub

### set secrets and secret names

Inside you repository, go to `Settings -> Secrets -> New Repository Secret`

Create two new secrets this way:

```
DOCKER_USERNAME = { Your Username on DockerHub }
DOCKER_PASSWORD = { Your Password on DockerHub }
```

## Configure GitHub Workflow

To automate the build and push process you can create a workflow in your repository at this location: `.github/workflows/docker.yml`

Add the following template code:

```yml
name: Publish Docker image
on:
  release:
    types: [published]
jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repo
        uses: actions/checkout@v2
      - name: Push to Docker Hub
        uses: docker/build-push-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
          repository: username/repo-name
          tag_with_ref: true
```

### variables to change (repository, etc.)

The only change you'll need to fill in is the `repository`, change `username/repo-name` to match the repo you made on DockerHub.
