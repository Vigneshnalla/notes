
# Complete Configuration and Setup Guide

This guide includes essential commands, configurations, and setup instructions for managing NGINX, system services, networking, and other project-related details.

---

## NGINX Server Setup

To Edit the NGINX HTML file using nano:
```bash
sudo nano /usr/share/nginx/html/index.html
```
To save changes, press `Ctrl + X`, then `Y`, and `Enter`.

To delete and recreate the HTML file:
```bash
rm /usr/share/nginx/html/index.html
sudo nano /usr/share/nginx/html/index.html
```
Enter the desired HTML content, then save with `Ctrl + X`, `Y`, and `Enter`.

Edit the main NGINX configuration file:
```bash
sudo nano /etc/nginx/nginx.conf
```

### Important Paths for NGINX
- Main Configuration: `/etc/nginx/nginx.conf`
- Additional Configuration: `/etc/nginx/conf.d/`
- Default Web Root: `/usr/share/nginx/html`
- Logs:
  - Access Logs: `/var/log/nginx/access.log`
  - Error Logs: `/var/log/nginx/error.log`

---

## Systemd and Logging

### Logging and Service Management
- **Syslog Identifier**:
  - `SyslogIdentifier=backend` - This defines how logs for this service will be labeled in system logs (e.g., `/var/log/syslog` or `journalctl`).

- **Check Logs**:
  ```bash
  sudo journalctl -u backend
  ```
  - `-u` stands for unit.

---

## Networking

We have one common public IP for all devices within our modem. If we want to connect to other mobiles within the same network, the modem first checks if it is in the network; if not, it contacts the ISP. Within the network, we use private IPs for network connections.

### Testing Connectivity
- **Ping a Device**:
  ```bash
  ping -c 4 <ip_address>
  ```

- **Telnet to Check Port Connectivity**:
  ```bash
  telnet <ip_address> <port_number>
  ```
  - Check if it can connect to that port or not.

---

## AWS Networking Example

```hcl
resource "aws_route" "public" {
  route_table_id = aws_route_table.public.id
  destination_cidr_block = "0.0.0.0/0"
}
```
This enables instances in the associated subnet to communicate with the internet. For example, if an EC2 instance in this subnet tries to reach a public IP (e.g., 8.8.8.8), the traffic will be routed via the Internet Gateway. This setup is essential for public subnets where the EC2 instances need internet access.

---

## Expense Project

### AMI
- AMI (Amazon Machine Image): An image that includes the OS and all installed packages.

### User and Group Management
- List Users:
  ```bash
  cat /etc/passwd
  ```
- List Groups:
  ```bash
  getent group
  ```
- Edit SSH Configuration:
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
- Restart SSH Service:
  ```bash
  sudo systemctl restart sshd
  ```

### MySQL Management
- Check Running MySQL Processes:
  ```bash
  ps -ef | grep mysql
  ```
- Set MySQL Root Password:
  ```bash
  mysql_secure_installation --set-root-pass ExpenseApp@1
  ```

### Network Status
- Check Listening Ports:
  ```bash
  netstat -lntp
  ```

---

## Domain and DNS Management

### AWS Route 53
- **Nameserver Change**: By setting AWS Route 53 nameservers in Hostinger, AWS Route 53 now manages all DNS queries for your domain, directing internet traffic based on the authoritative DNS records set in Route 53.

---

# End of Guide
