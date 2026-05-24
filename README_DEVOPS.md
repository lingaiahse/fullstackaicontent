# Complete DevOps Guide for Azure, Kubernetes, Splunk, Grafana, Docker, GitHub Actions, and JFrog

## Who this guide is for
This guide is for beginners who want to learn how modern DevOps practices work in real cloud applications.
It explains the tools, concepts, and processes in an approachable way.

## What you'll learn
- What DevOps means and why it matters
- How infrastructure, code, and automation fit together
- Key DevOps tools and cloud services
- A sample deployment architecture for modern apps


## 1. Real-Time Example Application

### 1.1 Application Overview
The example application consists of:
- **React frontend**: single-page application served via static hosting or NGINX container.
- **Node.js backend**: Express or Fastify API for business logic and data access.
- **Database**: PostgreSQL or MongoDB for persistent storage.
- **Authentication**: OAuth/OIDC with Azure AD or another identity provider.
- **APIs**: REST endpoints for user data, authentication, and application features.

### 1.2 Deployment Architecture
- **GitHub repository** hosts frontend and backend code.
- **Docker** builds images for frontend and backend.
- **JFrog Artifactory** stores container and package artifacts.
- **Azure Kubernetes Service (AKS)** runs containers in dev, staging, and prod clusters.
- **Azure Container Registry (ACR)** may provide private image storage.
- **Azure DevOps/GitHub Actions** manages CI/CD workflows.
- **Grafana** visualizes metrics and dashboards.
- **Splunk** collects logs and events for troubleshooting.

---

## 2. Azure Platform Components

### 2.1 Azure Kubernetes Service (AKS)
AKS is the managed Kubernetes service for container workloads.

Real-time usage:
- Deploy React and Node.js containers to AKS.
- Create separate clusters or namespaces for dev, staging, and prod.
- Use Horizontal Pod Autoscaler (HPA) for scaling.
- Integrate with Azure Monitor for container insights.

Key Azure services:
- **Azure Container Registry (ACR)**: private image registry.
- **Azure Key Vault**: secrets, certificates, and keys.
- **Azure Application Gateway / Azure Front Door**: traffic routing and WAF.
- **Azure Monitor**: metrics and logs for AKS and Azure services.
- **Azure SQL / Cosmos DB**: managed databases for stateful backend storage.

### 2.2 Azure Container Registry
ACR stores Docker images securely and integrates with AKS.

Real-time usage:
- Push frontend and backend images from CI.
- Use image tagging for environment promotion: `app:dev`, `app:staging`, `app:prod`.
- Enable content trust and vulnerability scanning.

### 2.3 Azure Key Vault
Secure secrets for application credentials, database credentials, and certificates.

Real-time usage:
- Store DB connection strings, OAuth client secrets, JWT signing keys.
- Grant AKS-managed identities access to secrets.
- Use Key Vault references in Kubernetes secrets or pipelines.

### 2.4 Azure Load Balancing and DNS
Use Application Gateway, Front Door, or NGINX ingress.

Real-time usage:
- Route traffic to React frontend and backend APIs.
- Use TLS certificates from Key Vault.
- Implement path-based routing, host-based routing, and WAF protection.

---

## 3. Docker Containerization

### 3.1 Docker for Frontend and Backend
Create Docker images for React and Node.js apps.

Example React Dockerfile:
```dockerfile
FROM node:20-alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
```

Example Node.js Dockerfile:
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

### 3.2 Build and Tag Images
- Build images locally or in CI:
  - `docker build -t myapp-frontend:dev ./frontend`
  - `docker build -t myapp-backend:dev ./backend`
- Tag for registry:
  - `docker tag myapp-backend:dev <registry>/myapp-backend:dev`
  - `docker push <registry>/myapp-backend:dev`

### 3.3 Docker Compose for Local Dev
Use `docker-compose.yml` to run the frontend, backend, and database together locally.

Example snippet:
```yaml
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - '3000:80'
  backend:
    build: ./backend
    ports:
      - '3001:3000'
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/app
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: app
```

---

## 4. Kubernetes Deployment

### 4.1 Kubernetes Manifests
Create manifests for deployments, services, ingress, and config.

Example backend deployment:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: <acr>/myapp-backend:dev
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secrets
              key: database_url
```

Example ingress:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - host: app.dev.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: backend
            port:
              number: 3000
```

### 4.2 Kubernetes Secrets and ConfigMaps
Store config and secrets separately.

- ConfigMaps for non-sensitive settings.
- Secrets for database credentials and OAuth secrets.

Example secret:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secrets
type: Opaque
stringData:
  database_url: 'postgres://user:pass@db:5432/app'
```

### 4.3 Autoscaling and Resource Management
Use resource requests and limits, plus auto scaling.

Example HPA:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 2
  maxReplicas: 6
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
```

### 4.4 Azure AKS Integration
- Provision AKS with Azure CLI or Terraform.
- Connect AKS with ACR using `az aks update -n <cluster> -g <rg> --attach-acr <acrName>`.
- Use Azure AD integration for cluster authentication.
- Enable monitoring with Azure Monitor for containers.

---

## 5. GitHub Actions CI/CD

### 5.1 Repository Structure
Use a mono-repo or separate repos for frontend and backend.
Suggested structure:
```
.github/workflows/
frontend/
backend/
Dockerfile.frontend
Dockerfile.backend
k8s/
  dev/
  staging/
  prod/
```

### 5.2 Multi-Environment Pipeline Design
- `dev`: build, test, publish dev artifacts, deploy to dev cluster.
- `staging`: build, integration tests, publish staging artifacts, deploy to staging.
- `prod`: approval gates, publish prod artifacts, deploy to production.
- Use branch protection and environment approvals.

### 5.3 Example GitHub Actions Workflow
```yaml
name: CI-CD
on:
  push:
    branches:
      - main
      - staging
      - dev
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Set up Node
      uses: actions/setup-node@v4
      with:
        node-version: '20'

    - name: Install frontend dependencies
      run: |
        cd frontend
        npm install

    - name: Build frontend
      run: |
        cd frontend
        npm run build

    - name: Build backend dependencies
      run: |
        cd backend
        npm install

    - name: Lint backend
      run: |
        cd backend
        npm run lint

    - name: Run backend tests
      run: |
        cd backend
        npm test

  docker:
    runs-on: ubuntu-latest
    needs: build
    steps:
    - uses: actions/checkout@v4

    - name: Log in to ACR
      uses: azure/docker-login@v1
      with:
        login-server: ${{ secrets.ACR_LOGIN_SERVER }}
        username: ${{ secrets.ACR_USERNAME }}
        password: ${{ secrets.ACR_PASSWORD }}

    - name: Build and push frontend image
      run: |
        docker build -f frontend/Dockerfile -t ${{ secrets.ACR_LOGIN_SERVER }}/myapp-frontend:${{ github.sha }} ./frontend
        docker push ${{ secrets.ACR_LOGIN_SERVER }}/myapp-frontend:${{ github.sha }}

    - name: Build and push backend image
      run: |
        docker build -f backend/Dockerfile -t ${{ secrets.ACR_LOGIN_SERVER }}/myapp-backend:${{ github.sha }} ./backend
        docker push ${{ secrets.ACR_LOGIN_SERVER }}/myapp-backend:${{ github.sha }}

  deploy-dev:
    runs-on: ubuntu-latest
    needs: docker
    if: github.ref == 'refs/heads/dev'
    steps:
    - uses: actions/checkout@v4
    - name: Deploy to AKS dev
      uses: azure/aks-deploy@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        resource-group: ${{ secrets.AKS_DEV_RG }}
        cluster-name: ${{ secrets.AKS_DEV_CLUSTER }}
        namespace: dev
        manifests: k8s/dev

  deploy-staging:
    runs-on: ubuntu-latest
    needs: docker
    if: github.ref == 'refs/heads/staging'
    steps:
    - uses: actions/checkout@v4
    - name: Deploy to AKS staging
      uses: azure/aks-deploy@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        resource-group: ${{ secrets.AKS_STAGING_RG }}
        cluster-name: ${{ secrets.AKS_STAGING_CLUSTER }}
        namespace: staging
        manifests: k8s/staging

  deploy-prod:
    runs-on: ubuntu-latest
    needs: docker
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
    - uses: actions/checkout@v4
    - name: Deploy to AKS prod
      uses: azure/aks-deploy@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        resource-group: ${{ secrets.AKS_PROD_RG }}
        cluster-name: ${{ secrets.AKS_PROD_CLUSTER }}
        namespace: prod
        manifests: k8s/prod
```

### 5.4 Artifact Management with JFrog
- Store build artifacts and container images in JFrog Artifactory.
- Use GitHub Actions to publish packages and images.
- Use repository layout to separate `dev`, `staging`, and `prod` artifacts.
- Use Artifactory as a Docker registry and generic package repository.

Example publish step:
```yaml
- name: Publish Docker image to JFrog
  run: |
    docker login ${{ secrets.JFROG_REGISTRY }} -u ${{ secrets.JFROG_USER }} -p ${{ secrets.JFROG_API_KEY }}
    docker tag myapp-backend:${{ github.sha }} ${{ secrets.JFROG_REGISTRY }}/myapp-backend:${{ github.sha }}
    docker push ${{ secrets.JFROG_REGISTRY }}/myapp-backend:${{ github.sha }}
```

---

## 6. Monitoring and Logging

### 6.1 Grafana for Metrics
Grafana visualizes metrics from Kubernetes, Prometheus, and Azure Monitor.

Real-time usage:
- Create dashboards for frontend uptime, backend latency, error rates, pod CPU/memory usage.
- Use Prometheus as a metrics source and Grafana for alerting.
- Create environment-specific dashboards for dev, staging, and prod.

### 6.2 Splunk for Logging
Splunk collects logs from applications and Kubernetes.

Real-time usage:
- Forward container logs from Fluentd/Fluent Bit or Azure Monitor to Splunk.
- Search logs for errors, traces, and authentication events.
- Build Splunk dashboards for security, operations, and business metrics.
- Create alerts for high error rates or suspicious behavior.

### 6.3 End-to-End Observability
- Use distributed tracing with OpenTelemetry or Jaeger.
- Correlate metrics, logs, and traces in Grafana and Splunk.
- Monitor deployments, rollbacks, and environment health.
- Track release metrics like deployment frequency and MTTR.

---

## 7. Security, Testing, and QA

### 7.1 Security Best Practices
- Use Azure AD and role-based access control for Azure resources.
- Use RBAC and Kubernetes Roles/RoleBindings for cluster access.
- Store secrets in Azure Key Vault and JFrog secrets.
- Scan container images with a registry scanner or GitHub Actions security checks.
- Use network policies to restrict pod communication.
- Enable least-privilege access for all service identities.

### 7.2 Testing Strategy
- **Unit tests**: React components and backend business logic.
- **Integration tests**: API contract tests and database integration.
- **End-to-end tests**: Cypress or Playwright for user flows.
- **Smoke tests**: run after deployment to each environment.
- **Security tests**: static code analysis, dependency scanning, container vulnerability scanning.

### 7.3 QA and Release Management
- Create release branches for staging and production.
- Use feature flags to enable/disable new functionality.
- Require approvals for promotion to staging and prod.
- Run QA test plans in staging before production deployment.
- Use rollback strategies with Helm, Kubernetes rollouts, or old image tags.

---

## 8. Infrastructure Management

### 8.1 Terraform for Azure and AWS
- Use Terraform to provision AKS, ACR, App Gateway, Key Vault, and network resources.
- Use separate state files or workspaces per environment.
- Use modules for reusable VPC/VNet, AKS/EKS, and database patterns.
- Store Terraform state remotely in Azure Storage, AWS S3, or Terraform Cloud.

### 8.2 Kubernetes Environment Isolation
- Use namespaces for dev, staging, and prod within each cluster.
- Use separate clusters when security or compliance requires stricter isolation.
- Label resources by environment and application.

### 8.3 GitOps Principles
- Store Kubernetes manifests and Helm charts in Git.
- Use GitHub Actions or Argo CD to synchronize cluster state from Git.
- Treat Git as the source of truth for infrastructure and application configuration.

---

## 9. Real-Time Workflow

### 9.1 Development Flow
1. Developer creates a feature branch.
2. Runs local tests with Docker Compose and unit tests.
3. Opens a pull request.
4. CI builds, tests, and publishes artifacts to JFrog.
5. PR triggers review, security scanning, and integration validation.

### 9.2 Deployment Flow
1. `dev` branch deploys automatically to dev environment.
2. `staging` branch triggers staging deployment after successful build.
3. `main` branch requires approval and deploys to prod.
4. Use feature flags and canary deployments to reduce risk.
5. Monitor metrics and logs in Grafana and Splunk.

### 9.3 Incident and Rollback Flow
1. Alert triggers from Grafana or Splunk.
2. Investigate the issue using logs, traces, and dashboards.
3. Roll back to the previous Docker image or Helm release.
4. Fix the root cause and create a patch release.
5. Validate the fix in dev/staging before redeploying.

---

## 10. Interview Preparation Topics

- Understand containerization with Docker and why it matters.
- Know Kubernetes concepts: pods, deployments, services, ingress, HPA, secrets.
- Explain AKS and how it integrates with Azure services.
- Describe GitHub Actions workflows and multi-environment CI/CD.
- Know artifact management with JFrog and image versioning strategies.
- Understand monitoring with Grafana and logging with Splunk.
- Be able to explain security controls for cloud-native DevOps.
- Discuss testing, QA, and release management best practices.

---

## 11. Practical Advice

- Build a real application and deploy it through the entire pipeline.
- Use infrastructure-as-code to standardize provisioning.
- Store secrets securely and avoid hardcoded credentials.
- Automate tests and quality checks in CI.
- Monitor every environment and act on alerts quickly.
- Keep documentation updated with architecture and process changes.

---

## 12. Closing Notes

This guide is designed to help you build a complete DevOps workflow for modern applications using Azure, Kubernetes, Splunk, Grafana, Docker, GitHub Actions, and JFrog.
Use it as a blueprint for real-time production systems, secure deployments, and end-to-end operations.
