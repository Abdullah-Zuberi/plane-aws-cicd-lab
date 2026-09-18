# Plane Feasibility Check

## 1. Applications/Services Included in Plane
After reviewing the `docker-compose.yml`, `README.md`, and `.env.example`, Plane contains the following services:
- **web**: Next.js frontend
- **admin**: Admin dashboard frontend
- **space**: Additional frontend
- **api**: Django backend API
- **worker**: Background worker (Celery)
- **beat-worker**: Scheduled tasks worker
- **migrator**: Database migration service
- **live**: Real-time service
- **plane-db**: PostgreSQL database
- **plane-redis**: Redis cache/message broker
- **plane-minio**: MinIO object storage
- **proxy**: Reverse proxy for routing

## 2. Frontend Technology
Next.js (React), React Router, Node.js.

## 3. Backend Technology
Django (Python), Celery.

## 4. Database Required
PostgreSQL.

## 5. Redis/Valkey Requirements
Redis is required for caching and background task messaging.

## 6. RabbitMQ Requirements
RabbitMQ is referenced in the `.env.example` as an option/requirement for robust message brokering, though Redis can sometimes act as a broker.

## 7. Object Storage
MinIO (or AWS S3) is required for file uploads and attachments.

## 8. Recommended Resources
While Plane's documentation implies a multi-container architecture, running this many heavily-loaded containers (Postgres, Redis, Django, multiple Next.js apps, MinIO) typically requires at least 4GB to 8GB of RAM and 2-4 vCPUs for stable operation.

## 9. AWS Free Tier Feasibility
An AWS Free Tier eligible EC2 instance is a `t2.micro` or `t3.micro`, which provides only 1 vCPU and 1 GB of RAM. 
Running the full Plane stack (10+ containers) on 1 GB of RAM will immediately lead to Out-Of-Memory (OOM) errors. The memory overhead of just PostgreSQL, Redis, and the Next.js build process exceeds 1 GB. Therefore, it is **not realistic** to run the complete Plane stack on a Free Tier EC2 instance.

## 10. Native Deployment Feasibility
Deploying the complete Plane application natively without Docker is theoretically possible but highly impractical. It would require installing and configuring Node.js, Python, PostgreSQL, Redis, MinIO, and a reverse proxy directly on the host. Managing the background workers, live services, and API dependencies would be highly error-prone and would still fail on a Free Tier instance due to memory constraints.

## Conclusion and Fallback
Because the Plane stack cannot run reliably within the available Free Tier EC2 resources, I will be using a lightweight open-source Next.js application as a fallback for both the Direct Deployment and Docker Deployment.

**Fallback Application**: `vercel/nextjs-portfolio-starter` (https://github.com/vercel/nextjs-portfolio-starter)
**Note**: The Plane source code is kept in this repository for reference purposes.
