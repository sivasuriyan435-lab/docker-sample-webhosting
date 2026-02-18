![Architecture](https://github.com/user-attachments/assets/c13b3126-efbe-43f6-a9ca-19e3ef3bf21e)

1: Launch EC2 Ubuntu Instance

Go to AWS EC2 → Launch Instance

AMI: Ubuntu 22.04 LTS

Instance Type: t2.micro (free tier)

Network: default VPC

Subnet: default public subnet

Auto-assign Public IP: Enable

Security Group: create a new SG allowing:

SSH (TCP 22) → your IP

TCP 3000 → 0.0.0.0/0 (for your Node.js app)


2: Connect via Session Manager

3: Update System and Install Docker
sudo apt update -y
sudo apt upgrade -y
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER

4.Create Website Directory and Files
mkdir ~/website
cd ~/website

nano nano.html         # HTML file
nano nano.css.style    # CSS file
nano nano.server.js    # Node.js server
nano nano.script.js    # Client-side JS
nano nano.package.js   # Node.js package.json

Example nano.server.js:

const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.static('.'));

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/nano.html');
});

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));

{
  "name": "nano-app",
  "version": "1.0.0",
  "main": "nano.server.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}

5: Create Dockerfile
FROM node:18
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "nano.server.js"]

6: Build and Run Docker Image
docker build -t nano-app .
docker run -d -p 3000:3000 --name nano-container nano-app


