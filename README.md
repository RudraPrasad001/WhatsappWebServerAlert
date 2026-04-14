# Minecraft Server WhatsApp Assistant

A lightweight WhatsApp bot and server assistant that automatically monitors your Minecraft server, sends real-time alerts to your group chat, and provides on-demand player stats right from your phone. 

## Features
* **Automated Server Monitoring:** Checks the server every 5 minutes and sends an alert to the group if the status changes (UP/DOWN).
* **Interactive Chat Commands:** Group members can interact with the bot to get live server statistics and player rosters.
* **Serverless & Flexible:** Can be deployed on any serverless platform (like Render, Netlify, Vercel) or run on a traditional VPS.
* **Extensible:** Built to easily accommodate new features, commands, and server management tools.

## Commands
The bot currently supports the following commands in the WhatsApp group:
* `!status` - Instantly check if the server is online and see the current player capacity.
* `!players` - Get a live, formatted list of the usernames currently playing on the server.
* `!help` - Displays the command menu.

## Tech Stack
* **Hosting / Cron:** Any platform that can host Node.js API routes and trigger a 5-minute background Cron Job.
* **Green-API:** Handles the WhatsApp connection (sending messages and receiving webhooks).
* **Redis (e.g., Upstash):** A lightweight database that remembers if the server was "UP" or "DOWN" during the last check.
* **mcstatus.io:** A fast API used to ping the Minecraft server (automatically resolves SRV records).
* **Node.js:** The core logic.

## Prerequisites
Before deploying, you need accounts/details for the following:
1. **Green-API:** A free "Developer" account, your Instance ID, and API Token.
2. **Hosting Provider:** A platform to host your code (Render, Railway, VPS, etc.).
3. **Redis Database:** A free Upstash Redis database (or your own Redis instance).
4. **Minecraft Server:** Your public IP/Host and Port (e.g., from Playit.gg).
5. **WhatsApp Group/Chat:** The specific ID of your WhatsApp Group/Chat (must end in `@g.us`).

## Setup & Deployment

### 1. Environment Variables
In your chosen hosting environment, you will need to add the following Environment Variables:

| Variable | Description | Example |
| :--- | :--- | :--- |
| `MC_SERVER_IP` | Your public server address | `my-minecraft-server.domain` |
| `MC_SERVER_PORT` | Your server port | `23531` |
| `GREEN_API_ID` | From your Green-API console | `7107...` |
| `GREEN_API_TOKEN` | From your Green-API console | `abc123...` |
| `TARGET_GROUP_ID` | Your WhatsApp Group/Chat ID | `120363...123@g.us` |
| `UPSTASH_REDIS_REST_URL` | From your Upstash console | `https://...upstash.io` |
| `UPSTASH_REDIS_REST_TOKEN` | From your Upstash console | `AX...` |

### 2. Redis Configuration
Ensure you have a Redis database accessible via a REST API (such as Upstash). Add the connection URL and Token to your environment variables as `UPSTASH_REDIS_REST_URL` and `UPSTASH_REDIS_REST_TOKEN`.

### 3. Deploy and Set Up the Cron Job
1. Deploy the project to your hosting provider.
2. Set up a Cron Job (using your provider's native tools, or a free service like cron-job.org) to ping the `/api/cron` endpoint of your hosted app every 5 minutes.

### 4. Webhook Configuration (Green-API)
To enable the interactive commands, configure your Green-API instance to forward webhooks to `https://<your-hosted-domain>/api/webhook`. Ensure it is set to receive notifications for incoming messages (and optionally, outgoing messages if you want the bot to reply to your own commands).

## Testing
* Type `!help` or `!players` in your WhatsApp group to test the interactive commands.
* Turn your server off, wait for the cron job to run (or manually visit `https://<your-hosted-domain>/api/cron`), and watch for the offline alert!