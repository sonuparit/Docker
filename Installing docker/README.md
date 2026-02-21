# 🐳 Docker Installation Guide on Rocky Linux (Step-by-Step)

This guide provides a complete step-by-step process to install Docker on
**Rocky Linux** in a clean and production-ready way.

Tested on: - Rocky Linux 10.1 (Red Quartz)

**Check your OS version:**
>
>```
>cat /etc/os-release
>```

------------------------------------------------------------------------

## 📌 Step 1: Update System Packages

Always start by updating your system:

``` bash
sudo dnf update -y
```

------------------------------------------------------------------------

## 📌 Step 2: Install Required Dependencies

``` bash
1.
sudo dnf install -y dnf-plugins-core

    → (may not be required after update)

2.
sudo dnf install -y yum-utils device-mapper-persistent-data lvm2
```

------------------------------------------------------------------------

## 📌 Step 3: Add Docker Official Repository

``` bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```

Rocky Linux is binary-compatible with rhel, so we use the rhel
Docker repository.

------------------------------------------------------------------------

## 📌 Step 4: Install Docker Engine

``` bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

------------------------------------------------------------------------

## 📌 Step 5: Start Docker Service

``` bash
sudo systemctl start docker
```

Enable Docker to start automatically at boot:

``` bash
sudo systemctl enable docker
```

------------------------------------------------------------------------

## 📌 Step 6: Verify Docker Installation

Check Docker version:

``` bash
docker --version
```

------------------------------------------------------------------------

## 📌 Step 7: Run Docker Without sudo (Recommended)

Add your user to the Docker group:

``` bash
sudo usermod -aG docker $USER
```

------------------------------------------------------------------------

## 📌 Step 8: Apply group changes

``` bash
newgrp docker

    → (Important if you want to avoid log off and log in)
    → (This starts new Instance of bash terminal with GID set to docker)
```

------------------------------------------------------------------------

## 📌 Step 9: Verify Docker Installation

Run a status check for Docker:

``` bash
systemctl status docker

    → should be active (running)
```

Check Docker version:

``` bash
docker --version
```

------------------------------------------------------------------------

## 🔐 Optional: Configure Firewall (If Needed)

> [!NOTE]   
> At the end Don't forget to aplly the changes:  
```
sudo firewall-cmd --reload
```

 - If running containers with exposed ports. Open specific common web ports:

``` bash
sudo firewall-cmd --add-port=8080/tcp --permanent
```
```
sudo firewall-cmd --permanent --add-port=80/tcp
```
```
sudo firewall-cmd --permanent --add-port=443/tcp
```

 - Open a wide range of ports for development (8000-9000):

``` bash
sudo firewall-cmd --permanent --add-port=8000-9000/tcp
```

 - Trust the Docker network interface to prevent internal blocks:

``` bash
sudo firewall-cmd --permanent --zone=trusted --add-interface=docker0
```

 - Enable masquerading for container ingress/egress:

``` bash
sudo firewall-cmd --zone=public --add-masquerade --permanent
```

 - Completely Disable firwall (not recommended):

    -   a. Stop the firewall:

``` bash
		sudo systemctl stop firewalld
```
- 
    - b. Disable the firewall (keep it off after reebot)
    

``` bash
		sudo systemctl disable firewalld
```
- Download it in bash script

``` bash
cat << 'EOF' > setup-firewall.sh
#!/bin/bash
## 1. Open a wide range of ports for development (8000-9000)
#sudo firewall-cmd --permanent --add-port=8000-9000/tcp
#
## 2. Open specific common web ports
#sudo firewall-cmd --permanent --add-port=80/tcp
#sudo firewall-cmd --permanent --add-port=443/tcp
#
## 3. Trust the Docker network interface to prevent internal blocks
#sudo firewall-cmd --permanent --zone=trusted --add-interface=docker0
#
## 4. Enable masquerading for container ingress/egress
#sudo firewall-cmd --zone=public --add-masquerade --permanent
#
## 5. Disable firwall:
#	a. Stop the firewall
#		sudo systemctl stop firewalld
#
#	b. Disable the firewall (keep it off after reebot)
#		sudo systemctl disable firewalld
#
## 5. Apply changes
#sudo firewall-cmd --reload
#
## 6. List all active firewall settings, including open ports and services:
#sudo firewall-cmd --list-all
#
## 7. List only explicitly added open ports:
#sudo firewall-cmd --list-ports
#
## 8. Check if a specific port is open:
#sudo firewall-cmd --query-port=<port-number>/<protocol>
# Example:
#sudo firewall-cmd --query-port=80/tcp
#
#echo "Firewall automated for Docker development!"
#EOF
#
```

------------------------------------------------------------------------

## 🧹 Optional: Remove Docker (Uninstall)

If you ever need to remove Docker:

``` bash
sudo dnf remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

------------------------------------------------------------------------

## 🚀 Next Steps

After installation, you can:

-   Build your first Docker image
-   Run Nginx or Node.js container
-   Learn Dockerfile basics
-   Explore Docker Compose
-   Integrate Docker into CI/CD pipelines

------------------------------------------------------------------------

## 🧠 Best Practices

-   Avoid using `latest` tag in production
-   Keep system updated
-   Regularly prune unused images and containers
-   Monitor Docker service logs

------------------------------------------------------------------------

🎯 You now have Docker successfully installed on Rocky Linux.
