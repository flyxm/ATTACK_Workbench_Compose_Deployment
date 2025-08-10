# ATT&CK_Workbench_Compose_Deployment
 The ATT&amp;CK Workbench deployment project provides a complete solution that integrates frontend, REST API, MongoDB, Navigator, and Portainer container management tools, supporting rapid deployment and operation.

## Understand the core concepts first

### ATT&CK Workbench: 
A tool for managing threat intelligence (STIX format data), similar to a "threat intelligence repository", that allows users to upload, edit, and delete threat data (such as attack methods, malware information, etc.) on their own.
 It provides a user-friendly interface that facilitates centralized management and analysis of threat intelligence by security analysts and researchers.

### ATT&CK Navigator: 
A visualization tool used to "visually display" threat intelligence (such as painting attack path maps). Through a graphical interface, users can have a clearer understanding of the attacker's behavior patterns and attack paths, thereby better formulating defense strategies.

### ATT&CK Website: 
The official threat intelligence website of MITRE (<>), but you can also "replace" the website content with your own Workbench data to become your own threat intelligence website. This allows users to customize and display threat intelligence information according to their own needs.

### MongoDB: 
As a database component of ATT&CK Workbench, it is used to store threat intelligence data.
It provides efficient data storage and retrieval capabilities, supporting fast access and management of large-scale data.

### Docker Compose: 
A tool for defining and managing multi container applications.
In ATT&CK Workbench deployment, it simplifies the orchestration and startup process of front-end, back-end, database, and other services, making the deployment of the entire system more convenient and efficient.

### Portainer: 
A lightweight container management interface designed to simplify the management and monitoring of Docker containers. 
It provides an intuitive web interface for users to view and manage running resources such as containers, networks, volumes, etc., ensuring the stable operation of the entire ATT&CK Workbench system.

## Related technical documents:
### [attack-workbench-frontend](https://github.com/center-for-threat-informed-defense/attack-workbench-frontend/)
### [attack-workbench-rest-api](https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api)
### [attack-website](https://github.com/mitre-attack/attack-website/tree/master)
### [attack-navigator](https://github.com/mitre-attack/attack-navigator/)



## Overview of Deployment Architecture
Manage all containers through Docker Compose, build an isolated attack network environment, and ensure communication between services through service names.
 The core components include:

### Attack workbench frontend: 
Front end interface that calls REST API.

### Attack workbench rest API: 
Provides API interface to connect to MongoDB.

### MongoDB: 
Stores ATT&CK Workbench data.

### Attack navigator: 
An independent MITRE ATT&CK navigation tool (standalone service).

### Portainer: 
Container Management Panel (optional for monitoring and managing containers).

### Docker Compose: 
A tool for defining and managing multi container applications.

## Deployment Environment
<font face="HEI">Docker Desktop WSL 2 backend on Windows</font>

### Requirements

#### [Node.js v22.18](https://nodejs.org/en)

Run PowerShell 

```powershell
node -v
npm -v
```

#### [AngularCLI v17](https://cli.angular.io/)

Run PowerShell 

```powershell
ng -v
```

#### [Git v2.50.1](https://git-scm.com/downloads)

Run PowerShell 

```powershell
git -v
```

#### [Docker Desktop v4.43.2](https://docs.docker.com/desktop/setup/install/windows-install/)

Run PowerShell 

```powershell
wsl -v  
```

## Quick start

### Create project directory

Run PowerShell

```powershell
mkdir attack-workbench    #The case is installed in the root directory of drive D and can be modified according to the actual situation
cd attack-workbench
```

### Create network

Run PowerShell

```powershell
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-frontend.git
git clone https://github.com/center-for-threat-informed-defense/attack-workbench-rest-api.git
git clone https://github.com/mitre-attack/attack-navigator.git
```

### Configure attack-workbench-frontend

#### Enter the attack-workbench-frontend project directory：

```PowerShell
cd D:\attack-workbench\attack-workbench-frontend
```

#### Initialize and pull submodules:

```PowerShell
#Initialize submodule configuration
git submodule init

#Pull submodule code
git submodule update 
```

#### Install dependencies and build the project

```PowerShell
#Install dependencies
npm install

#Used to attempt to fix all possible issues, including those that may introduce disruptive changes.
npm audit fix --force

#To obtain more detailed information on security vulnerabilities, there is no need to take any action
npm audit

#Build project
npm run build
```

#### Confirm the generation of the build file and ensure that there is an index.html file in the dist directory

```powershell
ls D:\attack-workbench\attack-workbench-frontend\dist\app\browser
```
View the situation
```powershell
PS D:\attack-workbench\attack-workbench-frontend> Get-ChildItem -Path D:\attack-workbench\attack-workbench-frontend -Filter index.html -Recurse


    Directory: D:\attack-workbench\attack-workbench-frontend\dist\app\browser


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          2025/8/6     12:29            856 index.html
```

### Configure attack-navigator

The process of building a Navigator image involves packaging your application code and dependencies into a Docker image.
 This is usually done through Dockerfiles.

Why do we need to build an image?

Encapsulation dependencies: Ensure that your application runs consistently in any environment, with all dependencies included in the image.

Easy to deploy: Built images can easily run on different machines without the need for manual installation of dependencies.

#### Building a Navigator image involves integrating your application code and dependencies

```powershell
cd D:\attack-workbench\attack-navigator\nav-app

#installation of Angular CLI
npm install -g @angular/cli

#Verification Angular CLI
ng -v

#Install dependencies
npm install

#Building Angular Projects
ng build
```

#### Building Navigator Images with Docker

```powershell
cd d:\attack-workbench\attack-navigator

#Build an image labeled as attacknavigator
docker build -t attacknavigator .   
```

notes!!!    

If the command [docker build - t attacknavigator.] reports an error

Error message: Indicates that Docker encountered an issue while attempting to pull the node: 18 image from Docker Hub.


Error message: 

```powershell
PS D:\attack-workbench\attack-navigator> docker build -t attacknavigator .
[+] Building 21.3s (3/3) FINISHED                                                                                           docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                        0.0s
 => => transferring dockerfile: 497B                                                                                                        0.0s
 => ERROR [internal] load metadata for docker.io/library/node:18                                                                           21.2s
 => [auth] library/node:pull token for registry-1.docker.io                                                                                 0.0s
------
 > [internal] load metadata for docker.io/library/node:18:
------
Dockerfile:3
--------------------
   1 |     # Build stage
   2 |
   3 | >>> FROM node:18
   4 |
   5 |     ENV NODE_OPTIONS=--openssl-legacy-provider
--------------------
ERROR: failed to build: failed to solve: failed to fetch oauth token: Post "https://auth.docker.io/token": dial tcp 31.13.83.2:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/i0d0d5mcylrz0yafvqc99kfgq
```

Solution: Manually pull the node: 18 image

```powershell
#Manually pull node: 18 image
docker pull node:18

#Confirm the existence of the node18 image
docker ps

#Build the image again
docker build -t attacknavigator .   
```

### Configure Docker Compose

#### First time

```powershell
cd attack-workbench-frontend/docker/compose
```

Edit the 'docker-compose. yml' file      

```yml
version: "3.9"
services:
  frontend:
    container_name: attack-workbench-frontend
    image: ghcr.io/center-for-threat-informed-defense/attack-workbench-frontend:latest
    build: ../../../attack-workbench-frontend
    depends_on:
      - rest-api
    ports:
      - "8080:8080"
    volumes:
      - D:/attack-workbench/attack-workbench-rest-api/app/config:/usr/src/app/app/config
      - D:/attack-workbench/attack-workbench-frontend/docker/compose/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - D:/attack-workbench/attack-workbench-frontend/dist/app/browser:/usr/share/nginx/html 
      
  rest-api:
    container_name: attack-workbench-rest-api
    image: ghcr.io/center-for-threat-informed-defense/attack-workbench-rest-api:latest
    build: ../../../attack-workbench-rest-api
    depends_on:
      - mongodb
    ports:
      - "3000:3000"
    volumes:
      - D:/attack-workbench/attack-workbench-rest-api/resources/rest-api-service-config.json:/usr/src/app/resources/rest-api-service-config.json:ro
      - D:/attack-workbench/attack-workbench-rest-api/app/api/definitions/openapi.yml:/usr/src/app/app/api/definitions/openapi.yml
    environment:
      - DATABASE_URL=mongodb://attack-workbench-database/attack-workspace
      - SERVICE_ACCOUNT_APIKEY_ENABLE=true
      - JSON_CONFIG_PATH=/usr/src/app/resources/rest-api-service-config.json
      - WORKBENCH_HOST=http://attack-workbench-rest-api
      - WORKBENCH_AUTHN_SERVICE_NAME=collection-manager
      - WORKBENCH_AUTHN_APIKEY=sample-key

  mongodb:
    container_name: attack-workbench-database
    image: mongo:latest
    volumes:
      - db-data:/data/db
    ports:
      - "27017:27017"
      
  navigator:
    container_name: attack-navigator
    image: attacknavigator  
    ports:
      - "4200:4200"
    volumes:
      - D:/attack-workbench/attack-navigator/nav-app/src/assets:/src/nav-app/src/assets
    environment:
      - DATA_SOURCE=http://rest-api:3000  
    depends_on:
      - rest-api
      
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
      
volumes:
  portainer_data:
  db-data:
```

#### Second time  

```powershell
cd D:\attack-workbench\attack-workbench-frontend\docker\compose\nginx
```

Edit the 'nginx.conf' file     

```conf
worker_processes  1;

events {
  worker_connections  1024;
}

http {
  server {
    listen 8080;
    server_name  localhost;

    proxy_connect_timeout 75;
    proxy_send_timeout 300;
    proxy_read_timeout 300;

    root   /usr/share/nginx/html;
    index  index.html index.htm;
    include /etc/nginx/mime.types;

    gzip on;
    gzip_min_length 1000;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css application/json application/javascript application/x-javascript text/xml application/xml application/xml+rss text/javascript;

    location / {
      try_files $uri $uri/ /index.html;
    }

    location /api {
      client_max_body_size 50M;
      proxy_pass http://172.18.0.5:3000;
    }
  }
}

```


#### Third time 

```powershell
cd D:\attack-workbench\attack-workbench-rest-api\resources\
```

Edit the 'rest-api-service-config.json' file      

```json
{
  "serviceAuthn": {
    "challengeApikey": {
      "enable": false,
      "serviceAccounts": [
        {
          "name": "collection-manager",
          "apikey": "sample-key",
          "serviceRole": "collection-manager"
        }
      ]
    },
      "basicApikey": {
      "enable": true,
      "serviceAccounts": [
        {
          "name": "navigator",
          "apikey": "sample-key",
          "serviceRole": "read-only"
        },
        {
          "name": "apikey-test-service",
          "apikey": "sample-key",
          "serviceRole": "stix-export"
        },
        {
          "name": "collection-manager",
          "apikey": "sample-key",
          "serviceRole": "collection-manager"
        }

      ]
    }
  }
}
```


#### Start Docker container

```powershell
cd D:\attack-workbench\attack-workbench-frontend\docker\compose

#For the first run, the backend needs to download relevant images
docker compose up -d 
```




## Verification Services

Attack workbench frontend： https://localhost:8080 (in your browser)

Attack workbench rest API： http://localhost:3000 (in your browser)

MongoDB： http://localhost:27017 (Using [MongoDB Compass](https://www.mongodb.com/try/download/compass) client tools)


Attack navigator： http://localhost:4200 (in your browser)

Portainer： http://localhost:9000 (in your browser)
 
