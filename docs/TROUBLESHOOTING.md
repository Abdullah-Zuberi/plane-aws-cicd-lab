# Troubleshooting Guide

## Deployment Pipeline Fails
- **GitHub Actions Logs**: Check the specific step that failed in the GitHub Actions dashboard.
- **SSH Connection Timeout**: Verify that the EC2 Security Group allows inbound SSH (Port 22) from GitHub Actions (or `0.0.0.0/0` for testing).
- **Authentication Failed**: Verify `EC2_SSH_PRIVATE_KEY` has been copied correctly into the secrets and is in PEM format with correct newlines.

## Application Returns 502 Bad Gateway
- The Node.js application is likely not running on port 3000.
- For Direct Deployment, SSH into the instance and run `pm2 status` and `pm2 logs`.
- For Docker Deployment, SSH into the instance and run `docker ps` and `docker logs plane-app`.
- Ensure there is no conflict between PM2 and Docker trying to use port 3000 at the same time. The Docker deployment script explicitly deletes the PM2 app before running.

## Cannot Pull Docker Image
- Ensure `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` are correctly set in GitHub Secrets.
- Verify the repository is public on Docker Hub or that the EC2 instance has pulled it properly.

## Switching Deployments
**To switch from Docker to Direct:**
```bash
docker stop plane-app && docker rm plane-app
pm2 start fallback-app
```

**To switch from Direct to Docker:**
```bash
pm2 stop fallback-app
docker start plane-app
```
