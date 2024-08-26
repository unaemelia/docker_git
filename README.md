# Dockerized .NET Application

This project demonstrates how to containerize a .NET 8.0 application using Docker and automate the process of building and publishing the Docker image to Docker Hub with GitHub Actions.

## Project Structure

### 1. `Dockerfile`

The `Dockerfile` defines the steps to build a Docker image for the .NET application:

- **Base Image**: The Docker image is built from the official .NET SDK 8.0 image.
- **Working Directory**: The working directory inside the container is set to `/app`.
- **Copy Files**: The compiled binaries from the `./bin/Debug/net8.0/` directory are copied into the container.
- **Command**: The container runs the `docker_git.dll` file using `dotnet`.

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env

WORKDIR /app

COPY ./bin/Debug/net8.0/ .

CMD ["dotnet", "docker_git.dll"]
```

### 2. `.github/workflows/publish.yml`

This file defines a GitHub Actions workflow to automate the process of building and pushing the Docker image to Docker Hub:

- **Triggers**: The workflow is triggered on each push to the main branch.
- **Jobs**:
  - `Checkout Repository`: Checkout Repository: Checks out the repository code using actions/checkout@v3.
  - `Set Up .NET`: Installs .NET 6.x SDK using actions/setup-dotnet@v3.
  - `Restore and Build`: Restores dependencies and builds the project.
  - `Login to Docker Hub`: Logs in to Docker Hub using credentials stored in GitHub secrets.
  - `Build and Push Docker Image`: Builds the Docker image and pushes it to Docker Hub.

### Getting Started

Prerequisites
- Docker installed on your machine.
- Docker Hub account for pushing the image.
- .NET 8.0 SDK for building the project locally.

### Building and Running the Docker Image Locally

1. Clone the repository:
  ```markdown
  git clone https://github.com/your-username/docker_git.git
  cd docker_git
  ```
2. Build the docker image
   ```markdown
   docker build -t your-dockerhub-username/docker_git .
   ```
3. Run the docker container
   ```markdown
   docker run -d -p 8080:80 your-dockerhub-username/docker_git
   ```
4. Access the application by navigating to http://localhost:8080 in your web browser.

### Continuous Integration and Deployment
This project uses GitHub Actions to automatically build and publish the Docker image to Docker Hub:

- Ensure you have the following GitHub secrets set up in your repository:
  - `DOCKERHUB_USERNAME`: Your Docker Hub username.
  - `DOCKERHUB_TOKEN`: Your Docker Hub access token.
On each push to the `main` branch, the GitHub Actions workflow will build the Docker image and push it to your Docker Hub repository.
