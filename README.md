# Coworking Analytics Service

This repository contains a Flask analytics API plus Docker, Kubernetes, and AWS CodeBuild assets used to build, publish, and deploy the service. The app reads database settings from environment variables and exposes health and reporting endpoints.

## Local Run

Create a Python virtual environment, install analytics/requirements.txt, set DB_USERNAME, DB_PASSWORD, DB_HOST, DB_PORT, and DB_NAME, then run app.py. Verify with /health_check, /readiness_check, /api/reports/daily_usage, and /api/reports/user_visits.

## Container Image

Use analytics/Dockerfile to build an image and run it with the same DB environment variables, use host.docker.internal as DB_HOST when the database is on the host.

## CI/CD with CodeBuild and ECR

CodeBuild builds the Docker image, tags it with the build number, and pushes to the coworking-ecr repository in ECR. The buildspec.yaml at repo root logs in to Docker Hub and ECR, then runs docker build, docker tag, and docker push.

## Kubernetes Deployment

deployment/configmap.yaml defines DB_NAME, DB_USERNAME, DB_HOST, and DB_PORT, while deployment/secret provides DB_PASSWORD. deployment/coworking.yaml wires the ConfigMap and Secret into the app container and exposes port 5153 via a LoadBalancer Service.

## Operations Notes

DNS for the LoadBalancer can take a few minutes to propagate. Use kubectl get svc to find the external hostname, then curl the endpoints to verify the app..
