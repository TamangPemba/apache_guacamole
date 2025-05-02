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

## 🚀 How to Deploy on Any Machine

###  1. Install Prerequisites

Ensure the machine has:

- Docker
- Docker Compose
- Internet access (to pull images)

### 2. Clone This Repository
```bash
   git clone https://github.com/TamangPemba/apache_guacamole.git
   cd apache_guacamole
```

### 3. Prepare Configuration (One-Time Setup)

- Ensure LDAP details in docker-compose.yml match your environment.
- Place your nginx.conf and SSL certs in the nginx/ folder, if using HTTPS.

### 4. Start the Guacamole Stack
```bash
docker-compose up -d 
```
This command will:

- Pull required images (guacamole/guacamole, guacamole/guacd, postgres, nginx)
- Create containers and volumes
- Start the services automatically

### 5. Access the Web Interface
https://your_server_ip  or https://your_domain_name



