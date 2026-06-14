# Kora Analytics API - DevOps Deployment Project

## Overview
This project demonstrates a full DevOps pipeline including containerization, CI/CD automation, and cloud deployment using AWS EC2 and GitHub Actions.

The application is a simple Node.js API with health and metrics endpoints.

---

## Architecture

GitHub → GitHub Actions CI/CD → GitHub Container Registry (GHCR) → AWS EC2 → Docker Container → Public API

---

## Features

- Node.js REST API
- Docker containerization
- GitHub Actions CI/CD pipeline
- Automated Docker image build and push
- Deployment to AWS EC2
- Public HTTP endpoint

---

## API Endpoints

- GET /health → returns system status
- GET /metrics → returns system metrics
- POST /data → echoes request body

---

## Deployment

See DEPLOYMENT.md for full deployment instructions.

---

## Status

✔ Fully deployed and running on AWS EC2  
✔ CI/CD pipeline active  
✔ Public API accessible