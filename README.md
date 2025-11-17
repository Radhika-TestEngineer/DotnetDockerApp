README.md
DotNetDockerApp

A simple .NET 8 Web API project with Unit Tests, Docker support, and CI/CD pipeline (GitHub Actions) including:

Build & Test

Code Coverage (Coverlet)

SonarQube Code Analysis

Docker Image build & push to GHCR

Deployment guidance for Azure / AWS ECS / Kubernetes

📁 Project Structure
DotNetDockerApp/
│── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── src/
│   └── MyApp/
│       ├── Program.cs
│       ├── MyApp.csproj
│       └── Dockerfile
│
└── tests/
    └── MyApp.Tests/
        ├── SampleTests.cs
        └── MyApp.Tests.csproj

🚀 Running Locally
1. Restore & Run
cd src/MyApp
dotnet restore
dotnet run

2. Run Unit Tests
cd tests/MyApp.Tests
dotnet test

🐳 Docker Build & Run
Build image
docker build -t myapp:latest .

Run app
docker run -p 8080:8080 myapp:latest

🌐 Deployment Options
1. Deploy to Azure Web App
Step 1: Push image to GHCR

Already done via CI.

Step 2: Create Azure Web App
az webapp create \
  --resource-group my-rg \
  --plan my-plan \
  --name myapp-web \
  --deployment-container-image-name ghcr.io/USERNAME/myapp:latest

Step 3: Give Azure permission to pull from GHCR
az webapp config container set \
  --resource-group my-rg \
  --name myapp-web \
  --docker-registry-server-url https://ghcr.io \
  --docker-registry-server-user USERNAME \
  --docker-registry-server-password YOUR_PAT


Done. Azure will pull and run your container.

2. Deploy to AWS ECS (Fargate)
Step 1: Push image to GHCR

Already done.

Step 2: Create ECS Cluster

AWS Console → ECS → Clusters → Create Cluster (Fargate)

Step 3: Create Task Definition

Container Image: ghcr.io/USERNAME/myapp:latest

Port: 8080

Step 4: Run service

Deploy the task in the cluster → assign load balancer if needed.

ECS automatically pulls GHCR images if PAT is stored in Secrets Manager.

3. Deploy to Kubernetes (AKS, EKS, Minikube)
Deployment YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: ghcr.io/USERNAME/myapp:latest
        ports:
        - containerPort: 8080

Service YAML
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 8080

Deploy
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
# DotnetDockerApp
