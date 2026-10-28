# Docker & Container Security Assessment
## Himanshu Soni
**Registration Number:** `202401100100121`

### Docker Hub repository	https://hub.docker.com/r/h30s/fest-backend

A containerized FEST application built as part of a Docker and Container Security practical assessment. The project demonstrates how to package an application, run multiple services with Docker Compose, scan container images for vulnerabilities, and work with image signing and verification.

## Overview

The application consists of three services:

- **Frontend:** Nginx serves the FEST web page.
- **Backend:** A Python Flask application provides API endpoints.
- **Database:** PostgreSQL provides the database service with persistent storage.

Docker Compose brings the services together, while separate networks control communication between the application and database services.

## Tech Stack

- Docker and Docker Compose
- Python and Flask
- Nginx
- PostgreSQL
- Trivy for vulnerability scanning
- Cosign for image signing and verification
- Git and GitHub

## Project Structure

```text
202401100100121-docker-assessment/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│   └── .dockerignore
├── frontend/
│   ├── index.html
│   └── nginx.conf
├── reports/
├── screenshots/
├── docker-compose.yml
└── README.md
```

## Getting Started

### Prerequisites

Install Docker Desktop and Git. Make sure Docker Desktop is running before executing the commands below.

### 1. Clone the repository

```bash
git clone https://github.com/hscecoder/202401100100121-docker-assessment.git
cd 202401100100121-docker-assessment
```

### 2. Build and start the application

```bash
docker compose up --build -d
```

### 3. Check running containers

```bash
docker compose ps
```

### 4. Open the frontend

Open the published frontend port shown by `docker compose ps` in your browser. In the current local setup, the frontend has been accessible at:

`http://localhost:8021`

The published port may differ if the Compose configuration changes.

### 5. Test the backend

The backend exposes its API on port `5000` inside the Docker network. To test it locally, first confirm the backend port mapping in `docker compose.yml`. If the backend is not published to the host, test it from inside the Compose network or configure a temporary local port mapping.

Health endpoint:

```text
/api/health
```

Example request, when the backend is available on local port `5000`:

```bash
curl http://localhost:5000/api/health
```

### 6. Stop the application

```bash
docker compose down
```

To remove the Compose-managed database volume as well, use the following command carefully. It deletes the stored database data.

```bash
docker compose down -v
```

## Docker Configuration

The backend is packaged using a Dockerfile that installs Python dependencies and runs the Flask application inside a container.

Docker Compose defines the frontend, backend, and PostgreSQL services. It also configures service dependencies, environment variables, networking, and persistent database storage.

The database uses a named volume so its data can survive container recreation.

## Container Security

### Vulnerability Scanning with Trivy

Trivy is used to scan the backend image for known vulnerabilities.

Example:

```bash
trivy image --severity HIGH,CRITICAL fest-backend:0.1
```

After updating vulnerable dependencies or the base image, rebuild the image and scan it again:

```bash
docker build --no-cache -t fest-backend:0.1 ./backend
trivy image --severity HIGH,CRITICAL fest-backend:0.1
```

Scan results and supporting evidence should be stored in the `reports/` directory.

> A successful scan command does not automatically mean the image is vulnerability-free. Review the final report and document the remaining findings accurately.

### Image Versioning and Docker Hub

The backend image is version-tagged to make releases easier to identify and reproduce.

Docker Hub username: `h30s`

Repository naming format:

```text
h30s/fest-backend:<version>
```

Example commands:

```bash
docker tag fest-backend:0.1 h30s/fest-backend:0.1
docker login
docker push h30s/fest-backend:0.1
```

Use the exact image tag that was successfully built, scanned, and published.

### Image Signing with Cosign

Cosign can be used to sign a published container image and verify its signature.

The signing and verification commands depend on the selected signing method and key configuration. Include the exact commands and successful verification output used during the assessment in the evidence section.

The unsigned-image rejection test should also be documented with its actual output. Do not treat a failed verification as proof of rejection unless the test was performed against the intended unsigned image.

## Evidence and Screenshots

The `screenshots/` directory is intended to contain evidence from the practical assessment.

Recommended evidence:

1. Frontend running in the browser.
2. Backend API health-check response.
3. Docker Compose services running.
4. Configured Docker networks and persistent volume.
5. Initial Trivy vulnerability scan.
6. Updated image and final Trivy scan.
7. Published, version-tagged Docker Hub image.
8. Cosign signing and successful verification.
9. Unsigned-image verification or rejection test.

## Assessment Summary

This project brings together application containerization, multi-service orchestration, persistent storage, image vulnerability scanning, image versioning, and container image signing.

The commands and evidence above are intended to make the setup reproducible and the security workflow easy to review.

## Author

**Registration Number:** `202401100100121`

**GitHub:** [hscecoder](https://github.com/hscecoder)

**Repository:** [202401100100121-docker-assessment](https://github.com/hscecoder/202401100100121-docker-assessment)
