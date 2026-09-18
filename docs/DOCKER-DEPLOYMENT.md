# Docker Deployment Guide

This repository contains a GitHub Actions workflow `.github/workflows/docker-deploy.yml`.

## Architecture
```text
Developer
   ↓ git push
GitHub
   ↓
GitHub Actions
   ↓ Docker Build
Docker Hub
   ↓ docker pull
AWS EC2
   ↓
Docker Container
   ↓
Nginx
   ↓
Internet
```

## How it works
1. **Trigger**: Manual `workflow_dispatch` (can be configured to run on push to main).
2. **Build**: Builds a multi-stage `Dockerfile` to optimize the Node.js application image size.
3. **Publish**: Pushes the Docker image to Docker Hub, tagged with the exact GitHub Commit SHA and `latest`.
4. **Deploy**:
   - Connects to the EC2 instance.
   - Kills any PM2 processes (Direct Deployment) to avoid port conflicts.
   - Pulls the exact SHA image.
   - Stops the old Docker container.
   - Runs the new container binding port 3000 to the host.
5. **Health Check**: Pings `http://$EC2_HOST/` to verify it returns a 200 OK status.

## Rollback
Because every image is tagged with the Git commit SHA, rolling back is as simple as re-running the workflow from a previous commit or manually pulling/running the older tag on EC2:
```bash
docker run -d -p 3000:3000 myusername/plane-aws-cicd:<previous-sha>
```
