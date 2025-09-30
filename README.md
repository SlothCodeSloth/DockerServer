# Personal Home Server

A comprehensive Docker-based home server solution for media management, personal website hosting, and game server hosting. Built for easy deployment, scalability, and maintenance with full containerization.

## 🎯 Overview

This home server provides an all-in-one solution for:
- **Media Management**: Automated media downloading with Sonarr, Radarr, and qBittorrent
- **Personal Website**: Full stack blog and portfolio with React frontend and Spring Boot backend
- **Game Hosting**: Minecraft Fabric server with mod support
- **VPN Security**: All torrent traffic routed through ProtonVPN via Gluetun
- **Centralized Dashboard**: Homarr for unified service management
- **Localized Management**: Portainerr for local management of containers
- **SSL/TLS**: Automated certificate management with Certbot

---

## 🏗️ Architecture

### Service Stack

| Service | Purpose | Technology |
|---------|---------|------------|
| **Gluetun** | VPN tunnel for secure torrenting | ProtonVPN WireGuard |
| **qBittorrent** | Torrent client (via VPN) | LinuxServer.io |
| **Sonarr** | TV show automation | LinuxServer.io |
| **Radarr** | Movie automation | LinuxServer.io |
| **Prowlarr** | Indexer manager | LinuxServer.io |
| **Overseerr** | Media request management | LinuxServer.io |
| **Homarr** | Service dashboard | Homarr |
| **Portainer** | Docker container management | Portainer CE |
| **Nginx** | Reverse proxy & web server | Nginx |
| **Certbot** | SSL certificate automation | Let's Encrypt |
| **PostgreSQL** | Blog database | Postgres 15 |
| **Blog Backend** | REST API server | Spring Boot (Java 17) |
| **Blog Frontend** | React website (Served via Nginx) | React + TypeScript + Vite |
| **PCPartPicker API** | Custom Backend for API | [PyPartPicker API](https://github.com/thefakequake/pypartpicker) |
| **Minecraft Server** | Game server | itzg/minecraft-server (Fabric) |

---

## ✨ Features

### 🎬 Media Management
- **Automated Downloads**: Sonarr and Radarr automatically search and download content
- **Indexer Management**: Prowlarr centralizes torrent indexer configuration
- **Request System**: Overseerr allows users to request movies and TV shows
- **VPN Protection**: All torrent traffic routed through ProtonVPN with port forwarding
- **Organized Storage**: Structured data directory for media files

### 🌐 Website Hosting
- **Personal Blog**: Feature rich Blog with text editing and image uploads
- **Project Portfolio**: Showcase professional projects with detailed descriptions
- **Secure Authentication**: JWT-based admin authentication for content management
- **SSL/TLS Encryption**: Automated certificate renewal via Let's Encrypt
- **Reverse Proxy**: Nginx handles routing and serves static frontend
- **Android App API**: [PyPartPicker API](https://github.com/SlothCodeSloth/PCPP_API) hosted for App usage

### 🎮 Game Hosting
- **Minecraft Server**: Fabric Modrinth modpack support
- **Auto-sync Mods**: Modrinth integration keeps mods synchronized
- **Resource Allocation**: Configurable memory (default 6GB)
- **Persistent Storage**: World data preserved in Docker volumes

### 🔒 Security & Privacy
- **VPN Tunnel**: Gluetun routes torrent traffic through ProtonVPN WireGuard
- **Network Isolation**: qBittorrent shares network stack with VPN container
- **JWT Authentication**: Secure token-based auth for blog administration
- **SSL/TLS**: HTTPS encryption for all web services
- **LAN-Only Ports**: Torrent WebUI restricted to local network for simple security

### 🛠️ Management & Monitoring
- **Portainer**: Web based Docker container management
- **Homarr Dashboard**: Unified interface for all services
- **Health Checks**: Automated monitoring for blog backend
- **Automatic Restarts**: Services configured to restart on failure unless manually stopped

---

## 📂 Project Structure

```
home-server/
├── docker-compose.yml                # Main configuration/start file
├── .env                              # Environment variables (sensitive data)
├── nginx.conf                        # Nginx reverse proxy configuration
├── appdata/
│   ├── certbot/
│   │   ├── conf/                     # SSL certificates
│   │   └── www/                      # ACME challenge files
│   ├── blog_backend/
│   │   └── blog-app.jar              # Spring Boot JAR file
│   ├── blog_frontend/
│   │   └── dist/                     # Built React application
│   ├── postgres_blog/                # PostgreSQL data
│   ├── qbittorrent/                  # qBittorrent config
│   ├── sonarr/                       # Sonarr config
│   ├── radarr/                       # Radarr config
│   ├── prowlarr/                     # Prowlarr config
│   ├── overseerr/                    # Overseerr config
│   ├── homarr/                       # Homarr config
│   ├── portainer/                    # Portainer data
│   └── minecraft/                    # Minecraft world data
└── PCPartPicker/                     # PCPartPicker API source code
│   ├── Backend.py                    # Homarr config
│   ├── Dockerfile                    # Portainer data
│   ├── requirements.txt              # Portainer data
└── └── .dockerignore                 # Minecraft world data
```

---

## 🚀 Getting Started

### Prerequisites

**Hardware Requirements**
- CPU: Quad-core processor (recommended)
- RAM: 16GB minimum
- Storage: 256GB+ Storage (Preferrebly SSD)
- Recommended: 1TB+ Storage (Separate Drive, Preferrebly HDD)
- Network: Stable internet connection with port forwarding capability

**Software Requirements**
- Docker Engine 20.10+
- Docker Compose 2.0+
- Operating System (Windows/Linux, Preferrerbly Server)

### Installation

#### 1. [Install Docker](https://docs.docker.com/compose/install/)

```bash
# Update package manager
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo apt install docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER
newgrp docker

# Verify installation
docker --version
docker compose version
```

#### 2. Clone Repository

```bash
git clone https://github.com/your-username/home-server.git
cd home-server
```

#### 3. Configure Environment Variables

Create a `.env` file in the root directory:

```env
# Network Configuration
LOCAL_IP= # Your server's local IP

# VPN Configuration (Chosen VPN)
WIREGUARD_PRIVATE_KEY=your_vpn_private_key

# Service Ports
// Enter port or go with Default for each
QBIT_PORT=
HOMARR_PORT=
OVERSEERR_PORT=
SONARR_PORT=
RADARR_PORT=
PROWLARR_PORT=
PORTAINER_PORT=
API_PORT=
MC_PORT=
NGINX_PORT_A=
NGINX_PORT_B=
SERVER_PORT=

# qBittorrent Credentials
QBIT_USERNAME=admin
QBIT_PASSWORD=your_secure_password

# Database Configuration
SPRING_DATASOURCE_URL=jdbc:postgresql://blog-db:5432/blogdb
SPRING_DATASOURCE_USERNAME=bloguser
SPRING_DATASOURCE_PASSWORD=your_db_password

# Blog Backend Security
JWT_SECRET=your_jwt_secret_key_here
ADMIN_USER=admin
ADMIN_PASS=your_admin_password
```

**Security Note**: Never commit your `.env` file to version control. Add it to `.gitignore`.

#### 4. Prepare Application Files

**Blog Backend:**
```bash
# Build your Spring Boot application
cd /path/to/blog-backend
./mvnw clean package
cp target/blog-app.jar /path/to/home-server/appdata/blog_backend/
```

**Blog Frontend:**
```bash
# Build React application
cd /path/to/blog-frontend
npm run build
cp -r dist/* /path/to/home-server/appdata/blog_frontend/dist/
```

**Minecraft Server:**
```bash
# Place your Modrinth modpack file
cp server.mrpack /path/to/home-server/appdata/minecraft/
```

#### 5. Configure Nginx

Create or update `nginx.conf` with your domain:

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    # Certbot ACME challenge
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    
    # Redirect to HTTPS
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;
    server_name your-domain.com;
    
    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;
    
    # Blog frontend
    location / {
        root /var/www/blog;
        try_files $uri $uri/ /index.html;
    }
    
    # Blog backend API
    location /api {
        proxy_pass http://blog-backend:8090;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    # PCPartPicker API
    location /pcpp-api {
        proxy_pass http://pcpartpicker-api:5000;
        proxy_set_header Host $host;
    }
}
```

#### 6. Obtain SSL Certificates

```bash
# Start nginx and certbot services
docker compose up -d nginx certbot

# Request certificate
docker compose run --rm certbot certonly --webroot \
  --webroot-path=/var/www/certbot \
  --email your-email@example.com \
  --agree-tos \
  --no-eff-email \
  -d your-domain.com

# Restart nginx to load certificates
docker compose restart nginx
```

#### 7. Launch All Services

```bash
# Start all containers
docker compose up -d

# View logs
docker compose logs -f

# Check service status
docker compose ps
```

---

## 🔧 Configuration

### Initial Setup for Media Services

#### 1. Prowlarr (Indexer Manager)
1. Access Prowlarr at `http://your-server-ip:PROWLARR_PORT`
2. Add indexers (torrent sites)
3. Configure API keys for Sonarr and Radarr integration

#### 2. Sonarr (TV Shows)
1. Access Sonarr at `http://your-server-ip:SONARR_PORT`
2. Settings → Download Clients → Add qBittorrent
   - Host: `gluetun` (since qBittorrent uses gluetun's network)
   - Port: `QBIT_PORT`
   - Username/Password: From your `.env`
3. Settings → Media Management → Configure root folder: `/data/torrents/tv`

#### 3. Radarr (Movies)
1. Access Radarr at `http://your-server-ip:RADARR_PORT`
2. Configure similarly to Sonarr
3. Set root folder: `/data/torrents/movies`

#### 4. Overseerr (Request Management)
1. Access Overseerr at `http://your-server-ip:OVERSEERR_PORT`
2. Connect to Sonarr and Radarr using their API keys
3. Configure user permissions and notification settings

#### 5. qBittorrent
1. Access via `http://LOCAL_IP:QBIT_PORT`
2. Login with credentials from `.env`
3. Settings → Downloads → Default Save Path: `/data/torrents`
4. Verify VPN connection in bottom right (should show VPN IP)

### VPN Configuration

The Gluetun container routes all qBittorrent traffic through VPN:
- **Provider**: Preferred VPN
- **Protocol**: WireGuard
- **Features**: Port forwarding can be enabled for better peer connectivity
- **Network Mode**: qBittorrent shares Gluetun's network stack

**To obtain WireGuard key from VPN:**
1. Access VPN account
2. Navigate to WireGuard configuration
3. Copy private key to `.env` file

---

## 📊 Suggested Service Access

### Local Network Access
- **Homarr Dashboard**: `http://your-server-ip:HOMARR_PORT`
- **qBittorrent**: `http://your-server-ip:QBIT_PORT`
- **Sonarr**: `http://your-server-ip:SONARR_PORT`
- **Radarr**: `http://your-server-ip:RADARR_PORT`
- **Prowlarr**: `http://your-server-ip:PROWLARR_PORT`
- **Portainer**: `http://your-server-ip:PORTAINER_PORT`

### Public Access (via domain)
- **Overseerr**: `https://your-domain.com/OVERSEERR_PORT`
- **Personal Website**: `https://your-domain.com/`
- **Blog API**: `https://your-domain.com/api`
- **Minecraft Server**: `your-domain.com:MC_PORT`

---

## 🛡️ Security Best Practices

### Network Security
- **VPN for Torrenting**: All torrent traffic encrypted through VPN
- **Firewall Configuration**: Only expose necessary ports
- **LAN-Only Services**: Sensitive services (qBittorrent, portainer) should be restricted to local network
- **Reverse Proxy**: Nginx acts as single entry point for web services

### Application Security
- **Strong Passwords**: Use unique, complex passwords for all services
- **JWT Tokens**: Blog uses token-based authentication with configurable expiration
- **Database Isolation**: PostgreSQL not exposed externally
- **SSL/TLS**: All public web traffic encrypted

### Maintenance
- **Regular Updates**: Keep Docker images updated
- **Backup Strategy**: Regular backups of appdata/ directory
- **Log Monitoring**: Review service logs for anomalies
- **Certificate Renewal**: Certbot auto-renews, but monitor expiration

---

## 🐛 Troubleshooting

### VPN/Torrent Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| qBittorrent won't start | VPN not connected | Check Gluetun logs: `docker logs gluetun` |
| Can't access qBittorrent | Wrong network mode | Verify `network_mode: "service:gluetun"` |
| Slow downloads | MTU mismatch | Adjust `WIREGUARD_MTU` in docker-compose |
| No peers connecting | Port forwarding off | Ensure `PORT_FORWARDING=on` in Gluetun |

### Blog/Website Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| 502 Bad Gateway | Backend not running | Check: `docker logs blog-backend` |
| Database connection error | Wrong credentials | Verify `.env` database settings |
| SSL certificate error | Certificate not obtained | Rerun Certbot, check nginx logs |
| API CORS errors | Nginx misconfiguration | Update proxy headers in nginx.conf |

### General Docker Issues

```bash
# View all container logs
docker compose logs -f

# Restart a specific container
docker compose restart [service_name]

# Rebuild a specific container
docker compose up -d --build [service_name]

# Check container status
docker compose ps

# View resource usage
docker stats
```

---

## 📈 Performance Optimization

### Resource Allocation
- **Minecraft Server**: Adjust `MEMORY` in docker-compose.yml based on player count
- **Database**: Consider increasing PostgreSQL shared_buffers for better performance
- **Media Services**: Limit concurrent downloads in qBittorrent

### Storage Management
- **Clean Old Torrents**: Configure qBittorrent to remove completed torrents after seeding
- **Log Rotation**: Implement log rotation to prevent disk space issues

---

## 🔄 Backup Strategy

### Critical Data to Backup
```bash
# Backup script example
#!/bin/bash
BACKUP_DIR="/path/to/backups/$(date +%Y%m%d)"
mkdir -p "$BACKUP_DIR"

# Backup configurations
tar -czf "$BACKUP_DIR/appdata.tar.gz" appdata/

# Backup database
docker exec blog-db pg_dump -U bloguser blogdb > "$BACKUP_DIR/blogdb.sql"

# Backup docker-compose and nginx config
cp docker-compose.yml nginx.conf .env "$BACKUP_DIR/"
```

**Recommended to Backup Frequency:**

---

## 🚀 Future Enhancements

- [ ] Add Nginx configuration for more global ports
- [ ] Add monitoring stack (Prometheus + Grafana)
- [ ] Set up Cloudflare tunnel for enhanced security
- [ ] Implement automated health checks and notifications

---

## 🔗 Related Repositories

- **Blog Frontend**: [Personal Website Frontend](https://github.com/SlothCodeSloth/BlogFrontend)
- **Blog Backend**: [Personal Website Backend](https://github.com/SlothCodeSloth/BlogBackend)
- **PyPartPicker**: [PyPartPicker API]([https://github.com/SlothCodeSloth/PCPP_API](https://github.com/thefakequake/pypartpicker))
- **PCPartPicker API Backend**: [PCPP API](https://github.com/SlothCodeSloth/PCPP_API)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **LinuxServer.io**: Excellent Docker images for media services
- **ProtonVPN**: Secure VPN service
- **itzg**: Minecraft server Docker image
- **Gluetun**: VPN client container
- All open-source projects that make this stack possible
