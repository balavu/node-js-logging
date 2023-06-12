# <h1>Node.js application with Docker, Loki, Grafana, and Promtail<h1>

# <h3>This project is a Node.js application that is dockerized and can be monitored using Loki, Grafana, and Promtail.<h3>

# <h3>Structure<h3>
├── config

    ├── datasources.yaml
    ├── promtail.yaml

├── nodejs-application

    ├── Dockerfile
    ├── application files

├── README.md

└── docker-compose.yaml

The files under config directory

'grafana-datasources.yaml' : Datasource to be configured in grafana

'promtail.yaml'            : Promtail configuration

'Dockerfile'               : This is used to create a Docker image for the Node.js application

'docker-compose.yml'       : This file is used to define and run multi-container Docker applications.

# <h2> Pre-requisites <h2>
Docker

Docker Compose

# <h2> Instructions <h2>
```bash
 git clone https://github.com/balavu/node-js-logging.git
 cd node-js-logging
 docker compose up -d # to run in detached mode
```

This project is configured to send logs from the Node.js application to Loki via Promtail. 

Grafana is used to visualize these logs

Grafana can be viewed on http://localhost:3000

NodeJs Application is exposed on port 5001 http://localhost:5001

For logs, Navigate to Grafana at http://localhost:3000

Select explore on the left

- at Label filters select container

- at Select Value select nodejs-app

Logs can be visualised

To Bring down the applications:
```bash
docker compose down
```
Grafana is not configured to have authentication as this is for POC.

To have auth enabled we can change the ENV in docker compose with

environment:

      - GF_SECURITY_ADMIN_USER=admin     # Set the username

      - GF_SECURITY_ADMIN_PASSWORD=secret  # Set the password

      - GF_AUTH_ANONYMOUS_ENABLED=false   # Disable anonymous access


Dockerfile can be modified as multi stage Dockerfile to reduce the size of the Docker Image

```bash
FROM node:14 AS base
WORKDIR /app

FROM base AS dependencies  
COPY package*.json ./
RUN npm install

FROM dependencies AS build
COPY . .
RUN npm run build

FROM base AS release  
COPY --from=dependencies /app/node_modules ./node_modules
COPY --from=build /app/build ./build
CMD ["npm", "start"]
EXPOSE 5001
```
# <h2>Thank you <h2>