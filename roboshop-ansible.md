
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

2. **Verify MongoDB Configuration**:
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

### Catalogue Service

The Catalogue service fetches product details from MongoDB.

1. **Deploy Catalogue Component**:
   ```bash
   ansible-playbook main.yaml -e "component=catalogue"
   ```

2. **Confirm MongoDB Connectivity**:
   Ensure Catalogue can access MongoDB and retrieve products data.

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
     ```
   - Ensure the web host is running.

---

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

2. **Update Web Configuration**:
   - Add entries in `/etc/nginx/default.d/roboshop.conf`.
   - Run the Web component again:
     ```bash
     ansible-playbook main.yaml -e "component=web"
     ```

---

### MySQL, RabbitMQ, Shipping, and Payment Services

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

4. **Deploy Payment**:
   ```bash
   ansible-playbook main.yaml -e "component=payment"
   ```

5. **Update Web Configuration**:
   - Add necessary entries in `/etc/nginx/default.d/roboshop.conf`.
   - Run the Web component once more:
     ```bash
     ansible-playbook main.yaml -e "component=web"
     ```

---

## Summary

Following this guide, you should have a fully configured Roboshop environment with all services deployed and functional. Verify each component as outlined to ensure proper setup.
