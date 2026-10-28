# FestPass Docker & Container Security Assessment

## Student Details

**Registration Number:** 202401100100152  
**Docker Hub Username:** bisht010mayank  
**GitHub Repository:** `https://github.com/mayankbishtmb/202401100100152-docker-assessment`

---

## 1. Project Overview

FestPass is a containerized registration application consisting of a Flask backend, PostgreSQL database, and Nginx frontend.

The project demonstrates Docker containerization, Docker Compose networking, persistent storage, health checks, container security, image vulnerability scanning, and Cosign image signing and verification.

---

## 2. Architecture

The application contains three services:

### Backend
- Flask REST API
- Python Alpine-based Docker image
- Runs on port `5000`
- Provides health and registration APIs

### Database
- PostgreSQL
- Uses `postgres:17-alpine`
- Persistent Docker volume
- Health check using `pg_isready`

### Frontend
- Nginx
- Uses `nginx:1.30-alpine`
- Exposes the application through host port `52`
- Communicates with the backend through the Docker Compose network

---

## 3. Docker Compose Services

The application uses two isolated networks:

- `frontend-backend`
- `backend-db`

The frontend communicates with the backend through `frontend-backend`, while the backend communicates with PostgreSQL through `backend-db`.

The PostgreSQL database uses the persistent volume:

```text
202401100100152-docker-assessment_festpass-data
```

---

## 4. Environment Variables

The backend uses:

```text
REGNO=202401100100152
APP_VERSION=1.0.0
DB_HOST=db
DB_NAME=festpass
DB_USER=festpass
DB_PASSWORD=festpass123
```

The database uses:

```text
POSTGRES_DB=festpass
POSTGRES_USER=festpass
POSTGRES_PASSWORD=festpass123
```

---

## 5. API Endpoints

### Health Check

```text
GET /api/health
```

Example:

```text
http://localhost:52/api/health
```

The response reports the service name, registration number, application version, container ID, and database availability.

### Get Registrations

```text
GET /api/registrations
```

Example:

```text
http://localhost:52/api/registrations
```

### Add Registration

```text
POST /api/registrations
```

Example JSON:

```json
{
  "name": "Mayank",
  "event": "Docker CA2"
}
```

---

## 6. Running the Application

Start all services:

```powershell
docker compose up -d
```

Check running containers:

```powershell
docker compose ps
```

Check the backend health:

```powershell
curl.exe http://localhost:52/api/health
```

Check registrations:

```powershell
curl.exe http://localhost:52/api/registrations
```

Stop the services:

```powershell
docker compose down
```

The PostgreSQL volume is intentionally preserved when using `docker compose down`.

---

## 7. Docker Image

The backend image uses Alpine Linux:

```text
python:3.13-alpine3.22
```

The application image was built and tagged as:

```text
fest-backend:1.0.0
```

Docker Hub image:

```text
bisht010mayank/fest-backend:1.0.0
```

Additional tag:

```text
bisht010mayank/fest-backend:1.0
```

---

## 8. Container Security

The backend image creates a non-root user:

```text
appuser
```

The application runs using this non-root user instead of the root user.

The Docker image also uses Alpine Linux to keep the image lightweight.

---

## 9. Docker Scout / Vulnerability Scanning

Docker image vulnerability scanning was performed on the backend image.

The scan was used to identify vulnerabilities and evaluate the security status of the container image.

---

## 10. Cosign Image Signing

The Docker image `1.0.0` was signed using Cosign.

The image was signed by digest rather than by a mutable tag.

### Signed Image Digest

```text
sha256:731cead1c6d5f06ff31bf880cd100c1c5c6fbee88f6a1ab207dc64f003c29363
```

The public key used for verification is stored as:

```text
cosign.pub
```

The private key:

```text
cosign.key
```

is intentionally excluded from Git using `.gitignore`.

---

## 11. Signature Verification

The signed `1.0.0` image was successfully verified using:

```powershell
cosign verify --key cosign.pub bisht010mayank/fest-backend@sha256:731cead1c6d5f06ff31bf880cd100c1c5c6fbee88f6a1ab207dc64f003c29363
```

The verification confirmed:

- Cosign claims were validated.
- The transparency log was verified.
- The signature matched the specified public key.

---

## 12. Unsigned Image Verification

A separate `1.0.1` image was pushed without a Cosign signature.

Verification using `cosign.pub` correctly failed with no valid signature found.

This demonstrates that the verification process rejects an unsigned image.

---

## 13. Persistence Verification

PostgreSQL data was stored using the Docker volume:

```text
202401100100152-docker-assessment_festpass-data
```

After stopping and restarting the Compose services, previously stored registrations remained available.

This confirms that database persistence was successfully configured.

---

## 14. Evidence

The project contains screenshots demonstrating the required implementation and verification steps.

Important evidence includes:

- Docker image build and execution
- Docker Compose configuration
- Service health
- Database persistence
- Docker security checks
- Cosign signing
- Successful signature verification
- Failed verification of the unsigned `1.0.1` image

---

## 15. Repository

GitHub repository:

```text
https://github.com/mayankbishtmb/202401100100152-docker-assessment
```

Docker Hub username:

```text
bisht010mayank
```
