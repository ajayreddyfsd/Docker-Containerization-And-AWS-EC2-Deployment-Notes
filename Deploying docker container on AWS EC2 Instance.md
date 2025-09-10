# Deploying a Docker Container on Amazon EC2 Inastance

## Step-by-Step Guide

### Step 1: Create an EC2 Instance

1. Go to the **AWS EC2 Dashboard** → **Launch a very very simple Instance**.
2. Configure **security group (SG)**:
   - Allow **SSH** (port 22) from your IP in the SG inbound rules
   - Allow the **port the docker image is created on** (e.g., custome TCP @ PORT 8000) from anywhere (`0.0.0.0/0`) so it’s publicly accessible. in the SG inbound rules.
     **why PORT 8000?, coz thats what we built out backend on and thats what we used while creating docker image**
3. **Launch instance** and **download the `.pem` key file** for SSH connection.

### Step 2: Connect to EC2 via SSH

1. Open terminal.
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
   sudo yum install docker
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

1. Login using your Docker Hub credentials, the email and password:

   ```bash
   sudo docker login
   ```

2. If successful, you are ready to pull and run Docker images.

### Step 6: Pull and Run Your Docker Image

1. Pull your Docker image and run it:

   ```bash
   sudo docker run --restart=always -p 8000:8000 your-docker-hub-username/your-image-name
   ```

   - `--restart=always` → ensures container restarts if EC2 reboots.
   - `-p 8000:8000` → 8000:8000 is what we used while creating docker image, we will use that same thing here.

2. Check running containers:

   ```bash
   sudo docker ps
   ```

### Step 7: Stop/Remove Containers (if needed)

- Stop a container:

  ```bash
  sudo docker stop <container_id_or_name>
  ```

- Remove a container:

  ```bash
  sudo docker rm <container_id_or_name>
  ```

### Step 8: Access the website using the EC2 instance's public IP

1. Get EC2 public IP from AWS EC2 dashboard

2. Open your browser and go to:

   ```
   http://<your-ec2-public-ip>:8000/
   ```

   → Your Dockerized app should be accessible.

### Step 9: Common Errors & Fixes

1. **Docker permission denied**

   - Always run Docker commands with `sudo` or add your user to the Docker group.

2. **Port already allocated**

   - Stop and remove any container using that port:

     ```bash
     sudo docker stop <container_id>
     sudo docker rm <container_id>
     ```

3. **MongoDB connection errors**

   - Make sure your EC2 public IP is whitelisted in MongoDB Atlas cluster's network access.
   - Use SSL/TLS connection if required by your DB.

4. **http instead of https**
5. **:8000 in the ec2 public IP**
6. **Appropriate inbound rules in the EC2 instance's security group**
