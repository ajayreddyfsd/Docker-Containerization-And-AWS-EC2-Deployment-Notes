# Deploying a Docker Container on Amazon EC2

## Step-by-Step Guide

### Step 1: Create an EC2 Instance

1. Go to the **AWS EC2 Dashboard** → **Launch Instance**.
2. Choose an **Amazon Linux 2023 AMI**.
3. Select **instance type** (e.g., `t2.micro` for free tier).
4. Configure **security group (SG)**:

   * Allow **SSH** (port 22) from your IP.
   * Allow the **port your app uses** (e.g., 8000) from anywhere (`0.0.0.0/0`) so it’s publicly accessible.
5. **Launch instance** and **download the `.pem` key file** for SSH connection.

### Step 2: Connect to EC2 via SSH

1. Open terminal (or PowerShell).
2. Move to the folder where your `.pem` file is located.
3. Connect using SSH:

   ```bash
   ssh -i "your-key.pem" ec2-user@your-ec2-public-dns
   ```
4. Accept the host fingerprint. You are now in your EC2 shell.

### Step 3: Update the System

```bash
sudo yum update -y
```

### Step 4: Install Docker

1. Install Docker and dependencies:

   ```bash
   sudo yum install docker -y
   ```
2. Start Docker service:

   ```bash
   sudo service docker start
   ```
3. Verify installation:

   ```bash
   sudo docker info
   ```

### Step 5: Log in to Docker Hub

1. Login using your Docker Hub credentials:

   ```bash
   sudo docker login
   ```
2. If successful, you are ready to pull and run Docker images.

### Step 6: Pull and Run Your Docker Image

1. Pull your Docker image and run it:

   ```bash
   sudo docker run --restart=always -p 8000:8000 your-docker-username/your-image-name
   ```

   * `--restart=always` → ensures container restarts if EC2 reboots.
   * `-p 8000:8000` → maps EC2 port 8000 to container port 8000.
2. Check running containers:

   ```bash
   sudo docker ps
   ```

### Step 7: Stop/Remove Containers (if needed)

* Stop a container:

  ```bash
  sudo docker stop <container_id_or_name>
  ```
* Remove a container:

  ```bash
  sudo docker rm <container_id_or_name>
  ```

### Step 8: Verify Public Access

1. Get EC2 public IP:

   ```bash
   curl http://checkip.amazonaws.com
   ```
2. Open your browser and go to:

   ```
   http://<your-ec2-public-ip>:8000/
   ```

   → Your Dockerized app should be accessible.

### Step 9: Common Errors & Fixes

1. **Command not found (Windows issues)**

   * Windows CMD doesn’t recognize `ls` or `chmod`. Use `dir` or do it inside EC2.

2. **Docker permission denied**

   * Always run Docker commands with `sudo` or add your user to the Docker group.

3. **Docker login unauthorized**

   * Use the correct Docker Hub credentials or a Personal Access Token (PAT).

4. **Port already allocated**

   * Stop and remove any container using that port:

     ```bash
     sudo docker stop <container_id>
     sudo docker rm <container_id>
     ```

5. **MongoDB connection errors**

   * Make sure your EC2 public IP is whitelisted in MongoDB Atlas.
   * Use SSL/TLS connection if required by your DB.
