## What is devops
- Devops is culture that helps organizations to improve the delivers of applications by ensuring proper automation with code  quality is maintained with in the applciation  and ensuring  continous monitoring and continous testing .By integrating these processes, DevOps allows teams to deliver applications to customers with high standards of reliability and performance.

## why devops
- Before DevOps, delivering an application to end users could take months, as various engineers—such as build and release engineers, test engineers, and others and also there is no proper communication betweent them 

  for suppose there is a hot fix in our application like the price of bus ticket  is  1000rs but instead of that it is showing  10 rs
  To prevent this, we need an immediate fix other wise we loss so much amount without devops it is not possible to sovle within days


DevOps solves this by enabling faster release cycles through CI/CD pipelines and allowing for rapid deployment of urgent fixes.













## Command Substitution in Shell Scripting
- **Explanation**: Command substitution is a process that allows you to replace a command with its output. It's achieved by enclosing the command in backticks (`` ` ``) or using `$(command)`.

## Internet Gateway
- An Internet Gateway enables resources in public subnets (which have public IPs) to access the internet for updates or external services.

## NAT Gateway
- **Location**: Typically placed in a public subnet.
- **Use Cases**: 
  - Fetching updates.
  - Downloading packages from the internet.
- A NAT Gateway enables resources in private subnets (which don’t have public IPs) to access the internet for updates or external services. 
- It does not allow inbound connections from the internet.

## IP Address
- An IP address (Internet Protocol address) is a unique numerical label assigned to each device connected to a computer network that uses the Internet Protocol for communication.
- **IP Address Formats**:
  1. **IPv4 (32-bit)**: e.g., `8.8.8.8` (allows for approximately 4.3 billion unique addresses).
  2. **IPv6 (128-bit)**: e.g., `29ad:sfd8:...` (divided into 8 groups of 4 hexadecimal digits separated by colons).
  
### Purposes of an IP Address
1. **Network ID**: Identifies a specific network.
2. **Host ID**: Identifies a specific device (host) within that network.

- **Public IP**: Visible to external networks.
- **Private IP**: Used within internal networks like home or office; not accessible from the internet directly.

## Subnet Mask
- A subnet mask is used to separate the network address from the host address, allowing a router to quickly determine where to forward a packet.
  
### Benefits of Subnet Mask
- Makes networks easier to maintain.
- Routes packets effectively.
  
### Packet Forwarding Example
- When a router receives a data packet, it examines the destination IP address included in the packet header.
- By applying the subnet mask to the destination IP address, the router can determine which part of the address refers to the network and which part refers to the host.
- **Example**: If Device A wants to send a packet to Device B and checks the destination IP address (`192.168.1.20`):
  - Since both devices share the same network portion (`192.168.1`), Device A can send the packet directly to Device B without needing to go through a router.
