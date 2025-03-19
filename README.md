# Redash Local with Docker

This repository contains the configuration to run Redash locally using Docker, with dedicated PostgreSQL and Redis.

## Prerequisites

- Docker and Docker Compose installed
- Mac with M1/M2 chip (specific configuration for ARM64)

## Environment Configuration

Before starting the setup, ensure you have a `.env` file in the root directory with the following variables:

```
REDASH_LOG_LEVEL=DEBUG
REDASH_COOKIE_DOMAIN=localhost
REDASH_PUBLIC_URL=http://localhost:5001
REDASH_HOST=http://localhost:5001
REDASH_DATABASE_URL=postgresql://postgres:postgres@postgres:5432/redash
REDASH_REDIS_URL=redis://redis:6379/0
REDASH_COOKIE_SECRET=<your_cookie_secret>
REDASH_SECRET_KEY=<your_secret_key>
REDASH_ENFORCE_CSRF=false
REDASH_ENFORCE_HTTPS=false
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=redash
```

Replace placeholders with your actual configuration values.

## Generating Secrets

To generate the `REDASH_COOKIE_SECRET` and `REDASH_SECRET_KEY`, you can use the following command in your terminal:

```bash
openssl rand -hex 32
```

This command will generate a 32-byte hexadecimal string that can be used as a secure secret key. Run it twice to generate both secrets.

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

- URL: http://localhost:5001
- Email: admin@example.com
- Password: admin

## Important Configurations

- Redash is configured to run in AMD64 mode (necessary for Mac M1/M2)
- Available services:
  - Server: Redash web interface (port 5001)
  - Scheduler: Manages query schedules
  - Worker: Processes queries in the background
  - Redis: Cache and queues (port 6379)
  - PostgreSQL: Database (port 5432)

## Stopping the Services

To stop the services:

```bash
docker-compose down
```

## Volumes

- `./data`: Stores persistent Redash data
- `./redis-data`: Stores Redis data
- `./postgres-data`: Stores PostgreSQL data

## Troubleshooting

If you encounter issues during setup or while running the services, consider the following tips:

- **Containers not starting:** Ensure Docker and Docker Compose are installed and running. Check for any error messages in the terminal.
- **Database connection errors:** Verify that the `REDASH_DATABASE_URL` in your `.env` file is correctly configured and that the PostgreSQL service is running.
- **Redis connection errors:** Ensure the `REDASH_REDIS_URL` is correct and that the Redis service is active.
- **Access issues:** Double-check the URL and port in your browser. Ensure the server is running and accessible.
- **Permission errors:** Make sure you have the necessary permissions to create and modify directories and files in the project directory.

For further assistance, consult the Redash documentation or community forums.