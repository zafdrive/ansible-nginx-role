<div align="center">

# ansible-nginx-role

Professional Ansible role to install and configure **NGINX** as a lightweight reverse proxy.

[![Ansible](https://img.shields.io/badge/Ansible-Automation-black?logo=ansible)](https://www.ansible.com/)
[![NGINX](https://img.shields.io/badge/NGINX-Reverse%20Proxy-009639?logo=nginx&logoColor=white)](https://nginx.org/)
[![Status](https://img.shields.io/badge/Status-Stable-blue)](#)

</div>

---

## Overview

▢ Purpose:  Install and configure NGINX as a simple reverse proxy
▢ Output:   /etc/nginx/conf.d/reverse-proxy.conf
▢ Behavior: Reload NGINX automatically when configuration changes

    [!NOTE]
    This role ships a minimal HTTP reverse proxy configuration. Extend it if you need TLS, multiple vhosts, or extra hardening. [web:64]

Repository layout

text
.
├─ inventory.ini
├─ playbook.yml
└─ roles/
   └─ ansible-nginx-role/
      ├─ defaults/
      │  └─ main.yml
      ├─ handlers/
      │  └─ main.yml
      ├─ tasks/
      │  └─ main.yml
      └─ templates/
         └─ nginx-revproxy.conf.j2

Features

    Installs NGINX using the OS package manager.

    Deploys a reverse proxy server block using a Jinja2 template.

    Ensures NGINX is enabled and running.

    Reloads NGINX on configuration changes (via handler).

Requirements

text
▢ Ansible: Installed on the control machine
▢ Access:  SSH connectivity to the target host(s)
▢ Privs:   sudo/become privileges on the target host(s)

Configuration
Variables
Variable	Default	Description
nginx_revproxy_listen_port	80	Port NGINX will listen on
nginx_revproxy_upstream	http://127.0.0.1:8080	Upstream backend to proxy to

    [!TIP]
    Keep nginx_revproxy_upstream on 127.0.0.1 if your app runs on the same server (common for single-node setups). [web:64]

Quick start
1) Inventory

Update inventory.ini with your host and SSH user:

text
[proxy]
your_server ansible_host=YOUR_HOST_OR_IP ansible_user=ubuntu

2) Playbook

Example playbook.yml usage:

text
- name: Install NGINX reverse proxy
  hosts: proxy
  become: true
  roles:
    - role: ansible-nginx-role
      vars:
        nginx_revproxy_listen_port: 80
        nginx_revproxy_upstream: "http://127.0.0.1:8080"

3) Run

bash
ansible-playbook -i inventory.ini playbook.yml

Example scenario

text
▢ App listens on:  127.0.0.1:8080
▢ NGINX exposes:   http://<server-ip>:80
▢ Proxy behavior:  /  ->  http://127.0.0.1:8080

    [!IMPORTANT]
    This role requires privilege escalation (become: true) to write under /etc/nginx and manage the nginx service. [web:64]

Notes & hardening ideas

    Add TLS (Let’s Encrypt), rate limiting, and separate server blocks per vhost for production.

    If you extend the role, consider validating before reload (e.g., nginx -t).

    [!WARNING]
    Always test configuration changes on a non-production host first to avoid locking yourself out or breaking routing. [web:64]

