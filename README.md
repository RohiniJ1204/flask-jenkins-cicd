# Flask Jenkins CI/CD


Simple Flask app containerized and deployed to an EC2 host using Jenkins pipeline.


## Prerequisites
- Jenkins with Docker installed (agent or master with docker)
- EC2 instance (Ubuntu) with Docker installed and SSH access
- Jenkins server able to SSH to EC2 (use SSH key or credentials plugin)
- GitHub repo with this code


## Steps on EC2 (one-time)
1. Install Docker: sudo apt update && sudo apt install -y docker.io and sudo usermod -aG docker ubuntu
2. Install Nginx (if you want reverse proxy): sudo apt install -y nginx
3. Create directory /home/ubuntu/flask-app and ensure docker can run
4. Optionally create a systemd service (not required for this demo)


## How Jenkins pipeline works
- Checkout code
- Build Docker image
- Push (optional) or copy image to EC2 and run container
- Restart container on EC2


## Usage
- Configure Jenkins credentials: add SSH private key (credential id: ec2-ssh-key)
- Update Jenkinsfile with EC2 user/IP and credential id or use Jenkins credential bindings
