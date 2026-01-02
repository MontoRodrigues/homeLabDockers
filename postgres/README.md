
# PostgreSQL & pgAdmin Docker Setup

This repository provides a containerised environment for **PostgreSQL 17** and **pgAdmin 4** using Docker Compose. It features persistent data storage, health checks, and environment-based configuration.

## 🚀 Quick Start

### 1. Prerequisites

Ensure you have the following installed:

-   [Docker Desktop](https://www.docker.com/products/docker-desktop/)
	-  [Install Docker Desktop on Mac](https://docs.docker.com/desktop/setup/install/mac-install/)
	- [Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
-  [Docker Compose](https://docs.docker.com/compose/install/)
    
### 2. Configuration
- Create a Directory with the name `postgress` (you can name it what ever you want). 

### 3. Configuration
- Create the following file  `postgress/.env`.
- The setup uses an `.env` file to manage credentials securely. Ensure your `.env` file is in the root directory:
- Copy the code snippet

```env
POSTGRES_USER=<user Name>
POSTGRES_PASSWORD=<strong Passsword>
POSTGRES_DB=<default DB>

PGADMIN_EMAIL=<user email>
PGADMIN_PASSWORD=<strong your_pgadmin_password>
```

### 4. Create the Composer file
- Create the following file at `docker-compose.yml`.
- copy the below code in the docker-compose.yml file
```yaml
services:
	db:
		image: postgres:17
		container_name: postgres_db
		restart: always
		environment:
		POSTGRES_USER: ${POSTGRES_USER}
		POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
		POSTGRES_DB: ${POSTGRES_DB}
		ports:
		- "5432:5432"
		volumes:
		- ./postgres_data:/var/lib/postgresql/data
		healthcheck:
		test: [ "CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}" ]
		interval: 10s
		timeout: 5s
		retries: 5
		shm_size: 128mb

	pgadmin:
		image: dpage/pgadmin4
		container_name: pgaadmin
		restart: always
		environment:
		- PGADMIN_DEFAULT_EMAIL=${PGADMIN_EMAIL}
		- PGADMIN_DEFAULT_PASSWORD=${PGADMIN_PASSWORD}
		ports:
		- "8080:80"
		volumes:
		- ./pg_admin_data:/var/lib/pgadmin
		depends_on:
		- db
```

### 5. Launch the Stack

Run the following command to start the containers in the terminal:
```
docker-compose up -d
```


### 6 Connecting pgAdmin to the Database

Once you log into pgAdmin at `http://localhost:8080`, follow these steps to connect to your database:

1.  **Add New Server:** Right-click 'Servers' > Register > Server.
    
2.  **General Tab:** Name it (e.g., `Local Postgres`).
    
3.  **Connection Tab:**
    
    -   **Host name/address:** `postgres_db` (This is the container name defined in `docker-compose.yml`).
        
    -   **Port:** `5432`
        
    -   **Maintenance database:** `<defualt data base>` (Matches `${POSTGRES_DB}` in .env)
        
    -   **Username:** `<user name>` (Matches `${POSTGRES_USER}` in .env)
        
    -   **Password:**:<password> (Matches `${POSTGRES_PASSWORD}` in .env)
        

----------

## 💾 Data Persistence

To ensure your data isn't lost when containers are stopped or removed, the following local volumes are created automatically:

-   `./postgres_data`: Stores all SQL tables and records.
    
-   `./pg_admin_data`: Stores pgAdmin settings and server connections.
    

> **Note:** These directories are ignored by Git (if configured) to prevent sensitive data or large binaries from being committed.
