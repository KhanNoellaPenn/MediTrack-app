# MediTrack — Hospital Patient Records API

## 🚀 Project Overview

MediTrack is a hospital patient records API built with Python and Flask.

This project goes beyond application development and demonstrates a full DevOps lifecycle, transforming a local application into a production ready, cloud deployed system using:

- CI/CD automation (Jenkins)
- Containerization (Docker)
- Kubernetes orchestration (AWS EKS)
- Infrastructure as Code (CloudFormation)
- Monitoring (Prometheus & Grafana)
Hospital: Meridian Health Systems

# Architecture Overview

The system is divided into two main layers:

## Infrastructure Layer (Provisioned Once)
- Jenkins EC2 Server (CI/CD engine)
- AWS VPC, Subnets, Networking
- Amazon EKS Cluster (Kubernetes)
- IAM Roles & Security Groups

### Application Layer (Automated Pipeline)

1. Code pushed to GitHub
2. Webhook triggers Jenkins pipeline
3. Jenkins:
   - Runs tests (pytest)
   - Builds Docker image
   - Pushes image to Docker Hub
4. Kubernetes pulls image and deploys application
5. Service exposes public endpoint
6. Prometheus & Grafana monitor the system

## 🔁 CI/CD Pipeline

The pipeline is fully automated using Jenkins:

- Trigger: GitHub push
- Stages:
  1. Checkout code
  2. Run tests
  3. Build Docker image
  4. Push image to Docker Hub

This ensures every code change is automatically tested and deployed-ready.
## Running Locally

**1. Install dependencies**

```bash
pip install -r requirements.txt
```

**2. Start the server**

```bash
python app.py
```

The API will be available at `http://localhost:5000`.

---

## Running with Docker

**Build the image**

```bash
docker build -t meditrack .
```

**Run the container**

```bash
docker run -p 5000:5000 meditrack
```

The API will be available at `http://localhost:5000`.

---
## Kubernetes Deployment

The application is deployed on AWS EKS using Kubernetes manifests:

- Deployment (3 replicas)
- Service (LoadBalancer)
- ConfigMap (environment variables)

To deploy:

```bash
kubectl apply -f k8s/
## Running Tests

```bash
pytest test_app.py -v
```

---
kubectl get svc meditrack-service

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | HTML status page (app name, hospital, status, patient count) |
| GET | `/health` | JSON health check |
| GET | `/dashboard` | Interactive patient dashboard (HTML) |
| GET | `/patients` | List all patient records |
| GET | `/patients/<id>` | Get a single patient by ID |
| POST | `/patients` | Create a new patient record |
| PUT | `/patients/<id>` | Update an existing patient record |
| DELETE | `/patients/<id>` | Delete a patient record |

---


## Patient Record Schema

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Auto-generated unique identifier |
| `name` | string | Patient full name |
| `age` | integer | Patient age |
| `gender` | string | e.g. `"Male"`, `"Female"`, `"Other"` |
| `diagnosis` | string | Medical diagnosis |
| `ward` | string | Hospital ward name |
| `admitted_on` | string | Admission date (`YYYY-MM-DD`) |

---

## Example Requests

**Create a patient**
```bash
curl -X POST http://localhost:5000/patients \
  -H "Content-Type: application/json" \
  -d '{"name":"Jane Smith","age":38,"gender":"Female","diagnosis":"Migraine","ward":"Neurology","admitted_on":"2026-03-28"}'
```

**Update a patient**
```bash
curl -X PUT http://localhost:5000/patients/1 \
  -H "Content-Type: application/json" \
  -d '{"diagnosis":"Hypertension Stage 2"}'
```

**Delete a patient**
```bash
curl -X DELETE http://localhost:5000/patients/1
```

---
## Monitoring and Observability
Monitoring is implemented using:

- Prometheus (metrics collection)
- Grafana (visual dashboards)

Metrics tracked:
- Pod health
- CPU usage
- Memory usage
- Application availability

Grafana is accessed via port-forwarding from the cluster.
---
## Cleanup

To avoid AWS charges:

- Delete CloudFormation stacks
- Terminate EC2 instances
- Delete EKS cluster

> Note: All data is stored in memory. It resets every time the server restarts. This is intentional for training purposes.
