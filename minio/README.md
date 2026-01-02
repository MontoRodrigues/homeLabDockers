
# MinIO AIStor Container Installation Guide

This README provides instructions for deploying the **MinIO AIStor Object Store** using containerization (Docker or Podman). This guide is based on the [official MinIO Enterprise documentation](https://docs.min.io/enterprise/aistor-object-store/installation/container/install/ "null") with some modifications that made it easy. 

## Prerequisites

-   **Container Engine**: Docker Engine 18.03+ or Podman 2.0+.
	- [Install Docker Desktop on Mac](https://docs.docker.com/desktop/setup/install/mac-install/)
	- [Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
    
-   **License**: A valid MinIO SUBNET license file (`minio.license`).
	- [Go to](https://www.min.io/pricing) and click get started on the free plan. You will be prompted with a form. Enter the details and license would be sent to your email. 	
    
-   **Operating System**: Linux (64-bit) with a modern kernel.
    
-   **Hardware**: Sufficient RAM and Disk space as per your storage requirements.
    
## 1. Create a Directories
- Create a Directory with the name `minio` (you can name it what ever you want). 
- create a directory `minio/data`. hange this path to reflect the folder, storage volume, or drive intended for use with the server.

## 2. Retrieve License
- Log into [SUBNET](https://subnet.min.io/?jmp=docs) and select the **License** button in the **Deployments** view.
- Download the file to `minio/minio.license` for use in later steps.

## 3. Create Config and Environment variables
- Create the following file at `minio/config`. The server uses this file as the source of all environment variables. Imp. the file dosent have an extension. 
- Copy the following code in the config file
```
# Copy the value of your SUBNET license below
MINIO_LICENSE="/mnt/minio/minio.license"
# Define the expected topology of the deployment
#
# The following is a single-node single-drive deployment pattern
# appropriate for workloads where distributed performance and
# high availability are not requirements.
#
# Single-Node Single-Drive Startup
MINIO_VOLUMES="/mnt/minio/data"
# Set all Server command-line options
#
# Use this envvar to add other CLI parameters to the `minio server` command.
MINIO_OPTS=""
MINIO_CONFIG_ENV_FILE="/mnt/minio/config"

# Extended mode allows AIStor to encode object or prefix names with special characters.
# This must be enabled at the first run of AIStor on a cluster.
MINIO_OBJECT_NAMING=extended
```

- Create the following file at `minio/.env`. This file stores the environment variables especially the credentials. copy the following code in the .env file.
```
#MINIO
MINIO_USER=<user name>
MINIO_PASSWORD=<strong password>
```

## 4. Create Docker Compose file
- Create the following file at `minio/docker-compose.yml`.
- copy the below code in the docker-compose.yml file
```yaml
services:
	minio:
		image: quay.io/minio/aistor/minio:latest
		container_name: minio
		ports:
		- "9000:9000"  # S3 API endpoint
		- "9001:9001"  # MinIO Console (web UI)
		environment:
		MINIO_CONFIG_ENV_FILE: "/mnt/minio/config"
		MINIO_ROOT_USER: ${MINIO_USER} 
		MINIO_ROOT_PASSWORD: ${MINIO_PASSWORD}  
		volumes:
		- ./:/mnt/minio  
		command: minio server
```

## 5. Run Container
- run the following command in the terminal.
```bash
docker-compose up -d
```


## 6. Verification

To verify that the server is running correctly, check the logs:

```bash
docker logs minio-aistor

```

You should see output indicating that the API and Console are listening.

## 7. Accessing the UI

1.  Open your browser and go to `http://localhost:9001`.
    
2.  Log in using the `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD` defined in your `.env` file.
    

## 8. Storage Best Practices

-   **Persistence**: Ensure your host volume (`~/minio-aistor/data`) is backed up.
    
-   **Security**: For production, always use TLS/SSL by mounting certificates to the `/etc/minio/certs` directory.
    
-   **Resource Limits**: Consider adding `--memory` and `--cpus` flags to the `docker run` command to ensure system stability.
    

_For more detailed information, visit the_ [_MinIO Documentation_](https://docs.min.io/ "null")_._