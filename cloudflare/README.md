
  

# Setting Up Cloudflare Tunnels for Local Docker Containers

  

This guide explains how to expose your locally hosted Docker containers to the internet securely using **Cloudflare Tunnels (formerly Argo Tunnels)**. This method is superior to port forwarding as it doesn't require opening ports on your router and keeps your home IP hidden.

  

## Prerequisites

  

1. A Cloudflare account.

2. A domain name added to Cloudflare (e.g., `example.com`).

3. Docker and Docker Compose installed on your local machine.

  

## Step 1: Create the Tunnel via Cloudflare Dashboard

  

While you can do this via CLI, the **Zero Trust Dashboard** is the most user-friendly method.

  

1. Log in to the [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com/).

2. Go to **Networks** -> **Connectors**.

3. Click **Create a tunnel**.

4. Select **Cloudflared** and click **Next**.

5. Give your tunnel a name (e.g., `Home-Server`) and save it.

6. You will see a "Choose your environment" page. Under **Docker**, you will see a command containing a token.

-  **Important:** Copy only the string after `--token`. This is your `TUNNEL_TOKEN`.

  

## Step 2: Project Structure

  

Create a directory for your project to keep your configuration organized.

  

```

mkdir cloudflare-tunnel && cd cloudflare-tunnel

touch docker-compose.yml

  

```

  

## Step 3: Docker Compose Configuration

  

We will set up a `docker-compose.yml` file that runs both the `cloudflared` connector and a sample web application (like Nginx) to test the connection.

  

```
services:
	# The Cloudflare Tunnel Connector
	tunnel:
		container_name: cloudflared-tunnel
		image: cloudflare/cloudflared:latest
		restart: unless-stopped
		command: tunnel run
		environment:
		- TUNNEL_TOKEN=${TUNNEL_TOKEN}
		networks:
		- homeLab
	 
networks:
	homeLab:
		external: true
```

  

## Step 4: Environment Setup

  

Create a `.env` file in the same directory to store your token securely.

  

```

# .env

TUNNEL_TOKEN=your_copied_token_here

  

```

  

## Step 5: Configure Public Hostnames

  

Back in the Cloudflare Dashboard (where you created the tunnel):

  

1. Click the **Public Hostname** tab.

2. Click **Add a public hostname**.

3. Fill in the details:

-  **Subdomain:**  `test` (or whatever you like).

-  **Domain:** Select your domain (e.g., `example.com`).

-  **Service Type:**  `HTTP`.

-  **URL:**  `webapp:80` (This uses the Docker service name defined in your compose file).

4. Click **Save hostname**.

  

## Step 6: Deploy

  

Run the following command to start the containers:

  

```

docker-compose up -d

  

```

  

Check the logs to ensure the tunnel is connected:

  

```

docker logs -f cloudflared-tunnel

  

```

  

If you see `Connected to...`, your local container is now live at `https://test.example.com`!

  

## Troubleshooting

  

Issue

  

Solution

  

**"Bad Gateway" (502)**

  

Ensure the `URL` in Cloudflare dashboard matches the service name and port in `docker-compose.yml` (e.g., `webapp:80`).

  

**Tunnel not showing active**

  

Ensure the `TUNNEL_TOKEN` is correct and the container has internet access.

  

**DNS Issues**

  

Cloudflare automatically creates the CNAME records; ensure you didn't manually create a conflicting record.

  

## Security Best Practices
  

1.  **Access Policies:** In the Zero Trust dashboard, go to **Access** -> **Applications** to add an authentication layer (like Email OTP or Google Login) in front of your local apps.

2.  **No Port Mapping:** Avoid using `ports:` in your `docker-compose.yml` for the services you are tunneling. This ensures they are _only_ accessible via the tunnel.