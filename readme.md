# Ansible Automation Guide for Multi-Server Setup

## 📌 Overview

Ansible is a powerful open-source automation tool used for configuration management, application deployment, and task automation across multiple servers. It uses a simple YAML-based syntax to describe tasks in playbooks and connects via SSH without requiring agents.

---

## ⚙️ How Ansible Works

1. **Inventory**: Defines target servers (hosts) to manage.
2. **Playbooks**: YAML files that describe automation steps.
3. **Modules**: Pre-built units (e.g., apt, yum, shell) that perform tasks.
4. **SSH-Based**: No agents needed on the remote hosts.
5. **Idempotency**: Re-running playbooks won't cause redundant changes.

---

## 🛠️ Installation

### On Ubuntu/Debian

```bash
sudo apt update
sudo apt install ansible -y
```

### On RHEL/CentOS

```bash
sudo yum install epel-release -y
sudo yum install ansible -y
```

### Check version

```bash
ansible --version
```

---

## 📁 Directory Structure

```
Ansible-Practice/
├── inventory.ini              # Host inventory file
├── playbook.yml              # Main playbook
├── roles/                    # (optional) Role-based structure
│   └── webserver/
│       ├── tasks/
│       │   └── main.yml
│       └── handlers/
│           └── main.yml
└── README.md                 # This guide
```

---

## 🧩 Essential Files Explained

### inventory.ini

Defines the servers and their login users:

```ini
[local]
192.168.0.29 ansible_user=imran ansible_ssh_private_key_file=~/.ssh/id_ed25519
192.168.0.30 ansible_user=root ansible_ssh_private_key_file=~/.ssh/id_ed25519
192.168.0.31 ansible_user=imran ansible_ssh_private_key_file=~/.ssh/id_ed25519

[remote]
43.253.154.210 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/id_ed25519 
```

### playbook.yml

Describes automation steps:

```yaml
- name: Setup all necessary tools on multiple servers
  hosts: all
  become: true
  tasks:
    - name: Install packages
      apt:
        name: nginx
        state: present
```

---

## 🚀 How Multi-Server Installation Works

- Ping using:

```bash
ansible-playbook -i inventory.ini all -m ping
```


- Run using:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

- Executes tasks on **all defined hosts in parallel**.
- Uses SSH to connect and run commands.
- All tasks are executed in the order they appear in the playbook.

---

## ✅ Use Cases

- Automate provisioning of new servers
- Set up monitoring agents (Prometheus, Node Exporter)
- Install developer tools (Node.js, NVM, Jenkins)
- Configure databases (PostgreSQL, MySQL)
- Patch/update software across fleet

---

## 📎 Tips

- Use `ansible_user` in `inventory.ini` to avoid hardcoding users
- Use `creates:` in shell tasks to avoid repeating installations
- Run with `--check` for dry-run mode
- Use `--limit` to run on specific hosts

---

## 🧠 Final Thoughts

Ansible's agentless architecture and human-readable syntax make it ideal for managing infrastructure across multiple environments. Once set up properly, it saves significant time and ensures consistency across all your systems.
