# Experiment 9: Ansible with Docker

## Objective
To understand and implement configuration management using **Ansible** by simulating multiple servers using **Docker** containers.

---

## Theory
Ansible is an open-source automation tool used for configuration management, application deployment, and task automation. It is **agentless** and uses SSH for communication. Tasks are written in YAML-based playbooks which makes automation simple and readable.

### Key Concepts
* **Control Node:** Machine where Ansible is installed.
* **Managed Nodes:** Target systems (Docker containers).
* **Inventory:** A list or group of managed nodes.
* **Playbook:** A YAML file defining the desired state or tasks.
* **Modules:** Built-in tools like apt, copy, and service.

### Why Ansible?
* Reduces manual work
* Ensures consistency
* Saves time
* Scales easily

---

## Tools Used
* **Ansible**
* **Docker**
* **Ubuntu (WSL/Linux)**

---

## Procedure

### Step 1: Install Ansible
```bash
sudo apt update -y
sudo apt install ansible -y
ansible --version
ansible localhost -m ping
