# SabziCart Docker Deployment

A Node.js Express API containerized with Docker and deployed on an AWS EC2 Ubuntu server using Nginx as a reverse proxy, DuckDNS for domain configuration, and Let's Encrypt for HTTPS.

## Live Demo

- **Domain:** https://tariqsabzicart.duckdns.org
- **Prices API:** https://tariqsabzicart.duckdns.org/api/prices
- **Health Check:** https://tariqsabzicart.duckdns.org/api/health

## Technologies Used

- Node.js and Express.js
- Docker and Docker Engine
- AWS EC2 (Ubuntu)
- Nginx Reverse Proxy
- DuckDNS
- Let's Encrypt SSL Certificate
- Certbot
- Git and GitHub (SSH authentication)

## Project Structure

```text
sabzicart-api/
├── .dockerignore
├── .gitignore
├── Dockerfile
├── package.json
├── package-lock.json
├── server.js
└── README.md
```

## API Endpoints

| Endpoint | Description |
|---|---|
| `/` | API welcome message |
| `/api/health` | Application health check |
| `/api/prices` | Returns vegetable prices in PKR per kg |

Example response:

```json
{
  "currency": "PKR per kg",
  "prices": {
    "tomato": 180,
    "onion": 120,
    "potato": 90,
    "spinach": 60
  }
}
```

## Docker Deployment

Build the Docker image:

```bash
docker build -t sabzicart-api:1.0.0 .
```

Run the container:

```bash
docker run -d \
  --name sabzicart-api \
  -p 127.0.0.1:3000:3000 \
  sabzicart-api:1.0.0
```

Check the container:

```bash
docker ps
```

View container logs:

```bash
docker logs sabzicart-api
```

## Nginx Reverse Proxy

Nginx receives public HTTP/HTTPS requests and forwards application requests to the Docker container on `127.0.0.1:3000`.

Configuration file:

```text
/etc/nginx/sites-available/sabzicart-api
```

## Domain and SSL

- Domain configured through DuckDNS.
- HTTPS certificate issued by Let's Encrypt.
- Certbot Nginx plugin used to install the certificate.
- HTTP-to-HTTPS redirection configured through Certbot (if enabled).
- Automatic certificate renewal configured with Certbot.

Test certificate renewal:

```bash
sudo certbot renew --dry-run
```

## Deployment Architecture

```text
User Browser
     |
     | HTTPS
     v
DuckDNS Domain
     |
     v
AWS EC2 Instance
     |
     v
Nginx Reverse Proxy
     |
     v
Docker Container
     |
     v
Node.js Express API
```

## GitHub Repository

https://github.com/tariq-csit/sabzicart-docker-deployment

## Author

Muhammad Tariq

GitHub: https://github.com/tariq-csit
