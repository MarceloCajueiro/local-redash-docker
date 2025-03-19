# Redash Local with Docker

This repository contains the configuration to run Redash locally using Docker, with dedicated PostgreSQL and Redis.

## Prerequisites

- Docker and Docker Compose installed
- Mac with M1/M2 chip (specific configuration for ARM64)

## Redash Installation

1. Clone this repository
2. Create the necessary directories:
   ```bash
   mkdir -p data redis-data postgres-data
   ```

3. Start the containers:
   ```bash
   docker-compose up -d
   ```

4. Create the database:
   ```bash
   docker-compose run --rm server create_db
   ```

5. Run the database migrations:
   ```bash
   docker-compose run --rm server manage db upgrade
   ```

6. Create an admin user:
   ```bash
   docker-compose run --rm server manage users create --admin --password admin admin@example.com Admin
   ```

## Access

- URL: http://localhost:5000
- Email: admin@example.com
- Password: admin

## Important Configurations

- Redash is configured to run in AMD64 mode (necessary for Mac M1/M2)
- Available services:
  - Server: Redash web interface (port 5000)
  - Scheduler: Manages query schedules
  - Worker: Processes queries in the background
  - Redis: Cache and queues (port 6379)
  - PostgreSQL: Database (port 5433)

## Stopping the Services

To stop the services:

```bash
docker-compose down
```

## Volumes

- `./data`: Stores persistent Redash data
- `./redis-data`: Stores Redis data
- `./postgres-data`: Stores PostgreSQL data