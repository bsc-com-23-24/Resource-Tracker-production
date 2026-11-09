
# Resource Tracker Backend - Docker Deployment Guide

## 1. Introduction

This guide explains how to deploy and run the Resource Tracker backend using Docker and Docker Compose.

The deployment package contains:

- A Docker Compose configuration file.
- An environment configuration template.
- Instructions for downloading and running the backend.
- A prebuilt backend image hosted on Docker Hub.

The backend source code is not required.

## 2. Docker Image Information

Docker Hub repository:

babylon1340/resource-tracker-api

Version:

v1.0.0

Full image name:

babylon1340/resource-tracker-api:v1.0.0

Database image:

mysql:8.4

## 3. Prerequisites

Install the following:

- Docker Engine
- Docker Compose plugin

Verify installation:

docker --version

docker compose version

## 4. Extracting the Deployment Package

Extract Resource-Tracker-Deployment.zip.

Open a terminal inside the extracted folder.

## 5. Configure Environment Variables

Create your environment file:

cp .env.example .env

Open .env and replace the example values with your own configuration.

Important:

- Use strong database passwords.
- Generate a long, random JWT secret.
- Configure the Managing Director account.
- Do not commit your .env file to GitHub.

## 6. Download Docker Images

Run:

docker compose pull

This downloads:

- The Resource Tracker backend image.
- The MySQL database image.

## 7. Start the Application

Run:

docker compose up -d

Docker Compose will:

1. Create the application network.
2. Create the MySQL container.
3. Create the persistent database volume.
4. Wait for MySQL to become healthy.
5. Start the Resource Tracker backend.

## 8. Verify the Deployment

Check container status:

docker compose ps

View backend logs:

docker compose logs -f app

View database logs:

docker compose logs -f mysql

Both services should be running.

## 9. Access the Backend

Backend:

http://localhost:3000

Swagger documentation:

http://localhost:3000/api

Check the actual Swagger route if it differs from this address.

## 10. Stop the Application

Run:

docker compose down

This stops and removes the containers and network.

The database volume is preserved.

To start the application again:

docker compose up -d

## 11. Update the Application

When a newer backend image is released:

1. Obtain the new deployment configuration and version.
2. Update the image tag in docker-compose.yml.
3. Run:

docker compose pull

4. Recreate the services:

docker compose up -d

## 12. Troubleshooting

### Port 3000 already in use

Change the host port mapping:

"3001:3000"

The API will then be accessible at:

http://localhost:3001

### Database connection errors

Check:

- DB_HOST is set to mysql.
- DB_PORT is set to 3306.
- Database credentials match.
- MySQL is healthy.

Run:

docker compose logs -f mysql

### Application errors

Run:

docker compose logs -f app

### Check container status

docker compose ps

## 13. Database Persistence

MySQL data is stored in a Docker named volume called mysql_data.

Stopping the containers does not delete the database.

Do not run:

docker compose down -v

unless you intentionally want to remove the database volume and its data.

## 14. Important Security Notes

- Never share production passwords.
- Never upload the real .env file to a public repository.
- Use strong JWT secrets.
- Use database migrations for production deployments.
- Back up the database regularly.
