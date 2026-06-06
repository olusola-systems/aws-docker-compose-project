## AWS Docker Compose Deployment

### Project Overview

This project demonstrates how to deploy and manage a containerized web application using Docker Compose on an AWS EC2 instance.
Instead of manually creating and running containers with long Docker commands, Docker Compose is used to define the entire deployment inside a single configuration file. This allows services to be deployed, managed, and restarted consistently.
The project deploys a simple Nginx web server hosting a custom HTML page and exposes it through the EC2 instance’s public IP address.
 
 
### Architecture

<img width="2880" height="1800" alt="week14 Architecture" src="https://github.com/user-attachments/assets/500b75fc-6aa7-4d81-8315-c70a6eb377ec" />

 
### Technologies Used

AWS EC2
Docker
Docker Compose
Nginx
Linux (Amazon Linux 2023)
Git
GitHub
 

### Project Workflow

Launch an EC2 instance
Install Docker
Configure Docker permissions
Install Docker Compose
Create a Docker Compose configuration file
Create a custom HTML page
Deploy the application using Docker Compose
Verify container status
Access the website through the EC2 public IP
Push project files to GitHub

 
### Project Files

docker-compose.yml
services:
  website:
    image: nginx:latest
    container_name: week14-website
    ports:
      - "80:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html
    restart: always
index.html
Custom HTML page served by the Nginx container.
 
 
### Commands Used

Verify Docker Installation
docker version
Verify Docker Compose Installation
docker compose version
Deploy Application
docker compose up -d
View Running Containers
docker ps
Stop Application
docker compose down

 
### Verification

The deployment was verified by:
Confirming Docker Compose installation
Successfully starting the Nginx container
Viewing running containers with Docker
Accessing the website through the EC2 public IP address
Confirming the custom HTML page loaded successfully

 
### What Went Wrong

Issue 1: Docker Compose Plugin Not Available
Problem
sudo dnf install docker-compose-plugin -y
Returned:
No match for argument: docker-compose-plugin
Resolution
Docker Compose was manually downloaded from the official Docker GitHub release and installed under:
~/.docker/cli-plugins/docker-compose
Installation was verified with:
docker compose version
 
Issue 2: Docker Permission Error
Problem
docker version
Returned:
permission denied while trying to connect to the Docker daemon socket
Resolution
Added the EC2 user to the Docker group:
sudo usermod -aG docker ec2-user
Logged out and logged back in to apply the changes.
 
Issue 3: Git Repository Not Initialized
Problem
Project files were created before Git initialization, causing:
fatal: not a git repository
Resolution
Initialized Git manually:
git init
git branch -M main
git remote add origin <repository-url>
Files were then committed and pushed successfully to GitHub.
 

### Lessons Learned

•	Docker Compose is not always available in Amazon Linux package repositories. The docker-compose-plugin package returned “No match for argument” on Amazon Linux 2023. The solution was to download Docker Compose directly from the official Docker GitHub release and install it manually under ~/.docker/cli-plugins/. When a package manager fails, go to the source.
•	Docker requires explicit permission configuration for non-root users. Running docker version as ec2-user returned a permission denied error because the user was not in the Docker group. Adding the user with sudo usermod -aG docker ec2-user and logging out and back in resolved it. This is a one-time setup step that is easy to miss and produces a confusing error if skipped.
•	Git should be initialized before creating project files, not after. Initializing Git after files already exist is harmless but creates unnecessary extra steps — running git init, setting the branch, and adding the remote manually. Starting with git init as the first step avoids this entirely.
•	Docker Compose replaces long Docker commands with a single configuration file. Instead of remembering docker run -d -p 80:80 --name container-name image-name, the entire deployment is defined in docker-compose.yml and started with docker compose up -d. One file, one command, consistent deployments every time.
•	Container volumes connect local files to running containers without rebuilding the image. Mounting ./index.html directly into the Nginx container means the HTML file can be updated without triggering a new Docker build.

 
### Screenshots

#### Docker Compose installation, version verification, deployment and  Running container verification
<img width="1440" height="900" alt="Screenshot 2026-06-06 at 5 13 10 AM" src="https://github.com/user-attachments/assets/808ddb8a-902b-458b-9ea5-bcb25e94bd1f" />

#### Website loaded in browser
<img width="1440" height="900" alt="Screenshot 2026-06-06 at 5 12 36 AM" src="https://github.com/user-attachments/assets/6a216279-a1f8-4ce7-a99d-7e721cad4d3c" />

#### Git push success
<img width="1440" height="900" alt="Screenshot 2026-06-06 at 5 53 36 AM" src="https://github.com/user-attachments/assets/b2e7208a-6f65-48b4-ba4e-8b614e4a2de6" />

#### GitHub repository contents
 <img width="1440" height="900" alt="Screenshot 2026-06-06 at 5 54 04 AM" src="https://github.com/user-attachments/assets/459368f8-9f3d-456c-a25f-6c5ab8e4be26" />


### Next Project

Week 15 – Multi-Container Applications
The next project expands Docker Compose by deploying multiple interconnected containers, introducing:
Multi-container architecture
Container networking
Environment variables
Persistent storage
Service-to-service communication
These concepts form the foundation of modern containerized application deployments.
