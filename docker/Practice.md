# Docker Beginner Exercises

Welcome to the Docker beginner exercises! These exercises are designed to help you learn Docker basics through hands-on practice. Follow the instructions for each exercise and complete the tasks to reinforce your understanding of Docker concepts.

## Exercise 1: Installation and Setup

1. Install Docker Desktop on your machine.
2. Verify the installation by running the `docker --version` command.
3. Configure Docker to start on boot.

## Exercise 2: Basic Commands

1. Run a Docker container using the `docker run` command with different options (e.g., detached mode, interactive mode, specifying a port).
2. List all running containers using the `docker ps` command.
3. List all containers (including stopped ones) using the `docker ps -a` command.
4. Stop a running container using the `docker stop` command.
5. Remove a container using the `docker rm` command.

## Exercise 3: Creating Dockerfiles

1. Create a Dockerfile for a simple web server (e.g., using Nginx).
2. Build an image from the Dockerfile using the `docker build` command.
3. Run a container from the built image.

## Exercise 4: Working with Volumes

1. Create a named volume using the `docker volume create` command.
2. Mount a volume to a container using the `-v` option in `docker run`.
3. Verify data persistence by creating a file in the mounted volume from within the container and checking it on the host machine.

## Exercise 5: Networking

1. Create a Docker network using the `docker network create` command.
2. Run multiple containers on the same network and verify they can communicate with each other.

## Exercise 6: Docker Compose

1. Write a `docker-compose.yml` file to define and run a multi-container application (e.g., a web server and a database).
2. Use `docker-compose up` and `docker-compose down` to start and stop the application.

## Exercise 7: Docker Registry

1. Push an image to Docker Hub using the `docker push` command.
2. Pull an image from Docker Hub using the `docker pull` command.
3. Tag an image with a custom name and version.

## Exercise 8: Advanced Topics (Optional)

1. Explore Docker Swarm by setting up a multi-node cluster.
2. Experiment with Docker's built-in orchestration features.
3. Investigate Docker security practices and implement them in your Docker configurations.

Congratulations on completing the Docker beginner exercises! You should now have a solid understanding of Docker basics and be ready to tackle more advanced topics. Happy containerizing!
