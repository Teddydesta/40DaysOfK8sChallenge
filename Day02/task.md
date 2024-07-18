### #40DaysOfK8sChallenge
**Day 3/40 — Multi Stage Docker Builduction**

In the previous challenge of the #40DaysOfKubernetes Challenge! prepared by Piyush Sachdeva, we explored the process of Dockerizing a project. Today, we will take it a step further by creating a multi-stage Docker build. This method allows us to separate the build environment from the runtime environment, resulting in leaner and more efficient Docker images. In this article, we will guide you through each step of the multi-stage Docker build process.

**Key Steps**

- Clone the Repository: Get the sample application code.
- Navigate to the Directory: Change into the cloned repository directory.
- Create a Dockerfile: Define the container environment for your app.
- Build Docker Images: Generate images from your Dockerfile.
- Tag and Push Images: Prepare and upload your images to Docker Hub.
- Pull the Image: Download the image to different environments.
- Run Containers: Start your application using Docker commands.
- Interact and Monitor: Use docker exec to run commands inside containers and docker logs to view application output.
- Clean Up Old Images: Remove unused Docker images from the local repository.

**Prerequisites**

Before you start, ensure that you have the following installed on your machine:

**Docker**: Follow the official Docker installation guide to install Docker.
Steps to Create a Multi-Stage Docker Build

1. Clone the Sample Repository
Clone the below sample repository, or you can use any web application that you have:

`git clone https://github.com/piyushsachdeva/todoapp-docker.git
`
2. Navigate to the Directory

Change into the repository directory:
`cd todoapp-docker/
`

3. Create a Dockerfile

A Dockerfile is a text document that contains all the commands to assemble an image.

After navigated to the repository directory, then create an empty file with the name Dockerfile under the parent directory, using below command: —

```
//for Linux terminal
touch Dockerfile

//for Windows power shell
New-Item -Path . -Name "Dockerfile" -ItemType "File" -Force
```

I have an empty file called “Dockerfile” in my parent application directory like below: —


4. Add Dockerfile Content

After Craeting the “Dockerfile” file, Using the text editor of your choice, paste the below content into the Dockerfile:

# Uses the Node.js 22 Alpine image for a lightweight build environment
FROM node:22-alpine3.19 AS installer
# Sets the working directory inside the container to /app
WORKDIR /app

# Copies the package files to the working directory.
COPY package*.json ./

# Installs the dependencies defined in the package files.
RUN npm install 

# Copies the entire application code to the working directory
COPY . .

# Builds the application
RUN npm run build

# Uses the latest Nginx image for serving the built application.
FROM nginx:latest AS deployer

# Copies the built files from the installer stage to the Nginx default directory for serving static files.
COPY --from=installer /app/build /usr/share/nginx/html

5. Build the Docker Image

Using the Dockerfiles, we will build Docker images for each part of our application. These images are templates for creating containers.

To build the docker image use the bellow command:

docker build -t todoapp-docker .

6. Verify the Image

Verify the image has been created and stored locally:

docker images

7. Push the Image to Docker Hub

Use the docker push command to push your images to Docker Hub, but before pushing, you have to login to docker as well 😊, If the credential is corect you will see a message “login succeeded”

docker login
docker tag todoapp-docker:latest username/remote-repo:tagname
docker images
docker push username/remote-repo:tagname

After pushing the images, you can verify that they have been uploaded successfully by logging into your Docker Hub account and navigating to your repositories.


8. Pull the Image to Another Environment

To pull the image to another environment , you can use below command

docker pull username/remote-repo:<tagname>

9. Run the Docker Container

To start your Docker containers, use the docker run command if you are managing individual containers.

docker run -dp 3000:3000 username/remote-repo:<tagname>

Verify your app. If you have followed the above steps correctly, your app should be listening on localhost:3000, just open a web browser to navigate to http://localhost:5000 and test your Application if it run successfully. Here we go 👉


10. Interact and Monitor: Run a Docker commands inside a running docker container.

Docker Exec Command
The docker exec command allows you to run commands inside a running Docker container. This can be useful for debugging, inspecting, or interacting with your containerized application.

To enter(exec) into the container, use the below command

docker exec -it <containername> sh     
//or
docker exec -it <containerid> sh

View Docker Logs
The docker logs command allows you to view the logs generated by a running Docker container. This is useful for monitoring the output of your application and debugging issues.

To view docker logs, use the below command: -

docker logs <containername>

//or

docker logs <containerid>

Inspect The Docker Container
docker inspect is a powerful Docker CLI command that provides detailed information about Docker objects such as containers, images, volumes, and networks. This command outputs JSON-formatted data, making it useful for obtaining comprehensive insights into various aspects of Docker objects.

To view the content of the Docker container, use the below command: -

docker inspect <container_id_or_name>

11. Clean up old Docker images from the local repository:

docker image rm image-id
Summary
In this challenge, we successfully created a multi-stage Docker build process. We started by cloning a sample repository, created a Dockerfile with multi-stage builds, built and verified the Docker image, and then pushed it to Docker Hub. Additionally, we explored how to pull the image to another environment, run the Docker container, and perform various management tasks such as viewing logs, inspecting containers, and cleaning up old images. This multi-stage build approach helps in creating optimized and secure Docker images suitable for production environments.

Thank you for following along! 🙏😊 This is Day Three of my Dockerization journey. Stay tuned for the upcoming series of articles where we will continue to delve deeper into Kubernetes and containerization.

Resources: Watch this video for more details