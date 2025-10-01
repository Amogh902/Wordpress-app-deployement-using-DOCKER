#  WordPress Two-Tier Deployment – Traditional vs Dockerized Approach

##  Project Context

This project demonstrates how Docker has **simplified the deployment of a two-tier WordPress application**.

Initially, I deployed WordPress with a MySQL backend manually (traditional way), which involved provisioning infrastructure and configuring services step by step. Later, I replicated the same deployment using **Docker in less than 2 minutes** with just two images: `wordpress` and `mysql`.

This README will walk you through both approaches, highlight the difference, and guide you to replicate the Dockerized deployment on an **AWS EC2 instance**.

---

##  Traditional Deployment (Before Docker)

Steps I had to follow earlier:

* Provision an EC2 instance / VM
* Install & configure MySQL manually
* Install PHP, Apache/Nginx, and WordPress
* Connect WordPress to MySQL
* Troubleshoot dependencies, firewall rules, and networking

 **Challenges:** Time-consuming, prone to human errors, and required deep configuration knowledge.

---

##  Dockerized Deployment (After Docker)

On an EC2 instance with Docker installed, I simply:

### Step 1: Switch to root

```bash
sudo -i
```

*(So I don’t need to prepend every command with `sudo`)*

### Step 2: Run MySQL container

```bash
docker run -d --name mydb \
  -e MYSQL_ROOT_PASSWORD=root@123 \
  -e MYSQL_DATABASE=wordpressdb \
  mysql
```

 **Note:** The `-e MYSQL_ROOT_PASSWORD` flag is **mandatory**. If it’s not set, the MySQL container will be created but exit immediately.

### Step 3: Run WordPress container

```bash
docker run -d --name wordpressapp -p 80:80 \
  -e WORDPRESS_DB_HOST=mydb \
  -e WORDPRESS_DB_USER=root \
  -e WORDPRESS_DB_PASSWORD=root@123 \
  -e WORDPRESS_DB_NAME=wordpressdb \
  --link mydb:mysql \
  wordpress
```

### Step 4: Verify containers

```bash
docker ps
```

You should see both **`mydb`** and **`wordpressapp`** containers running.

### Step 5: Access WordPress

Open a browser → `http://<EC2-Public-IP>` → Complete the WordPress setup → Login to the dashboard.

 **Result:** Entire application ready in minutes with zero manual configuration.

---

##  Conclusion

What once took multiple manual steps can now be achieved with **two Docker commands**. Docker brings **speed, portability, and reliability** to deployments, making it a game-changer for developers and DevOps engineers alike.
