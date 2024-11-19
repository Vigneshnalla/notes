### Puppet vs Ansible

#### Puppet
- **Architecture**: 
  - Master-Agent Model
    - **Puppet Master**: Central server that stores all configuration files (manifests) and modules.
    - **Puppet Agents**: Installed on managed nodes (servers); they periodically check in with the Puppet Master to retrieve configurations and apply them.
- **Communication**: 
  - Pull Model: Agents pull configurations from the Puppet Master at regular intervals.
- **Configuration Language**: 
  - Domain-Specific Language (DSL): Uses its own syntax to define system configurations.
- **Management Style**: 
  - Focused on enforcing the desired state of the system.

#### Ansible
- **Architecture**: 
  - Agentless Model: Does not require any agent installed on managed nodes; communicates directly via SSH (or WinRM for Windows).
- **Communication**: 
  - Push Model: Configurations are pushed to the managed nodes from the control machine when a playbook is executed.
- **Configuration Language**: 
  - YAML: Uses human-readable YAML syntax to define playbooks for automation tasks.
- **Management Style**: 
  - Procedural approach, executing tasks in the order defined in the playbooks.

### Summary
- **Puppet**: Suited for environments that require a robust master-agent architecture and strict enforcement of configurations.
- **Ansible**: Ideal for simpler, agentless setups where ease of use and quick deployments are prioritized.


### To develop custom modules in ansible
    we have to use python

### asnible supports both linux and window using ssh and winrm


## push vs pull

### Does Ansible Support AWS, Azure, and GCP?

   Yes, Ansible supports AWS, Azure, and GCP. It doesn’t matter what the cloud provider is; Ansible only checks if the resources are accessible via a public IP and whether they can be reached over SSH.