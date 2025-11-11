# README - Ansible Role for Apache Web Server Installation

## Overview
This project demonstrates how to create and use an **Ansible role** to deploy and configure an Apache Web Server. 
It includes task organization, service handlers, and a simple HTML template to verify deployment.

---

## Objectives
- Create and manage Ansible roles
- Define and organize tasks within roles
- Use handlers for service management
- Create and apply Jinja2 templates
- Deploy Apache Web Server using a playbook
- Verify the server configuration

---

## Project Structure
```
Lab7/
├── inventory
├── site.yml
└── roles/
    └── apache/
        ├── tasks/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── templates/
        │   └── index.html.j2
        ├── defaults/
        │   └── main.yml
        ├── meta/
        │   └── main.yml
        └── README.md
```

---

## Step 1 — Role Creation
The role was created using the command:
```bash
ansible-galaxy init roles/apache
```

This command generates the directory structure required for organizing role components.

---

## Step 2 — Main Tasks
The tasks are defined in `roles/apache/tasks/main.yml`. 
They install Apache, copy the index template, and notify the handler if any changes occur.

### File: `roles/apache/tasks/main.yml`
```yaml
---


- name: Install Apache (httpd) package
  ansible.builtin.yum:
    name: httpd
    state: present

- name: Copy custom index.html template
  ansible.builtin.template:
    src: index.html.j2
    dest: /var/www/html/index.html
    owner: root
    group: root
    mode: '0644'
  notify: Restart Apache

- name: Enable and start Apache service
  ansible.builtin.service:
    name: httpd
    state: started
    enabled: true
```

---

## Step 3 — Handler Definition
Handlers are used to restart or reload services when configuration changes occur.

### File: `roles/apache/handlers/main.yml`
```yaml
---
- name: Restart Apache
  ansible.builtin.service:
    name: httpd
    state: restarted
```

---

## Step 4 — Template File
A Jinja2 template is used to generate a custom welcome page for each host.

### File: `roles/apache/templates/index.html.j2`
```html
<html>
<head>
<title>Welcome Page</title>
</head>
<body>
<h1>Welcome to {{ ansible_hostname }} web server!</h1>
<p>Deployed automatically using Ansible Role: Apache</p>
</body>
</html>
```

---

## Step 5 — Main Playbook
The playbook `site.yml` is used to execute the Apache role on the target hosts.

### File: `site.yml`
```yaml
---
- name: Deploy Apache Web Server on webservers
  hosts: webservers
  become: true
  gather_facts: true
  roles:
    - roles/apache
```

---

## Step 6 — Inventory Example
The inventory defines the target hosts for deployment.

### File: `inventory`
```
[webservers]
worker1 ansible_host=192.168.124.249 ansible_user=root ansible_password=ahmad123 ansible_python_interpreter=/usr/bin/python3
```

---

## Running the Playbook
1. Navigate to the main project directory:
```bash
cd ~/Desktop/ansible/Lab7
```
2. Run the playbook with privilege escalation:
```bash
ansible-playbook -i inventory site.yml --become
```
3. Once completed, open a browser and access the server's IP to verify the Apache welcome page.

---

## Technical Notes
- The role installs and manages Apache (httpd on RHEL/CentOS or apache2 on Debian/Ubuntu).
- Handlers are triggered automatically when templates or configurations change.
- Templates use the `{ ansible_hostname }` variable to display the current host name.
- `become: true` is used to execute tasks with elevated privileges.
- The role can be easily reused or extended to manage additional Apache configurations.

---

## Summary
This lab demonstrates:
- How to structure and use Ansible roles
- How to deploy Apache Web Server automatically
- How to use handlers and templates effectively
- How to manage configurations in a modular, reusable way

---

