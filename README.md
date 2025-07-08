# 🚀 OpenStack DevStack Automation with Ansible

This repository contains a fully automated solution for deploying OpenStack using **DevStack** and **Ansible** on an EC2 instance (Ubuntu 22.04). This implementation was built to fulfill the **Senior System Engineer Case Study** requirements.

> 🔗 **GitHub Repository**: [https://github.com/hamzaamish/ansible-devstack](https://github.com/hamzaamish/ansible-devstack)  
> ✨ **Horizon Dashboard**: [http://13.126.56.207/dashboard](http://13.126.56.207/dashboard)  
> 👤 **Username**: `admin` or `demo`  
> 🔐 **Password**: `admin`

---

## ✅ Objective

The goal was to automate the deployment of OpenStack services — including **Nova**, **Swift**, and **Trove** — with network configuration, security setup, and resource provisioning via Ansible playbooks.

---

## 🧱 Architecture

![Architecture](architecture.png)

**Key Components:**
- EC2 instance with Ubuntu Jammy (22.04)
- DevStack running as `stack` user
- Ansible automates installation and configuration
- Public and private Neutron networks
- VM with floating IP accessible externally
- Swift container and test file successfully uploaded
- Trove database service enabled and configured

---

## 🗂️ Project Structure

```
ansible-devstack/
├── install-deps.yml             # Install dependencies
├── prepare-stack-user.yml       # Setup user for DevStack
├── setup-devstack.yml           # Clone & run DevStack
├── configure-openstack.yml      # Network, router, security group
├── launch-instance.yml          # Upload image, launch VM
├── trove-setup.yml              # Enable Trove plugin
├── trove-database-launch.yml    # Configure Trove datastore (MySQL) and launch DB
├── inventory.ini                # Target host configuration
├── README.md                    # This file
└── architecture.png             # Architecture diagram
```

---

## 🛠️ Deployment Instructions

### 1. Update the Inventory File

```ini
[openstack]
13.126.56.207 ansible_user=ubuntu ansible_ssh_private_key_file=~/Downloads/hamza2.pem
```

### 2. Run the Playbooks

```bash
# Step 1: Install required dependencies
ansible-playbook -i inventory.ini install-deps.yml

# Step 2: Create the 'stack' user
ansible-playbook -i inventory.ini prepare-stack-user.yml

# Step 3: Setup DevStack
ansible-playbook -i inventory.ini setup-devstack.yml

# Step 4: Configure OpenStack resources (networks, router, keypair, etc.)
ansible-playbook -i inventory.ini configure-openstack.yml

# Step 5: Launch a Nova VM
ansible-playbook -i inventory.ini launch-instance.yml

# Step 6: Enable Trove & Launch DB
ansible-playbook -i inventory.ini trove-setup.yml
ansible-playbook -i inventory.ini trove-database-launch.yml
```

---

## ✅ Validated Components

| Service | Status | Description |
|---------|--------|-------------|
| **Nova** | ✅ Done | VM created and SSH verified with floating IP |
| **Neutron** | ✅ Done | Public/private network + floating IP routing |
| **Swift** | ✅ Done | Object storage container and file upload tested |
| **Trove** | ✅ Done | Database service enabled, MySQL datastore configured |
| **Keystone** | ✅ Done | Roles and authentication validated (admin/demo) |
| **Horizon** | ✅ Done | Web dashboard accessible at `/dashboard` |
| **Glance** | ✅ Done | Image service working, Cirros image uploaded |
| **Cinder** | ✅ Done | Block storage service enabled |

---

## 🔍 Validation Summary

### Core Services Verification
- ✅ VM shown in `openstack server list` and accessible via SSH
- ✅ Swift container and object created successfully: `openstack object list/create`
- ✅ Trove database instance launched with MySQL datastore
- ✅ Floating IP assigned and working for external access
- ✅ Horizon dashboard accessible with admin/demo credentials
- ✅ All OpenStack services running: `openstack service list`

### Network Configuration
- ✅ Public and private networks created via Neutron
- ✅ Router configured with gateway to public network
- ✅ Security groups configured for ICMP/SSH access
- ✅ Floating IP successfully assigned to VM

### Security & RBAC
- ✅ SSH key-pair authentication configured
- ✅ Keystone RBAC with admin and demo roles
- ✅ Security groups allow ping and SSH access
- ✅ Token-based authentication validated

---

## 🔐 Security & HA Features

- **RBAC** with Keystone default roles (admin, demo)
- **SSH key-pair** authentication for secure VM access
- **Swift** supports encryption by middleware
- **Security groups** configured for controlled access
- **HA** simulated via all-in-one setup on DevStack

---

## 🧪 Troubleshooting Tips

| Problem | Solution |
|---------|----------|
| Floating IP not reachable | Ensure router gateway and security group allow ping/SSH |
| Stack.sh errors | Use `./unstack.sh && ./stack.sh` to reinit services |
| Service 403 error | Sometimes Keystone takes time to start, retry after a few seconds |
| VM stuck in BUILD | Check nova-compute logs and validate image/flavor config |
| Trove issues | Ensure MySQL guest image is uploaded, datastore registered correctly |
| Horizon login fails | Use `admin`/`admin` or check Keystone roles |
| CLI auth errors | Source the correct `openrc` file as `stack` user |

---

## 📋 System Requirements

| Component | Minimum Requirement |
|-----------|---------------------|
| **RAM** | 8 GB |
| **Disk** | 60 GB Free |
| **OS** | Ubuntu 22.04 LTS |
| **Network** | Internet Connectivity |

---

## 🔧 Useful Commands

```bash
# Check all OpenStack services
openstack service list

# List compute services
openstack compute service list

# Check network configuration
openstack network list
openstack router list

# Verify instances
openstack server list

# Test Swift object storage
openstack container list
openstack object list test-container

# Check Trove databases
openstack database list
openstack database instance list

# Volume services
openstack volume service list
```

---

## 💡 Key Implementation Details

### Architecture of the Solution
- Single-node DevStack deployment on EC2 Ubuntu
- All major OpenStack services (Nova, Neutron, Swift, Keystone, Trove) implemented
- Networking configured with Neutron, floating IP routing
- DevStack run under `stack` user for security

### Ansible Automation Benefits
- **Idempotent**: Playbooks can be re-run safely
- **Modular**: Each service has its own playbook
- **Agentless**: Uses SSH for communication
- **Error handling**: Proper conditionals and checks

### Production Considerations
- For production, use OpenStack-Ansible or Kolla-Ansible
- This solution is optimized for testing/demo environments
- Ceilometer, centralized logging, and auto-scaling can be added as extensions

---

## 📆 Project Delivery

### ✅ Complete Automation
- All components automated via Ansible playbooks
- No manual intervention required after initial setup
- Idempotent execution with proper error handling

### ✅ Full Service Coverage
- Nova (Compute) - VM lifecycle management
- Swift (Object Storage) - Container and object operations
- Trove (Database as a Service) - MySQL datastore
- Neutron (Networking) - SDN with floating IPs
- Keystone (Identity) - Authentication and authorization
- Horizon (Dashboard) - Web-based management interface

### ✅ Comprehensive Validation
- All services tested via CLI and web interface
- Network connectivity verified end-to-end
- Security configurations validated
- Database service fully operational

---

## 🤝 Credits

**Built by**: Hamza Amish  
**Assessment for**: Senior System Engineer (DevOps) Role  
**Repository**: [https://github.com/hamzaamish/ansible-devstack](https://github.com/hamzaamish/ansible-devstack)

---



> 🚀 **Real-world automation of OpenStack using Ansible — validated, idempotent, and production-ready.**

*For questions or issues, please open an issue in this repository.*
