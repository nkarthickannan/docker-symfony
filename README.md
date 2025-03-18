# Dockerized Symfony development environment

[![Docker](https://img.shields.io/badge/Docker-27%2B-blue)](https://www.docker.com/)
[![Symfony](https://img.shields.io/badge/Symfony-7.x-%23000000)](https://symfony.com/)
[![PHP](https://img.shields.io/badge/PHP-8.3-%23777BB4)](https://www.php.net/)

A complete Docker development stack for Symfony applications with pre-configured services and debugging support.

## Features

- 🐋 Full Docker integration
- 🚀 Symfony 7.x optimized environment
- 🔧 Pre-configured services:
  - Nginx web server
  - PHP-FPM 8.3 with Xdebug
  - PostgreSQL database
  - RabbitMQ message broker
  - MailHog email testing
  - pgAdmin database management
- 🔒 Automatic SSL with mkcert
- 🐛 PHPStorm/Xdebug integration
- 📦 Environment variable configuration
- ♻️ Healthchecks

## Pre-requisites

- [Docker Desktop 4.37+](https://www.docker.com/products/docker-desktop/) (Docker 27+ & Docker Compose 2.31+)
- Git
- [PHPStorm](https://www.jetbrains.com/phpstorm/) (for debugging)
- [mkcert](https://github.com/FiloSottile/mkcert) (for SSL certificates)

## Getting started

### 1. Clone repository
```bash
git clone https://github.com/nkarthickannan/docker-symfony.git
cd docker-symfony
```

### 2. Configure environment
```bash
cp .env.sample .env
```
Edit `.env` file with your preferred credentials.

### 3. Build & start containers
```bash
docker-compose up -d --build
```

### 4. Initialize Symfony project
```bash
docker-compose exec phpfpm composer create-project symfony/skeleton:"7.4.*" .
```
Or clone the existing GIT repo
```bash
git clone <GIT_CLONE_URL> app
```

## Configuration

### Environment variables
| Variable | Description | Default |
|----------|--------------|---------|
| `DOMAIN` | Base domain name | `app.local` |
| `POSTGRES_*` | PostgreSQL credentials | - |
| `RABBITMQ_*` | RabbitMQ credentials | - |
| `PGADMIN_*` | pgAdmin credentials | - |

### Services overview
Replace `app.local` with your domain name mentioned in `.env` file
| Service | Port | Default URL | Credentials |
|---------|------|-----|-------------|
| Nginx | 80/443 | https://app.local | - |
| PHP-FPM | 9000 | - | - |
| PostgreSQL | 5432 | - | `.env` values |
| pgAdmin | 8080 | https://pgadmin.app.local | `.env` values |
| RabbitMQ | 15672 | https://rabbitmq.app.local | `.env` values |
| MailHog | 8025 | https://mailhog.app.local | - |

### SSL setup
Certificates are automatically generated using mkcert. To trust certificates:
```bash
mkcert -install
```

## Debugging with PHPStorm
See detailed [PHPStorm Configuration Guide](docs/PHPSTORM-XDEBUG.md)

## Common commands

### Start/Stop Containers
```bash
docker-compose start
docker-compose stop
```

### View logs
```bash
docker-compose logs -f [service_name]
```

### Run Composer
```bash
bin/composer [command]
```

### Access database
```bash
docker-compose exec db psql -U $POSTGRES_USER $POSTGRES_DB
```

## Troubleshooting

### Port conflicts
```bash
sudo lsof -i :80 # Find process using port
```

### Xdebug not connecting
1. Check PHPStorm listener status
2. Validate path mappings in PHPStorm

### SSL certificate errors
```bash
docker-compose exec nginx cat /etc/nginx/ssl/rootCA.pem > local-root-ca.pem
# Install certificate for your OS
```

### Services not starting
Check health status:
```bash
docker-compose ps
```

## License
Distributed under the MIT License. See `LICENSE` for more information.
