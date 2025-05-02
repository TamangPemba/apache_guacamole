# Apache Guacamole Bastion Server (Docker Compose)

This repository provides a ready-to-deploy Docker Compose setup for running [Apache Guacamole](https://guacamole.apache.org/) as a **bastion host**. It includes support for:

- Remote desktop access via web browser
- LDAP authentication (Active Directory)
- Session recording
- Nginx reverse proxy for secure HTTPS access

## 🧱 Architecture

The stack includes:

- **Guacamole** (web interface)
- **guacd** (remote desktop protocol daemon)
- **PostgreSQL** (for Guacamole configuration storage)
- **Nginx** (reverse proxy for secure external access)

All services are connected over an internal Docker network (`guacnet`).

## 📁 Folder Structure

.
├── docker-compose.yml
├── guacamole_data/
├── guacd_data/
├── postgres_data/
├── recordings/
└── nginx/
├── nginx.conf
└── certs/
