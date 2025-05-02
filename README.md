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

## 📦 Deployment Guide

###  1. Install Prerequisites

Ensure the machine has:

- Docker
- Docker Compose
- Internet access (to pull images)

### 2. Clone This Repository
```bash
git clone https://github.com/TamangPemba/apache_guacamole.git
cd apache_guacamole/guacamole
```

### 3. Configure Environment 
- Edit docker-compose.yml with your LDAP server settings.
- Place SSL certificates in nginx/certs/.
- Verify or customize nginx.conf in the nginx/ directory.

### 4. Start the Guacamole Stack
```bash
docker-compose up -d 
```
This command will:

- Pull required images (guacamole/guacamole, guacamole/guacd, postgres, nginx)
- Create containers and volumes
- Start the services automatically
### Database Initialization (Required Once)
After starting the containers, you must initialize the PostgreSQL database schema for Guacamole:

```bash
# Generate the initdb.sql schema file
docker run --rm guacamole/guacamole /opt/guacamole/bin/initdb.sh --postgresql > initdb.sql

# Copy the SQL file into the database container
docker cp initdb.sql guac_db:/initdb.sql

# Enter the PostgreSQL container
docker exec -it guac_db bash

# Run the SQL initialization script
psql -U guac_user -d guacamole_db -f /initdb.sql

# Exit the container
exit
```
Note: You only need to run this once, after the initial startup.
Guacamole will then be ready to store connections, users, and history in the database.

### 5. Access the Web UI
Open your browser and go to:
- https://your_server_ip  
or
-  https://your_domain_name

Login Options
- LDAP credentials (Active Directory users)
- Fallback default user (if LDAP fails):

**Username**: `guacadmin`  
**Password**: `guacadmin`

⚠️ You should change or disable the guacadmin account after initial setup for security.

### LDAP Authentication
Ensure the following environment variables in docker-compose.yml match your directory:
```yaml
LDAP_HOSTNAME: "192.168.90.90"
LDAP_PORT: "389"
LDAP_SEARCH_BIND_DN: "pemba@dc.pemba"
LDAP_SEARCH_BIND_PASSWORD: "domanuserpassword"
LDAP_USER_BASE_DN: "DC=dc,DC=pemba"
```
### Session Recording

All RDP sessions are automatically recorded to the ./recordings/ folder on the host.
If you encounter permission issues, run the following commands:
```bash
sudo chown -R 1000:1001 recordings/
sudo chmod 2775 recordings
```
This sets the correct ownership and permissions for Guacamole to write recordings.

### 6. Stopping the Stack
```bash
docker compose down
```
Persistent data remains in the local volume directories.



