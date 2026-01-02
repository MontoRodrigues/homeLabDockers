
# Home Lab Docker Containers

Welcome to the Home Lab Docker repository. This project serves as a central hub for managing various self-hosted services using Docker and Docker Compose. Each service is isolated in its own directory with dedicated configuration files.

 ## Project Structure
 
The repository is organized by service. Below are the currently available services with links to their specific documentation:

### 🏠 Network
- before installing any service, create a network by running the following command: 
```bash
docker  network  create  homeLab
```

### 🗄️ [Cloudflare](./cloudflare/README.md)
A tunnel to serve your local Docker container over the internet 

-  **Location**: [`/cloudflare`](./cloudflare)
-  **Key Features**: Expose your local Docker container over the internet.


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

## Setting up your Home Lab

  
1.  **Clone the repository**:
```bash 
git  clone  https://github.com/MontoRodrigues/homeLabDockers.git
cd  homeLabDockers
```
2.  **Create Network**:
Create the network for your home lab so containers and services and talk to each other.
```bash 
docker  network  create  homeLab
```
3.  **Run CloudFlare Container**:
Navigate to the cloudflare directory, user the [README.MD](./cloudflare/README.md) to setup and run the following command on your terminal to run the cloudflare container:
```bash
cd  cloudflare
docker-compose  up  -d
```


4.  **Install Psotges SQL**:
Navigate to the postgres directory and run the following command:

```bash
cd  postgres
docker-compose  up  -d
```

6.  **Install MinIO**:
Navigate to the minio directory and run the following command:
```bash
cd  minio
docker-compose  up  -d
```

7.  **Install n8n with FFmpeg**:
Navigate to the n8n_with_FFMPG directory and run the following command:
```bash
cd  n8n_with_FFMPG
docker-compose  up  -d
```

8.  **Setup application routes in CLoudflare**:
-  Go to the Cloudflare dashboard and navigate to the **Zero Trust** section.
-  Click on **Connectors** and then **Tunnels**.
-  Click on the tunnel you created in step 3.
- click on **Published application routes** and then click **Add a published application route**. Add the details for each application as mentioned below:
    - n8n:
        -   **Subdomain:**  `n8n` (or whatever you like).    
        -   **Domain:** Select your domain (e.g., `example.com`).    
        -   **Service Type:**  `HTTP`.    
        -   **URL:**  `n8n:5678` (This uses the Docker service name defined in your compose file).
    - pgAdmin:
        -   **Subdomain:**  `pgadmin` (or whatever you like).    
        -   **Domain:** Select your domain (e.g., `example.com`).    
        -   **Service Type:**  `HTTP`.    
        -   **URL:**  `pgaadmin:80` (This uses the Docker service name defined in your compose file).
    - minIO:
        -   **Subdomain:**  `minio` (or whatever you like).    
        -   **Domain:** Select your domain (e.g., `example.com`).    
        -   **Service Type:**  `HTTP`.    
        -   **URL:**  `minio:9000` (This uses the Docker service name defined in your compose file).  
     - minIO API:
        -   **Subdomain:**  `minioapi` (or whatever you like).    
        -   **Domain:** Select your domain (e.g., `example.com`).    
        -   **Service Type:**  `HTTP`.    
        -   **URL:**  `minio:9001` (This uses the Docker service name defined in your compose file).               
-  Navigate to the cloudflare directory and run the following command:

Your home lab is now ready and accessible over the internet.

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/) installed on your machine.
- [Docker Compose](https://docs.docker.com/compose/install/) installed on your machine.

--- 

*Maintained by Monto Rodrigues*