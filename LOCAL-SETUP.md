# Local Postiz setup

This folder contains the official Postiz Docker Compose deployment, configured for http://127.0.0.1:4007. The dashboard port is bound to this computer only. Installation secrets are in the git-ignored `.env` file; keep that file private and retain it when restarting the services.

## Start after Windows prerequisites are ready

1. Restart Windows if requested by WSL or Docker installation.
2. Open Docker Desktop and complete its first-run setup. Wait until the Linux container engine is running.
3. Open PowerShell in this folder and run:

```powershell
docker compose config --quiet
docker compose up -d
docker compose ps
```

The first start downloads the application and its databases and may take several minutes. Open http://127.0.0.1:4007 after the services become healthy. Create a local account; this is separate from the hosted Postiz account.

## Stop and diagnose

```powershell
docker compose stop
docker compose logs --tail 100 postiz
```

Database and upload data are stored in Docker volumes. Do not use `docker compose down -v` unless you intend to delete that data.

## Connect the existing CLI

Once the local account is ready, obtain its public API key from the local developer settings. Set these variables in your terminal, without sharing the key in chat:

```powershell
$env:POSTIZ_API_URL = 'http://localhost:4007/api'
$env:POSTIZ_API_KEY = 'YOUR_LOCAL_API_KEY'
node ..\postiz-agent-main\dist\index.js integrations:list
```

Stored hosted OAuth credentials override these settings. If you have subsequently completed hosted CLI login, run `auth:logout` before switching the CLI to this local instance.

## Social accounts

The local dashboard alone does not connect social networks. Many platforms require your own developer application credentials and an appropriate public HTTPS callback address. Configure the first chosen provider after the dashboard works, then create and verify a draft before scheduling content. Your computer and Docker must remain running for local scheduled publishing.

## Sources

- https://docs.postiz.com/self-host/installation/docker-compose
- https://github.com/gitroomhq/postiz-docker-compose
- https://docs.docker.com/desktop/setup/install/windows-install/

## Current validation

Docker Desktop and WSL 2 are installed. The Compose configuration passed validation, all local Postiz services are healthy, and the registration API was verified at http://127.0.0.1:4007. The first application start can take several minutes while Postiz prepares workflow bundles for its supported platforms. Registration and background scheduling are enabled. Create the first local account when you are ready; it is separate from any hosted Postiz account.
