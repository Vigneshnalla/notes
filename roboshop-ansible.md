
# Roboshop Ansible Automation Guide

## Overview

This document outlines the steps to automate the deployment and configuration of the Roboshop application using Ansible. The setup includes provisioning servers, setting up Route53 records, configuring various services (MongoDB, Redis, MySQL, RabbitMQ, etc.), and ensuring the application components function as expected.

---

## Step 1: Setup Ansible Server and Configuration

1. **Create the Ansible server** and install Ansible:
   ```bash
   sudo yum install ansible -y
   ```

2. **Edit Ansible configuration** in `ansible.cfg` to avoid specifying inventory and credentials during each command:
   ```ini
   [defaults]
   inventory = /home/ec2-user/roboshop-ansible-roles/inventory
   remote_user = ec2-user
   private_key_file = /path/to/your/private-key.pem
   host_key_checking = False
   ```

3. **Set Ansible configuration environment variable**:
   ```bash
   export ANSIBLE_CONFIG=/home/ec2-user/roboshop-ansible-roles/ansible.cfg
   ```

---

## Step 2: Provision Servers and Configure Route53

1. **Create 12 servers** to host different services for Roboshop.
2. **Set up Route53 records** to map IP addresses for each server, ensuring DNS resolution.

---

## Step 3: Deploy Components Using Ansible Playbooks

Run all playbooks as the `sudo` user to ensure correct permissions.

---

### MongoDB Setup

1. **Run MongoDB playbook** to set up MongoDB:
   ```bash
   ansible-playbook main.yaml -e "component=mongodb"
   ```

---

### Catalogue Service

The Catalogue service fetches product details from MongoDB.

1. **Deploy Catalogue Component**:
   ```bash
   ansible-playbook main.yaml -e "component=catalogue"
   ```

2. **Verify MongoDB configuration because data is added to MongoDB during the execution of the catalogue role**:
   - Connect to MongoDB to confirm accessibility:
     ```bash
     mongosh "mongodb://mongodb.vigneshdev.online:27017/?directConnection=true&serverSelectionTimeoutMS=2000"
     ```
   - Switch to the `catalogue` database and check for `products` collection:
     ```javascript
     use catalogue
     db.products.find()
     ```

---

### Web Service

The Web service displays products on the frontend.

1. **Deploy Web Component**:
   ```bash
   ansible-playbook main.yaml -e "component=web"
   ```

2. **Verify Web Configuration**:
   - Confirm `/etc/nginx/default.d/roboshop.conf` is updated correctly.
   - Test Nginx configuration:
     ```bash
     nginx -t
     sudo tail -n 20 /var/log/nginx/error.log
     ```
   - Ensure the web host is running and check whether is it able to connect with catalogue service or not.
   ```bash
    curl http://web.vigneshdev.online/api/catalogue/categories
    ```

---

## User and Cart Services (MongoDB and Redis)

### Redis Setup

1. **Deploy Redis Component**:
   ```bash
   ansible-playbook main.yaml -e "component=redis"
   ```

---

### Cart Service

1. **Deploy Cart Component** (requires Redis):
   ```bash
   ansible-playbook main.yaml -e "component=cart"
   ```

---

### User Service

1. **Deploy User Component** (depends on MongoDB and Redis):
   ```bash
   ansible-playbook main.yaml -e "component=user"
   ```

**Update Web Configuration**:
   - Add entries in `/etc/nginx/default.d/roboshop.conf`.
   - Run the Web component again:
     ```bash
     ansible-playbook main.yaml -e "component=web"
     ```

---

## MySQL, RabbitMQ, Shipping, and Payment Services

These services handle the Roboshop application's backend processes related to order and payment management.

- **MySQL**: Manages data storage for critical transactions, such as order details.
- **RabbitMQ**: Acts as a messaging broker, enabling services to communicate seamlessly, especially for asynchronous tasks.
- **Shipping Service**: Manages product shipment details within the order fulfillment process.
- **Payment Service**: Handles customer payments, verifying and processing transactions.

Execute each Ansible playbook for these services as follows:

1. **Deploy MySQL**:
   ```bash
   ansible-playbook main.yaml -e "component=mysql"
   ```

2. **Deploy RabbitMQ**:
   ```bash
   ansible-playbook main.yaml -e "component=rabbitmq"
   ```

3. **Deploy Shipping**:
   ```bash
   ansible-playbook main.yaml -e "component=shipping"
   ```
Verify MySQL connection for Shipping service because it fetches country and shipping codes from MySQL:

Check MySQL connection:
   ```bash
      mysql -h mysql.vigneshdev.online -uroot -pRoboShop@1
   ```

If any errors occur related to the Shipping service, navigate to the Shipping service directory and manually run:
```bash
sudo /bin/java -jar /app/shipping.jar
```
This will allow Spring to run the application directly, helping you identify and troubleshoot any errors in the Shipping project.
Also, check if the records and codes are properly inserted into MySQL.






4. **Deploy Payment**:
   ```bash
   ansible-playbook main.yaml -e "component=payment"
   ```

5. **Update Web Configuration**:
   - Add necessary entries to `/etc/nginx/default.d/roboshop.conf`.
   - Run the Web component once more:
     ```bash
     ansible-playbook main.yaml -e "component=web"
     ```

---

## Summary

Following this guide, you should have a fully configured Roboshop environment with all services deployed and functional. Verify each component as outlined to ensure proper setup.





# Troubleshooting Steps
If you try to access Route 53 records, you may encounter issues with the host. To resolve this, go directly to the service and execute this command in that service

## 1. Update DNS Resolver
```bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

## 2. Check Open Ports
```bash
netstat -lntp
```

## 3. Check Running Processes
```bash
ps -ef | grep <your_process>
```

## 4. Check Service Status
```bash
systemctl status <service_name>
```

## 5. Test Host Resolution
```bash
telnet catalogue.vigneshdev.online
```

## 6. Verify Nginx Configuration
```bash
cat /etc/nginx/default.d/roboshop.conf
sudo nginx -t && sudo systemctl reload nginx
```

## 7. Check DNS Resolution
Use the following commands to verify DNS resolution:
```bash
dig catalogue.vigneshdev.online
nslookup catalogue.vigneshdev.online
```

---

Ensure DNS resolution works and the services are correctly running to resolve the host and establish connectivity.
