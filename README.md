<div align="center">

# ansible-nginx-role

Professional Ansible role to install and configure **NGINX** as a lightweight reverse proxy.

[![Ansible](https://img.shields.io/badge/Ansible-Automation-black?logo=ansible)](https://www.ansible.com/)
[![NGINX](https://img.shields.io/badge/NGINX-Reverse%20Proxy-009639?logo=nginx&logoColor=white)](https://nginx.org/)
[![Status](https://img.shields.io/badge/Status-Stable-blue)](#)

</div>

---

## ▢ Overview

▢ Purpose:  Install and configure NGINX as a simple reverse proxy
▢ Output:   /etc/nginx/conf.d/reverse-proxy.conf
▢ Behavior: Reload NGINX automatically when configuration changes
▢ Repository layout
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
▢ Requirements
text
▢ Ansible: Installed on the control machine
▢ Access:  SSH connectivity to the target host(s)
▢ Privs:   sudo/become privileges on the target host(s)

▢ Configuration
Variables
Variable	Default	Description
nginx_revproxy_listen_port	80	Port NGINX will listen on
nginx_revproxy_upstream	http://127.0.0.1:8080	Upstream backend to proxy to
▢ Quick start
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

▢ Example scenario

text
▢ App listens on:  127.0.0.1:8080
▢ NGINX exposes:   http://<server-ip>:80
▢ Proxy behavior:  /  ->  http://127.0.0.1:8080

▢ Notes & hardening ideas

    For production, consider TLS (Let’s Encrypt), rate limiting, and separate server blocks per vhost.

    If you extend the role, consider validating before reload (e.g., nginx -t).
