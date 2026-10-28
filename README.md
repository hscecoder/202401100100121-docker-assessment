# Docker & Container Security — Practical Assessment

## Student Details

- **Name:** Himanshu Soni
- **Registration Number (REGNO):** 202401100100121
- **Port (PORT):** 8021
- **Docker Hub Username:** h30s
- **Docker Hub Repository:** https://hub.docker.com/r/h30s/fest-backend
- **GitHub Repository:** https://github.com/hscecoder/202401100100121-docker-assessment

## 1. Objective

To containerise the FestPass three-tier web application using Docker, manage its services using Docker Compose, scan and fix image vulnerabilities using Trivy, publish versioned backend images to Docker Hub, and sign and verify images using Cosign.

## 2. Application Architecture

The application consists of three services:

- **Frontend:** HTML page served by Nginx.
- **Backend:** Python Flask API.
- **Database:** PostgreSQL for persistent registration data.

Docker Compose manages the services. Separate networks restrict direct communication between the frontend and database, while a named volume provides persistent database storage.

## 3. Stage 1 — Containerise the Backend

The backend is containerised using a Dockerfile. Its dependencies are installed, application files are copied into the image, and the Flask application is exposed through the required port.

**Files:** `backend/Dockerfile`, `backend/app.py`, `backend/requirements.txt`, `backend/.dockerignore`

**Verification:** Build and run the backend image, then check the health endpoint and registration number.

**Evidence:** `screenshots/` — Stage 1 screenshots as specified in the assessment.

## 4. Stage 2 — Docker Compose

Docker Compose is used to run the frontend, backend, and PostgreSQL database together.

- Frontend uses `nginx:1.30-alpine`.
- PostgreSQL uses `postgres:17-alpine`.
- Frontend and backend share one network.
- Backend and database share a separate network.
- A named volume stores database data.
- Environment variables are configured without committing secrets to Git.

**Verification:** Open `http://localhost:8021`, add two registrations, restart the Compose stack, and verify that the registrations remain available. Confirm that the frontend cannot directly reach the database.

**Evidence:** Compose configuration, `.env.example`, and the relevant Stage 2 screenshots.

## 5. Stage 3 — Vulnerability Scanning with Trivy

Trivy is used to scan the initial backend image, identify vulnerabilities, and verify the updated image after remediation.

The remediation process includes using a pinned slim or Alpine Python base image, running the application as a non-root user, and upgrading vulnerable dependencies.

Reports are saved as:

- `reports/trivy-before.txt`
- `reports/trivy-after.txt`

### Before-and-After Results

**Evidence:** Trivy reports and Stage 3 screenshots. is in the folder please check!

## 6. Stage 4 — Versioning and Publishing

The fixed backend image is versioned and published to the public Docker Hub repository.

Required release tags:

- `h30s/fest-backend:1.0.0`
- `h30s/fest-backend:1.0`

Both tags must be pushed to Docker Hub. The Compose configuration must then use the published `1.0.0` image rather than building the backend locally.

### Published Image Details

- **Repository:** https://hub.docker.com/r/h30s/fest-backend
- **Release version:** `1.0.0`
- **Image digest:** sha256:c3a07634d81bb24151024b08605f463ed863b334d197b55085e493a43d8bb8a7

**Verification:** Remove the local backend image, start the Compose stack, and verify that the backend image is pulled from Docker Hub and the application displays version `1.0.0`.

**Evidence:** Push output, Docker Hub Tags page, digest, and Stage 4 screenshots.

## 7. Stage 5 — Image Signing and Verification with Cosign

Cosign is used to establish the authenticity of the released backend image.

1. Generate a password-protected Cosign key pair.
2. Add `*.key` to `.gitignore` and commit only `cosign.pub`.
3. Sign version `1.0.0` using its image digest.
4. Verify the signature using `cosign.pub`.
5. Publish version `1.0.1` without signing it and demonstrate that Cosign rejects its verification.

The private key, passwords, access tokens, and environment secrets must never be committed to Git.

**Evidence:** Public key, signature and verification output, unsigned-image rejection, and Stage 5 screenshots.

## 8. AI Tools Used

- **ChatGPT:** Used for understanding assessment requirements, troubleshooting commands, and preparing documentation.
- **Other AI tools:** Declare any additional tools actually used for this assessment and describe their purpose.

All commands and verification steps must be run on the student's own machine.
