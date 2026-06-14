1. Cloud Provider and Service Used

I used Amazon Web Services (AWS) EC2 to deploy the application.

Why AWS EC2:
It provides a simple virtual machine (VM) environment suitable for running containerized applications.
It supports Docker installation and full control over the server environment.
It integrates well with CI/CD pipelines using GitHub Actions and SSH.
It is widely used in production DevOps workflows, making it a realistic deployment choice.
2. Virtual Machine Setup

The virtual machine was created using AWS EC2 with the following configuration:

Instance Type: t3.micro (free tier eligible)
Operating System: Ubuntu Server (latest LTS)
Region: Closest available AWS region for low latency
Storage: Default 8 GB EBS volume
Security Group Configuration:
SSH (port 22) access is restricted to the developer’s trusted IP range to reduce attack surface, while HTTP access remains publicly available for application usage.
HTTP (Port 80): Open to all (0.0.0.0/0) to allow public access to the API

After launching the instance, I connected to it using:

ssh -i kora-test.pem ubuntu@<EC2-PUBLIC-IP>
3. Docker Installation and Image Deployment
Docker Installation:

On the EC2 instance, Docker was installed using:

sudo apt update
sudo apt install -y docker.io

Then Docker service was enabled and started:

sudo systemctl enable docker
sudo systemctl start docker

To allow non-root usage:

sudo usermod -aG docker ubuntu
newgrp docker
Pulling the Docker Image:

The application image was stored in GitHub Container Registry (GHCR).

I authenticated with GHCR using a Personal Access Token:

docker login ghcr.io

Then pulled the image:

docker pull ghcr.io/<username>/kora-api:<commit-sha>
Running the Container:

The container was started using:

docker run -d \
  --name kora-api \
  -p 80:3000 \
  -e PORT=3000 \
  ghcr.io/<username>/kora-api:<commit-sha>

This maps:

Host port 80 → Container port 3000
4. How to Check if the Container is Running

To verify the running container:

docker ps

Expected output includes:

Container name: kora-api
Status: Up
Port mapping: 0.0.0.0:80->3000/tcp

To test the API directly:

curl http://localhost/health

Expected response:

{ "status": "ok" }

Or from browser:

http://<EC2-PUBLIC-IP>/health
5. How to View Application Logs

To view logs of the running container:

docker logs kora-api

To follow logs in real-time:

docker logs -f kora-api

Logs show:

Application startup messages
Port binding confirmation
Runtime errors (if any)
6. Summary of Architecture
GitHub Actions CI/CD pipeline builds and tests the application
Docker image is built and pushed to GitHub Container Registry (GHCR)
EC2 instance pulls the latest image
Container is restarted automatically on deployment
Application is publicly accessible via HTTP port 80
7. Final Result

The application is successfully deployed and accessible at:

http://<EC2-PUBLIC-IP>/health

Response:

{ "status": "ok" }