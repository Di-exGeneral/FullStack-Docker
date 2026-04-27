# FullStack Docker

A production-like multi-container web application using Django, PostgreSQL, and Nginx managed with Docker Compose.

## Architecture

```
Browser → Nginx (port 80) → Django (port 8000) → PostgreSQL (port 5432)
```

- **Nginx** acts as a reverse proxy, receiving all incoming requests and forwarding them to Django
- **Django** handles the application logic, only accessible through Nginx
- **PostgreSQL** stores the data, only accessible by Django

## Project Structure

```bash
FullStack-Docker/
  web/
    project/
      app/
      project/
        settings.py
        urls.py
        wsgi.py
      manage.py
      Dockerfile
      requirements.txt
  nginx/
    Dockerfile
    nginx.conf
  docker-compose.yml
  .env
  .env.example
  .gitignore
```

## Environment Variables

Copy `.env.example` to `.env` and fill in your values:

```bash
cp .env.example .env
```

Required variables:

```
SECRET_KEY=
DEBUG=
ALLOWED_HOSTS=
dbname=
dbuser=
dbpassword=
```

## Setup

**1. Clone the repo**
```bash
git clone git@github.com:Di-exGeneral/FullStack-Docker.git
cd FullStack-Docker
```

**2. Create your `.env` file**
```bash
cp .env.example .env
# edit .env with your values
```

**3. Build and start all containers**
```bash
docker-compose up -d
```

**4. Run migrations**
```bash
docker exec -it fullstack-docker-web-1 python manage.py migrate
```

**5. Access the app**
```
http://localhost:80
```

## Common Commands

```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# View logs for a specific service
docker logs fullstack-docker-web-1
docker logs fullstack-docker-db-1
docker logs fullstack-docker-server-1

# Rebuild after changes
docker-compose down
docker-compose build --no-cache
docker-compose up -d

# Access Django shell
docker exec -it fullstack-docker-web-1 python manage.py shell

# Run migrations
docker exec -it fullstack-docker-web-1 python manage.py migrate
```

## Key Concepts Used

- **Dockerfile** to define each service's environment
- **Docker Compose** to manage multi-container setup
- **Environment variables** via `.env` file for secrets and config
- **Nginx reverse proxy** to forward traffic to Django
- **Named network** connecting all services so they can communicate
- **PostgreSQL** as the production database instead of SQLite

## Gotchas

- `ALLOWED_HOSTS` must include `web` since Nginx proxies using the service name as the host
- Port mapping in compose: `"80:80"` for Nginx, no external port for Django or PostgreSQL
- `HOST: 'db'` in Django's `DATABASES` setting uses the Compose service name to reach PostgreSQL
- Always rebuild with `--no-cache` after changing `requirements.txt` or `Dockerfile`



readme from master
