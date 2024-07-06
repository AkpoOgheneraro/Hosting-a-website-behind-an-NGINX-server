# Step-by-Step Guide to Set Up and Push to GitHub

## 1. Create the Project Structure
First, create the directory structure and necessary files for your project.

### Bash
mkdir my-devops-project

cd my-devops-project

mkdir app nginx .github

mkdir .github/workflows

touch app/package.json app/server.js app/Dockerfile

touch nginx/nginx.conf nginx/Dockerfile

touch docker-compose.yml

touch prometheus.yml

touch .github/workflows/deploy.yml

touch terraform/main.tf

touch README.md

## 2. Fill in the Files with the Provided Content

app/package.json:



  
  
  
