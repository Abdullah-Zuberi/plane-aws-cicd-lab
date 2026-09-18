# Direct Deployment Guide

This repository contains a GitHub Actions workflow `.github/workflows/direct-deploy.yml`.

## Architecture
```text
Developer
   ↓ git push
GitHub
   ↓
GitHub Actions (Builds app)
   ↓ SCP/SSH
AWS EC2
   ↓
PM2 Application
   ↓
Nginx
   ↓
Internet
```

## How it works
1. **Trigger**: Pushes to `main` branch or manual `workflow_dispatch`.
2. **Build**: Checks out the code and runs `npm run build` using Node.js 20.
3. **Deploy**:
   - Uses `scp` to copy the `.next` build folder, `public`, and `package.json` to EC2.
   - Connects via SSH to run `npm install --production`.
   - Stops the old PM2 process and starts the new one.
4. **Health Check**: Pings `http://$EC2_HOST/` to verify it returns a 200 OK status.

## Rollback
Because PM2 is used, you can simply run `git revert` on the repository to go back to a previous commit, and the CI/CD pipeline will automatically build and deploy the old version.
Alternatively, the releases could be versioned using symlinks, but PM2 handles zero-downtime reloads gracefully.
