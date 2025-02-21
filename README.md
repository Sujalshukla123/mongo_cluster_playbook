# MongoDB Cluster Deployment Project

This Ansible project automates the deployment and configuration of a MongoDB replica set cluster. It provides a robust, secure, and production-ready MongoDB deployment with built-in replication and authentication.

## Project Structure 
├── README.md
├── inventory/
│ └── hosts # Inventory file containing server definitions
├── group_vars/
│ └── all.yml # Global variables
├── roles/
│ └── mongodb_cluster/ # MongoDB cluster role
│ ├── tasks/
│ │ └── main.yml # Main tasks for MongoDB deployment
│ ├── templates/
│ │ ├── config_tem.j2 # MongoDB configuration template
│ │ ├── replicate_script.j2 # Replica set initialization script
│ │ └── mongo_admin.j2 # Admin user creation script
│ └── handlers/
│ └── main.yml # Role handlers
└── site.yml # Main playbook
```

## Prerequisites

- Target servers running Ubuntu 20.04 LTS
- Ansible 2.9 or higher on the control node
- SSH access to target servers
- Sudo privileges on target servers

## Features

### MongoDB Cluster Role

The `mongodb_cluster` role performs the following tasks:

1. **Installation**
   - Adds MongoDB 4.4 repository and GPG key
   - Installs required dependencies (libssl1.1)
   - Installs MongoDB 4.4 Community Edition
   - Prevents automatic MongoDB updates

2. **Security Configuration**
   - Generates and distributes authentication keyfile
   - Sets up inter-node authentication
   - Creates admin user with secure credentials
   - Enables authentication in MongoDB configuration

3. **Cluster Configuration**
   - Configures MongoDB for replica set operation
   - Initializes the replica set
   - Sets up proper permissions and ownership

## Configuration

### Inventory Setup

Define your MongoDB servers in the inventory file:

```ini
[mongodb_servers]
mongo1 ansible_host=192.168.1.101
mongo2 ansible_host=192.168.1.102
mongo3 ansible_host=192.168.1.103

[mongodb_servers:vars]
pri=mongo1
```

### Required Variables

Define the following variables in your group_vars or inventory:

- `pri`: Primary node hostname for replica set initialization
- Additional configuration variables as needed for templates
- Adding the public Ip of the instance in the inventory file
- Adding the private Ip in the vars.yml file



## Usage

1. Clone this repository:
```bash
git clone <repository-url>
```

2. Update the inventory file with your server details

3. Run the playbook:
```bash
ansible-playbook -i inventory/hosts site.yml
```

## Security Features

- Keyfile authentication between replica set members
- Admin user creation with role-based access control
- SSL/TLS support for client connections (if configured)
- Secure file permissions for sensitive files

## Maintenance

### Adding New Nodes

1. Add the new node to the inventory file
2. Run the playbook with --limit for the new node
3. Add the node to the replica set using MongoDB commands

### Backup and Restore

- Implement backup solutions using MongoDB tools
- Regular backup scheduling recommended
- Document recovery procedures

## Troubleshooting

Common issues and solutions:

1. **MongoDB Service Won't Start**
   - Check system logs: `journalctl -u mongod`
   - Verify configuration file permissions
   - Ensure ports are available

2. **Replica Set Issues**
   - Verify network connectivity between nodes
   - Check MongoDB logs for replication errors
   - Ensure keyfile permissions are correct



## Author

[Sujal Shukla/ Cloud Solitary]

