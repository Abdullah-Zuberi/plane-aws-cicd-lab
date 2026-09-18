# AWS CI/CD Lab: Direct vs Docker Deployment

**Objective**: Complete two deployment approaches (Direct PM2 and Docker) for an application using GitHub Actions, AWS EC2, and Docker Hub.

**Original Upstream Application**: Plane (See `UPSTREAM.md` and `FEASIBILITY.md`)
**Deployed Application**: `vercel/nextjs-portfolio-starter` (Located in `/fallback-app`)

## Architectures

### Direct Deployment
```text
Developer → GitHub → GitHub Actions → AWS EC2 → PM2 → Nginx → Internet
```

### Docker Deployment
```text
Developer → GitHub → GitHub Actions → Docker Build → Docker Hub → AWS EC2 → Docker Container → Nginx → Internet
```

## Setup Instructions
Please refer to the detailed documentation in the `/docs` folder:
1. `docs/AWS-SETUP.md`: EC2 setup, AMI selection, and dependency installation.
2. `docs/DIRECT-DEPLOYMENT.md`: Instructions and details for the PM2-based Direct Deployment pipeline.
3. `docs/DOCKER-DEPLOYMENT.md`: Instructions and details for the Docker-based Deployment pipeline.
4. `docs/TROUBLESHOOTING.md`: Common issues and switching between deployments.

## CI/CD Quality Requirements Met
- **Clear Job/Step Names**: Self-documenting workflow files.
- **Fail-Fast**: The health checks run `curl -f` and will exit on failure.
- **Git SHA Versioning**: Docker images are tagged precisely with the Git commit SHA for traceability.
- **Safe Secrets**: All SSH keys, hosts, and tokens are stored securely in GitHub Secrets.

## Resources Used (AWS Free Tier)
- `t3.micro` instance
- Ubuntu 22.04 LTS
- Security Group allowing only Port 22 and Port 80
