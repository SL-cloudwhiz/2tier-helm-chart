# 🚆 Railway Ticket Booking Application – Helm Chart

This Helm chart deploys the Railway Ticket Booking Application on a Kubernetes cluster (EKS-ready).
It provisions both frontend and backend services and connects securely to an external AWS RDS database using Kubernetes Secrets.

📁 Helm Chart Directory Structure
```
helm-chart/
├── Chart.yaml                      # Helm chart metadata (name, version)
├── values.yaml                     # Configurable values (images, replicas, secrets, etc.)
└── templates/                      # Kubernetes manifest templates
    ├── frontend-deployment.yaml    # Frontend Deployment
    ├── frontend-service.yaml       # Frontend Service
    ├── backend-deployment.yaml     # Backend Deployment
    ├── backend-service.yaml        # Backend Service
    └── db-secret.yaml              # Kubernetes Secret for AWS RDS credentials
```

## AWS RDS Credentials via Kubernetes Secrets

- The AWS RDS database credentials are securely stored as Kubernetes Secrets.
- The backend deployment references these secrets.
- Credentials are injected as environment variables.
- Secret values are defined in values.yaml (base64-encoded).

### Configure values.yaml
Update values.yaml with your environment-specific values:
```
frontend:
  deploymentname: frontend-deployment
  replicas: 1
  image: <YOUR ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/repo-front:1.0.9
  servicename: frontend-service
  type: LoadBalancer

backend:
  deploymentname: backend-deployment
  replicas: 1
  image: <YOUR ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/repo-back:1.0.9
  servicename: backend-service

db:
  secrets:
    name: db-secrets
    username: dmlqYXk=
    password: UGFzc3dvcmQxMjM=
```

📌 Note: Replace image tags and database credentials with your actual values before deployment.

### Installation
Clone the repository

```git clone https://github.com/YOUR-USERNAME/railway-booking-helm-chart.git
cd helm-chart
```

Install the Helm chart
`helm install railway-app ./helm-chart`

Verify installation:
```
helm status railway-app
# or
helm ls
```

Upgrade the release (on changes):
`helm upgrade railway-app ./helm-chart`

Uninstall the chart:
`helm uninstall railway-app`


## 📌 Notes

- Ensure your Kubernetes cluster has network access to the AWS RDS instance (VPC, subnet routing, security groups).
- Images must exist in your container registry (e.g., AWS ECR).
- Base64-encode database credentials before placing them in values.yaml.
