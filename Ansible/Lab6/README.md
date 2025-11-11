# README - Ansible Lab (Ad-hoc & Playbook)

## Objectives
This document explains how to complete the lab for managing packages and services using Ansible. 
It covers both **Ad-hoc commands** and **Playbook automation**.

---

## Prerequisites
1. Control node with Ansible installed (v2.9+ or newer).
2. SSH access to target hosts (via SSH keys or passwords).
3. An `inventory` file defining your target hosts, or use the `-i` option.
4. Sudo privileges on target hosts (for package/service management).

---

## Playbook Overview
**File:** `playbook.yml` 
**Purpose:** Automate software package management, service control, and version checking.

### Variables
- `web_package`: the main package name (e.g., nginx)
- `tools`: a list of additional packages to install (git, curl, vim, nginx)

### Tasks
1. Remove the main web package (`package` module)
2. Install all packages listed in `tools` (using a loop)
3. Start and enable the main web service (`service` module)
4. Check the version of the web package (`command` module)
5. Display the stored command output (`debug` module)

### Provided Playbook
```yaml
---
- name: Package management playbook
  hosts: all
  become: true

  vars:
    web_package: nginx
    tools:
      - git
      - curl
      - vim
      - nginx

  tasks:

    - name: Remove the main web package
      ansible.builtin.package:
        name: "{{ web_package }}"
        state: absent

    - name: Install tools (loop over tools list)
      ansible.builtin.package:
        name: "{{ item }}"
        state: present
      loop: "{{ tools }}"

    - name: Start and enable the web service
      ansible.builtin.service:
        name: "{{ web_package }}"
        state: started
        enabled: true

    - name: Check web package version (register output)
      ansible.builtin.command: "{{ web_package }} -v"
      register: web_version
      ignore_errors: true
      changed_when: false

    - name: Show web package version
      ansible.builtin.debug:
        var: web_version
```

---

## Part 1 — Ad-hoc Commands

### 1. Check Connectivity
```bash
ansible all -i inventory -m ping
```

### 2. Install nginx on all hosts
```bash
ansible all -i inventory -m package -a "name=nginx state=present" --become
```

### 3. Verify installation
```bash
ansible all -i inventory -m command -a "nginx -v" --become
```

### 4. Remove nginx
```bash
ansible all -i inventory -m package -a "name=nginx state=absent" --become
```

---

## Example Inventory File
You can create an `inventory` file in the same folder as your playbook:
```
[web]
host1.example.com
host2.example.com

[all:vars]
ansible_user=your_user
ansible_ssh_private_key_file=~/.ssh/id_rsa
```
You can also use IP addresses instead of domain names.

---

## Optional `ansible.cfg` Setup
You can create an `ansible.cfg` file to simplify command usage:
```ini
[defaults]
inventory = ./inventory
host_key_checking = False
retry_files_enabled = False
```
This allows running commands without specifying `-i` each time.

---

## Running the Playbook
1. Make sure `inventory` and `ansible.cfg` are in the same folder.
2. Execute the playbook:
```bash
ansible-playbook playbook.yml
```
Or specify an inventory file manually:
```bash
ansible-playbook -i inventory playbook.yml
```

---

## Technical Notes
- `become: true` allows using sudo privileges.
- `register: web_version` stores command output for later use.
- `ignore_errors: true` prevents the playbook from stopping if a command fails.
- `changed_when: false` avoids marking info commands as changes.
- The `package` module is cross-platform (works with apt, yum, dnf, etc.).
- You can use an inventory file or define hosts directly in the command line.

---

## Summary
This README includes:
- Lab objectives
- Ad-hoc commands
- Playbook explanation
- Example inventory and ansible.cfg files
- Run instructions
- Technical explanations

---

