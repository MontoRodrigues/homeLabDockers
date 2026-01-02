# Installing n8n with FFmpeg

This guide provides step-by-step instructions on how to set up n8n with FFmpeg. While you can install n8n via NPM, using **Docker** is the recommended method because it allows us to easily bundle FFmpeg within the n8n container, ensuring all dependencies are met without polluting your host system.

## Prerequisites

-   **Docker** installed on your machine.
    
-   **Docker Compose** installed.
    
-   Basic knowledge of the terminal/command line.
    

## Method 1: Using Docker Compose (Recommended)

This is the cleanest way to run n8n with FFmpeg. We will use a custom Dockerfile to ensure FFmpeg is available to the n8n process.

### Step 1: Create a Project Directory

Open your terminal and create a folder for your n8n setup:

```
mkdir n8n-ffmpeg && cd n8n-ffmpeg
```

### Step 2: Create a Custom Dockerfile

We need to add FFmpeg to the base n8n image. Create a file named `Dockerfile`:

```dockerfile
FROM docker.n8n.io/n8nio/n8n:latest

USER  root
RUN  set  -eux;  \
	if  command  -v  apt-get  >/dev/null  2>&1;  then  \
		apt-get  update  &&  apt-get  install  -y  --no-install-recommends  ffmpeg  &&  \
		rm  -rf  /var/lib/apt/lists/*;  \
	elif  command  -v  apk  >/dev/null  2>&1;  then  \
		apk  add  --no-cache  ffmpeg;  \
	else  \
		echo  "No  supported  package  manager  found  (expected  apt-get  or  apk)";  \
	exit  1;  \
	fi

USER  node
```

### Step 3: Create the Docker Compose File

Create a file named `docker-compose.yml` in the same directory:

```yaml
services:
	n8n:
		build: .
		# Local default: expose 5678
		container_name: n8n
		ports:
		- "5678:5678"
		environment:
			# Database
			DB_TYPE: postgresdb
			DB_POSTGRESDB_HOST: ${POSTGRES_HOST}
			DB_POSTGRESDB_PORT: 5432
			DB_POSTGRESDB_DATABASE: ${POSTGRES_DB}
			DB_POSTGRESDB_USER: ${POSTGRES_USER}
			DB_POSTGRESDB_PASSWORD: ${POSTGRES_PASSWORD} # Security & basics
			N8N_ENCRYPTION_KEY: ${N8N_ENCRYPTION_KEY}
			GENERIC_TIMEZONE: ${GENERIC_TIMEZONE}
			N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS: true
			N8N_RUNNERS_ENABLED: true
			# Access / URLs (set these if using a reverse proxy or a domain)
			N8N_HOST: ${N8N_HOST:-localhost}
			N8N_PORT: 5678
			N8N_PROTOCOL: ${N8N_PROTOCOL:-http}
			N8N_EDITOR_BASE_URL: ${WEBHOOK_URL:-}
			WEBHOOK_URL: ${WEBHOOK_URL:-}
			# Optional: privacy/telemetry
			N8N_DIAGNOSTICS_ENABLED: true
			# Cookies secure in prod (set PRODUCTION=true to enable)
			N8N_SECURE_COOKIE: true
		volumes:
		- ./data:/home/node/.n8n
		restart: unless-stopped
	
```

### Step 4: Build and Start

Run the following command to build your custom image and start the container:

```
docker-compose up -d --build

```

### Step 5: Access N8N
- URL:  [http://localhost:5678/](http://localhost:5678/)