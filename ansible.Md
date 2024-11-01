
# Ansible Configuration and Usage Guide

## Overview
Ansible operates in an agentless architecture, meaning that it runs from a central control node (the machine where Ansible is installed) and communicates with the managed nodes over SSH (or WinRM for Windows).
Means Master need to have ansible software in it then only we able to perform ansible related operatioins

### Installation Command
To install Ansible on the control node, use the following command:
```bash
sudo dnf install ansible -y
```

## Pinging the Target Nodes
To check connectivity with the target nodes, use the following command:
```bash
ansible -i "ipaddress1,ipaddress2," all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping
```
If the target nodes respond with a "pong," it confirms that Ansible can connect successfully.

## Command-Level Commands (Ad-hoc Commands)
You can run ad-hoc commands to perform tasks on the managed nodes. For example, to install Nginx:
```bash
ansible -i "ipaddress1,ipaddress2," all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m dnf -a "name=nginx state=present" -b
```
- `-a`: Specifies the arguments to the module.
- `-b`: Indicates to become root (sudo).

The command will return `changed`, `success`, or `failure`. `Changed` means it made some changes in the connected nodes.

## Playbooks
Playbooks are YAML files that define a series of tasks to be executed on the target nodes. They allow for more complex automation compared to ad-hoc commands.

To run a playbook, use the following command:
```bash
ansible-playbook -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 db.yaml
```

### Gathering Facts
When you run an `ansible-playbook` command, Ansible performs a series of actions before executing the tasks defined in the playbook. One of these initial steps is gathering facts about each host, including information about the distribution and its version. You can use these facts to conditionally execute tasks that are specific to each distribution.

Example of conditionally installing Nginx on Red Hat-based systems:
```yaml
- name: Install Nginx on Red Hat-based systems
  ansible.builtin.yum:
    name: nginx
    state: present
  when: ansible_facts['os_family'] == 'RedHat'
```

## Ansible Roles
Ansible roles are a way to organize and reuse configuration management code in a modular and scalable way. They help to structure your playbooks into reusable components, making them cleaner, easier to maintain, and more manageable, especially in large environments.

### Role Structure
```
roles/
  └── myrole/
      ├── tasks/
      │   └── main.yml        # Main tasks of the role
      ├── handlers/
      │   └── main.yml        # Handlers (e.g., to restart services)
      ├── templates/
      │   └── template.j2      # Jinja2 templates
      ├── files/
      │   └── myfile.conf      # Static files to copy
      ├── vars/
      │   └── main.yml         # Variables specific to the role
      ├── defaults/
      │   └── main.yml         # Default variables
      ├── meta/
      │   └── main.yml         # Metadata and role dependencies
      └── README.md            # Documentation for the role
```

## Configuration File Preferences
Ansible considers configuration file preferences in the following order:
1. **Environment Variable**: You can set an environment variable for configuration:
   ```bash
   export ANSIBLE_CONFIG=/path/to/ansible.cfg
   ```
2. **Current Directory**: If the `ANSIBLE_CONFIG` environment variable is not set, Ansible looks for an `ansible.cfg` file in the current working directory (the directory where you run the Ansible command). This is useful for project-specific configurations.
3. **Home Directory**: If there is no `ansible.cfg` in the current directory, Ansible checks for a configuration file in the user’s home directory (`~/.ansible.cfg`). This is typically used for user-specific settings that should apply regardless of the project.
4. **System-Wide Configuration**: The last option is the system-wide configuration file located at `/etc/ansible/ansible.cfg`.

## Checking Services and Status
After connecting to all nodes, you can check the status of services using the following commands:
```bash
systemctl status mysqld
systemctl status backend
systemctl status frontend
systemctl status nginx
netstat -lntp
telnet db.vigneshdev.online 33006
telnet db.vigneshdev.online 8080
mysql -h db.vigneshdev.online -u root -pExpenseApp@1
```

## Tags
In Ansible, tags allow you to control which tasks are executed during a playbook run. Tags enable you to selectively run specific portions of a playbook without executing the entire set of tasks. This can save time and make development and debugging more efficient.

### Using Tags in Playbooks
To run tasks with tags, use the following commands:
```bash
ansible-playbook playbook.yml --tags "webserver"
ansible-playbook playbook.yml --skip-tags "database"
ansible-playbook playbook.yml --tags "webserver,database"
```
You can mention tags in tasks like this:
```yaml
- name: Disable default Node.js
  ansible.builtin.command: dnf module disable nodejs -y
  tags:
    - nodejs

- name: Enable Node.js:20
  ansible.builtin.command: dnf module enable nodejs:20 -y
  tags:
    - nodejs
```


# Ansible: Pull and Push Models

In Ansible, you have two primary modes of operation: **pull** and **push**. Here's an explanation of both:

## 1. Ansible Push

The **push** model is the default way Ansible operates. In this mode, the control node  pushes configurations to managed nodes.

### How it works:
- You run Ansible playbooks from the control node.
- The control node connects to the managed nodes over SSH.
- Ansible executes the necessary tasks directly on the managed nodes.

### Example:
```bash
ansible-playbook -i inventory_file playbook.yml
```
In the command:
- `-i inventory_file`: Specifies the inventory file with the target hosts and credentials.
- `playbook.yml`: The playbook that contains the tasks to be executed.

## 2. Ansible Pull

The **pull** model is less common but useful in specific scenarios where managed nodes fetch the configuration from a central repository instead of receiving them from a control node.

### How it works:
- Each managed node runs the `ansible-pull` command.
- It clones a repository (usually Git) containing the playbooks.
- The managed node executes the playbook locally.

### Example:
```bash
ansible-pull -U https://github.com/your-repo/your-playbook-repo.git playbook.yml
```
In the command:
- `-U https://github.com/your-repo/your-playbook-repo.git`: Specifies the Git repository containing the playbooks.
- `playbook.yml`: The playbook to be executed.

## When to Use Each Model

- **Push**:
  - Good for centralized management.
  - Suitable when you have control over when and how the configurations are applied.
  - The control node initiates actions.

- **Pull**:
  - Useful in environments with intermittent network connectivity.
  - In the Ansible Pull model, each managed node is responsible for deciding when to update its configuration.
  - Better for large-scale deployments where nodes can operate independently and manage their configurations.
- **Resilience**: If a node is offline or loses connection temporarily, it can update itself once it's back online, without missing any updates.

These methods offer flexibility depending on your environment and operational needs!
