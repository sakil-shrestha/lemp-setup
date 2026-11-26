# Ansible WordPress Deployment Using Nginx, MySQL, PHP & Ansible Roles

## 📌 Project Overview

This project demonstrates the automated deployment of a WordPress application using **Ansible** on an Ubuntu server.

The deployment uses Ansible Roles to organize infrastructure configuration into reusable and maintainable components. Each service has its own role, making the automation modular and easier to manage.

The project automates the installation and configuration of:

- **Nginx** – Web server
- **MySQL** – Database server
- **PHP** – PHP runtime and required extensions
- **WordPress** – Content Management System (CMS)

MySQL database operations are also performed using SQL queries through Ansible tasks.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Ansible | Configuration management and automation |
| Ubuntu | Target operating system |
| Nginx | Web server |
| MySQL | Database management |
| PHP | WordPress runtime |
| WordPress | Content Management System |
| YAML | Ansible playbooks and configuration |

## 📂 Project Structure

```text
ansible-wordpress/
│
├── inventory/
│   └── hosts.ini
│
├── group_vars/
│   └── wordpress.yml
│
├── roles/
│   ├── nginx/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   └── default.conf.j2
│   │   └── defaults/
│   │       └── main.yml
│   │
│   ├── mysql/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── defaults/
│   │       └── main.yml
│   │
│   ├── php/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── defaults/
│   │       └── main.yml
│   │
│   └── wordpress/
│       ├── tasks/
│       │   └── main.yml
│       ├── templates/
│       │   └── wp-config.php.j2
│       └── defaults/
│           └── main.yml
│
├── playbook.yml
└── README.md
```

## ⚙️ Prerequisites

Before running this project, make sure you have:

1. Ansible installed on the control node.
2. An Ubuntu target server.
3. SSH connectivity between the control node and target server.
4. A user with sudo privileges on the target server.
5. Python installed on the target server.

### Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
```

Verify the installation:

```bash
ansible --version
```

## 🔧 Configuration

### 1. Inventory Configuration

Define your target server in `inventory/hosts.ini`.

```ini
[wordpress]
devops-server ansible_host=YOUR_SERVER_IP ansible_user=YOUR_USERNAME
```

Replace the IP address and username with your target server details.

### 2. Group Variables

Configure shared variables in `group_vars/wordpress.yml`.

Example:

```yaml
---
# MySQL Configuration
mysql_root_password: "root_password"
mysql_database: "wordpress_db"
mysql_user: "wordpress_user"
mysql_password: "wordpress_password"

# WordPress Configuration
wordpress_path: "/var/www/html/wordpress"
wordpress_url: "https://wordpress.org/latest.tar.gz"

# PHP Configuration
php_version: "8.3"

# Nginx Configuration


These variables are shared across tasks executed against hosts in the `wordpress` inventory group.

**Note:** Use Ansible Vault to encrypt sensitive credentials before using this project in production.

### 3. Ansible Playbook

The main `playbook.yml` orchestrates the execution of individual roles.

```yaml
---
- name: Deploy WordPress using Ansible Roles
  hosts: wordpress
  become: yes

  roles:
    - nginx
    - mysql
    - php
    - wordpress
```

Each role is responsible for configuring its respective service.

## 🚀 Deployment

Follow these steps to deploy WordPress.

### Step 1: Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
cd ansible-wordpress
```

### Step 2: Verify SSH Connectivity

```bash
ansible wordpress -i inventory/hosts.ini -m ping
```

### Step 3: Check the Playbook Syntax

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml --syntax-check
```

### Step 4: Execute the Playbook

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

Ansible will execute the roles in the following order:

1. Nginx installation and configuration.
2. MySQL installation and configuration.
3. PHP installation and configuration.
4. WordPress download and configuration.

### Step 5: Verify the Deployment

Check the services on the target server:

```bash
sudo systemctl status nginx
sudo systemctl status mysql
sudo systemctl status php8.3-fpm
```

Check the WordPress directory:

```bash
ls -la /var/www/html/wordpress
```

Finally, open the target server's IP address in your browser:

```text
http://YOUR_SERVER_IP
```

Complete the WordPress installation through the browser.

## 📚 Key Ansible Concepts Demonstrated

This project provides practical experience with the following Ansible concepts:

- **Inventory:** Managing target hosts.
- **Playbooks:** Defining automation workflows.
- **Roles:** Organizing automation into reusable components.
- **Tasks:** Executing individual configuration operations.
- **Modules:** Using Ansible modules to manage packages, services, files, and databases.
- **Variables:** Managing configuration through role defaults and group variables.
- **Templates:** Generating configuration files dynamically using Jinja2.
- **Handlers:** Restarting or reloading services when configuration changes.
- **Privilege Escalation:** Using `become: yes` to execute privileged tasks.
- **SQL Queries:** Executing database operations through Ansible.

## 🎯 Project Objectives

- Automate WordPress deployment using Ansible.
- Understand role-based infrastructure automation.
- Separate application and service configurations.
- Learn variable management using `group_vars` and role defaults.
- Practice Nginx, MySQL, and PHP configuration.
- Understand how configuration management works in DevOps environments.

## 🔐 Security Considerations

For production environments:

- Store passwords using Ansible Vault.
- Avoid committing sensitive credentials to GitHub.
- Use strong MySQL passwords.
- Configure firewall rules.
- Enable HTTPS using SSL certificates.
- Restrict SSH access to trusted users and networks.

## 👨‍💻 Author

**Sakil Shrestha**

Aspiring DevOps Engineer | Linux | Docker | Ansible | Nginx | MySQL

