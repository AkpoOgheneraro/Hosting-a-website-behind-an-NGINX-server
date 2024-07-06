# Step-by-Step Guide to Set Up and Push to GitHub

## 1. Create the Project Structure
First, create the directory structure and necessary files for your project.

### Bash
![bash](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/032649d7-3f19-4db0-8c90-7323cdf82481)

## 2. Fill in the Files with the Provided Content

app/package.json:

![apppackage json11111](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/b094f212-9ce6-4a9a-bbaa-f101dffd7ba9)

app/server.js:

![appserver js123](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/fc72e4f4-f40f-47b5-a192-baf6a882473e)

app/Dockerfile:

![appDockerfile](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/325df46b-645e-46b8-b009-7c0524ab207c)

nginx/nginx.conf:

![nginxnginx conf](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/6887eef2-06bd-4e62-821e-1e51ed522fee)

nginx/Dockerfile:

![nginxDockerfile](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/7e0ceda9-5204-4f18-bdef-a72a1f58da0a)

docker-compose.yml:

![docker-compose yml](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/deb78f18-553c-4917-b84b-160a3966b6a0)

prometheus.yml:

![prometheus yml](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/b08f7613-bd88-47ad-b7eb-e4d0ad0eda60)

terraform/main.tf:

![terraformmain tf](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/c1d43697-dcca-40fe-990c-88db2094aa37)

.github/workflows/deploy.yml:

![githubworkflowsdeploy yml](https://github.com/AkpoOgheneraro/Hosting-a-website-behind-an-NGINX-server/assets/152994401/1ba54460-8749-48a0-8af5-d7fb3fbd9be1) 

# DevOps Project: Hosting a Website Behind an NGINX Server

## Overview

This project involves containerizing a web application, deploying it to a virtual machine (VM) using a CI/CD pipeline, and setting up monitoring with Prometheus and Grafana. The domain name used is `raro.solutions`.

## Technology Stack

### Required Technologies
- **Docker**: Containerization platform ([Documentation](https://docs.docker.com/))
- **Docker Compose**: Multi-container orchestration for Docker ([Documentation](https://docs.docker.com/compose/))
- **Terraform**: Infrastructure provisioning tool ([Documentation](https://www.terraform.io/))
- **GitHub Actions**: CI/CD tool ([Documentation](https://docs.github.com/en/actions))
- **Prometheus**: Monitoring tool ([Documentation](https://prometheus.io/))
- **Grafana**: Monitoring tool ([Documentation](https://grafana.com/))

## Project Setup

1. **Create a Local Development Directory**:
    ```bash
    mkdir my-devops-project
    cd my-devops-project
    ```

2. **Initialize a Git Repository**:
    ```bash
    git init
    ```

## Part 1: Containerization

### 1.1 Application Container

#### 1.1.1 Create a Sample Node.js Application

**Directory Structure**:


**app/package.json**:
```json
{
  "name": "myapp",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```
**app/server.js**:
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```
### 1.1.2 Create a Dockerfile for the Application
**app/Dockerfile**:
```Dockerfile
FROM node:14

WORKDIR /app

COPY package.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```
## 1.2 NGINX Server Container
### 1.2.1 Create NGINX Configuration
**nginx/nginx.conf**:
```nginx
http {
  server {
    listen 80;

    location / {
      proxy_pass http://app:3000;
    }
  }
}
```
### 1.2.2 Create a Dockerfile for NGINX
**nginx/Dockerfile**:
```Dockerfile
FROM nginx:latest

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 1.3 Multi-Container Orchestration (Docker Compose)
**docker-compose.yml**:
```yaml
version: "3.8"

services:
  app:
    build: ./app
    ports:
      - "3000:3000"

  nginx:
    build: ./nginx
    ports:
      - "80:80"
    depends_on:
      - app

  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3001:3000"
```

**prometheus.yml**:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'nginx'
    static_configs:
      - targets: ['nginx:80']

  - job_name: 'app'
    static_configs:
      - targets: ['app:3000']
```


## Part 2: Infrastructure Provisioning with Terraform
### 2.1 Install Terraform
Download and install Terraform from the official website. Follow the instructions for your operating system to ensure Terraform is correctly installed and available in your system's PATH.

### 2.2 Write Terraform Configuration
**Create a directory for your Terraform configuration files**:
```bash
mkdir terraform
cd terraform
```

**main.tf**:
```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0" # Amazon Linux 2 AMI
  instance_type = "t2.micro"

  tags = {
    Name = "DevOpsWebServer"
  }

  provisioner "remote-exec" {
    inline = [
      "sudo yum update -y",
      "sudo amazon-linux-extras install docker -y",
      "sudo service docker start",
      "sudo usermod -a -G docker ec2-user",
      "sudo curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose",
      "sudo chmod +x /usr/local/bin/docker-compose",
    ]
  }
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
````

### 2.3 Initialize and Apply Terraform Configuration
**Run the following commands to initialize Terraform and apply the configuration**:
```bash
terraform init
terraform apply
```
Review the plan output and type 'yes' to apply the changes. Note the public IP of the created instance from the output.

## Part 3: CI/CD Pipeline with GitHub Actions
### 3.1 Set Up GitHub Repository
Create a new repository on GitHub. Name it my-devops-project (or any preferred name).

**Clone the repository to your local machine**:
```bash
git clone https://github.com/AkpoOgheneraro/my-devops-project.git
cd my-devops-project
```
Copy your project files into the cloned repository directory.

### 3.2 Configure GitHub Secrets
To securely store sensitive information required for deployment, configure GitHub Secrets:

Navigate to the repository on GitHub.

Go to Settings > Secrets > Actions.

Add the following secrets:

DOCKER_HUB_USERNAME: Your Docker Hub username.

DOCKER_HUB_PASSWORD: Your Docker Hub password.

VM_HOST: The public IP of your VM.

VM_USER: The username to SSH into your VM.

VM_KEY: Your private SSH key for accessing the VM.

### 3.3 Create GitHub Actions Workflow
### Create a GitHub Actions workflow file:

**.github/workflows/deploy.yml**:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v1

    - name: Log in to Docker Hub
      run: echo ${{ secrets.DOCKER_HUB_PASSWORD }} | docker login -u ${{ secrets.DOCKER_HUB_USERNAME }} --password-stdin

    - name: Build and push Docker images
      run: |
        docker-compose build
        docker-compose push

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: SSH to VM and deploy
      uses: appleboy/ssh-action@v0.1.3
      with:
        host: ${{ secrets.VM_HOST }}
        username: ${{ secrets.VM_USER }}
        key: ${{ secrets.VM_KEY }}
        script: |
          cd /path/to/your/app
          docker-compose pull
          docker-compose up -d
```
## Part 4: Final Touches
### 4.1 Domain Name Configuration
Configure your domain name (raro.solutions) to point to the public IP of your VM. This involves:

Log in to your domain registrar (e.g., GoDaddy, Namecheap).

Navigate to the DNS settings for your domain name, mine was (raro.solutions.)
Add an A record with the following details:

Name: @ (or leave it blank)

Type: A

Value: The public IP of your VM

TTL: 1 hour (or your preferred value)

### 4.2 Testing and Monitoring
### 4.2.1 Test the Deployment
After the pipeline completes, visit your domain name, mine was ( http://raro.solutions ) in your browser. You should see the "Hello, World!" message from your application.

Verify that the NGINX server is correctly proxying requests to your application.

### 4.2.2 Set Up Monitoring with Prometheus and Grafana

Access Prometheus at http://<VM_IP>:9090 to ensure it's scraping metrics.

Access Grafana at http://<VM_IP>:3001, and log in using the default credentials (admin / admin). Add Prometheus as a data source and create dashboards to visualize metrics.

### 4.3 Secure Your Deployment
Ensure your containers are secure by following best practices:

1. Use non-root users in your Dockerfiles.
2. Limit the exposed ports to only what's necessary.
3. Regularly update your images to include security patches.

## Part 5: Push to GitHub
Add, commit, and push your changes to GitHub:
  
