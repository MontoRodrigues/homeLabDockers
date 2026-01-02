# Home Lab Docker Containers

  

Welcome to the Home Lab Docker repository. This project serves as a central hub for managing various self-hosted services using Docker and Docker Compose. Each service is isolated in its own directory with dedicated configuration files.

  

## Project Structure

  

The repository is organized by service. Below are the currently available services with links to their specific documentation:

### 🏠 Network
- before installing any service, create a network by running the following command:

```bash
docker network create homeLab
```

### 🗄️ [MinIO](./minio/README.md)

S3-compatible object storage server. Used for storing artifacts, backups, and other unstructured data.

-  **Location**: [`/minio`](./minio)

-  **Key Features**: High performance, S3 compatibility.

  

### 🔄 [n8n with FFmpeg](./n8n_with_FFMPG/README.md)

Workflow automation tool extended with FFmpeg capabilities.

-  **Location**: [`/n8n_with_FFMPG`](./n8n_with_FFMPG)

-  **Key Features**: Workflow automation, media processing support.

  

### 🐘 [PostgreSQL & pgAdmin](./postgres/README.md)

Relational database management system bundled with pgAdmin for web-based administration.

-  **Location**: [`/postgres`](./postgres)

-  **Key Features**: Reliable data storage, web-based UI for management.

  

---

  

## Getting Started

  

1.  **Clone the repository**:

```bash

git clone https://github.com/MontoRodrigues/homeLabDockers.git

cd homeLabDockers

```

  

2.  **Navigate to a service directory**:

Choose the service you want to start, for example, PostgreSQL:

```bash

cd postgres

```

  

3.  **Start the service**:

Each directory typically contains a `docker-compose.yml` file. You can start the service using:

```bash

docker-compose up -d

```

  

## Prerequisites

  

- [Docker](https://docs.docker.com/get-docker/) installed on your machine.

- [Docker Compose](https://docs.docker.com/compose/install/) installed on your machine.

  
---

*Maintained by Monto Rodrigues*