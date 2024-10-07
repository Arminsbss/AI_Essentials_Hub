# Docker Essentials

## What is Docker?
Docker is a platform for developing, shipping, and running applications in containers. Containers package an application and its dependencies together, ensuring consistency across environments.

## Key Concepts

### 1. Container
- **Definition**: A lightweight, standalone executable package that includes everything needed to run a piece of software, including the code, runtime, libraries, and system tools.
- **Isolation**: Containers are isolated from each other and the host system, allowing multiple applications to run on the same machine without conflicts.

### 2. Image
- **Definition**: A read-only template used to create containers. Images contain the application code and all its dependencies.
- **Layers**: Images are built in layers, with each instruction in a Dockerfile creating a new layer.

### 3. Dockerfile
- **Definition**: A text file that contains instructions for building a Docker image.
- **Common Instructions**:
  - `FROM`: Specifies the base image.
  - `COPY`: Copies files from the host to the image.
  - `RUN`: Executes commands in the image.

### 4. Docker Hub
- **Definition**: A cloud-based registry for sharing Docker images. It allows users to store and retrieve images publicly or privately.

## Basic Commands

### 1. Installing Docker
Follow the official [Docker installation guide](https://docs.docker.com/get-docker/) for your platform.

### 2. Pulling an Image
```bash
docker pull <image-name>
```
- Downloads an image from Docker Hub.

### 3. Running a Container
```bash
docker run <image-name>
```
- Creates and starts a new container from the specified image.

### 4. Listing Containers
```bash
docker ps
```
- Shows running containers. Add `-a` to list all containers.

### 5. Stopping a Container
```bash
docker stop <container-id>
```
- Stops a running container.

### 6. Removing a Container
```bash
docker rm <container-id>
```
- Deletes a stopped container.

### 7. Building an Image
```bash
docker build -t <image-name> .
```
- Builds an image from a Dockerfile in the current directory.

## Best Practices
- **Use Official Images**: Start with trusted base images from Docker Hub.
- **Keep Images Small**: Remove unnecessary files and dependencies to minimize image size.
- **Leverage Layer Caching**: Organize Dockerfile commands to take advantage of caching for faster builds.

## Conclusion
By understanding these core concepts and commands, you'll be equipped to effectively use Docker for application development and deployment, ensuring consistency across different environments.
