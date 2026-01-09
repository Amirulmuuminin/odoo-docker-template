# Odoo Docker Template

Docker Compose template for running Odoo with PostgreSQL.

## Requirements

- Docker
- Docker Compose

## Directory Structure

```
odoo-from-template/
├── docker-compose.yaml    # Docker Compose configuration
├── .env                   # Environment variables
├── .gitignore            # Git ignored files
├── config/               # Odoo configuration
├── addons/               # Custom Odoo addons
└── README.md             # This documentation
```

## Usage

### 1. Clone or Download

```bash
git clone https://github.com/Amirulmuuminin/odoo-docker-template.git
```

### 2. Configure Environment

Edit the `.env` file according to your needs:

```env
# Odoo Configuration
ODOO_VERSION=19.0
ODOO_PORT=8069

# PostgreSQL Configuration
POSTGRES_VERSION=16
POSTGRES_DB=postgres
POSTGRES_USER=odoo
POSTGRES_PASSWORD=odoo
```

### 3. Start Containers

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Stop and remove volumes (delete data)
docker-compose down -v
```

### 4. Access Odoo

Open your browser and visit:

```
http://localhost:8069
```

## Container Management

### View Running Containers

```bash
docker-compose ps
```

### View Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f web
docker-compose logs -f db
```

### Access Container Shell

```bash
# Access Odoo container
docker-compose exec web bash

# Access PostgreSQL container
docker-compose exec db psql -U odoo -d postgres
```

### Restart Containers

```bash
docker-compose restart
```

## Custom Addons

Place your custom Odoo addons in the `addons/` directory:

```
addons/
├── module1/
├── module2/
└── module3/
```

## Odoo Configuration

Place your Odoo configuration file in the `config/` directory:

```
config/
└── odoo.conf
```

Example `odoo.conf`:

```ini
[options]
admin_passwd = admin
db_host = db
db_port = 5432
db_user = odoo
db_password = odoo
dbfilter = .*
addons_path = /mnt/extra-addons
```

## Backup & Restore

### Backup Database

```bash
docker-compose exec db pg_dump -U odoo postgres > backup.sql
```

### Restore Database

```bash
docker-compose exec -T db psql -U odoo postgres < backup.sql
```

### Backup File Data

```bash
docker run --rm -v odoo-web-data:/data -v $(pwd):/backup alpine tar czf /backup/odoo-data-backup.tar.gz -C /data .
```

## Troubleshooting

### Container Won't Start

```bash
# Check logs
docker-compose logs

# Check port usage
sudo lsof -i :8069
```

### Reset All Data

```bash
docker-compose down -v
docker-compose up -d
```

### Delete Volumes

To remove all Docker volumes associated with this project:

```bash
# Stop and remove containers with their volumes
docker-compose down -v

# List all volumes to verify removal
docker volume ls

# Remove specific volume manually (if needed)
docker volume rm odoo-from-template_db-data odoo-from-template_web-data
```

**Warning**: Deleting volumes will permanently remove all data including databases, filestore, and configurations. Make sure to backup before removing volumes.

### Change PostgreSQL Password

1. Edit the `.env` file
2. Run `docker-compose down` and `docker-compose up -d`

## License

This template is free to use and modify as needed.
