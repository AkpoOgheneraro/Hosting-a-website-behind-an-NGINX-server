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










  
  
  
