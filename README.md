# Backend Services: REST API and worker

The project is about a REST API built with Nest.js to handle basic CRUD operations for user data (name, email, id) stored in a MongoDB database that also supports caching the read operations with redis as cache store. Also there is a background worker implemented in Go that logs summary statistics of users periodically. Also this shows concurrency features of Go. Finally we dockerized both apps and deployed using kubernetes.  

## Table of Contents

- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Testing](#testing)

## Project Structure

```
├── docker-compose.yml
├── go-worker/
├── kubernetes/
├── nodejs-api/
└── README.md
```

```
- docker-compose.yml: Docker Compose configuration for local development.
- go-worker/: Go worker project directory.
- kubernetes/: Kubernetes manifests for deployment.
- nodejs-api/: Node.js API project directory.

```

## Prerequisites

Ensure the following tools are installed on your machine:

- Docker
- Node.js and npm
- Go
- Docker Compose
- Minikube
- kubectl

## Getting Started

### Node.js API

1. Install Dependencies:
   
   Navigate to the nodejs-api/ directory and install dependencies.

``` bash
cd nodejs-api
npm install
```

2. Configure Environment:
   
   Copy the .env.example file to .env and update the configuration as needed.

``` bash
cp .env.example .env
```

3. Run the API:
   
   Start the Node.js API.

``` bash
npm run start:dev
```

### Go Worker

1. Build Go Worker:
   
   Navigate to the go-worker/ directory and build the Go worker.

``` bash
cd go-worker
go mod tidy
```

2. Configure Environment:
   
   Copy the .env.example file to .env and update the configuration as needed.

``` bash
cp .env.example .env
```

3. Run Go Worker:
   
   Start the Go worker.

```bash
go run main.go
```


## Testing Locally

1. Node.js API:
   
   Test the Node.js API by making requests to http://localhost:3000/api/users.

``` bash
curl http://localhost:3000/api/users
```

For other endpoints, see the terminal where the api is running

2. Go Worker:
   
   Just watch the log output in the terminal in a regular interval. The default is set to every 5s,

3. Using Docker Compose

   - Build Images: Build Docker images for both the Node.js API and Go worker.
   
``` bash
docker compose build
```
   - Run Containers: Start containers using Docker Compose.

``` bash
docker-compose up
```
   - Access Services: Test your services using the provided endpoints:
   
     - Node.js API: `curl http://localhost:3000/api/users`
     - Go Worker: `docker logs opika-go-worker-1`

## Deployment Using Kubernetes (Minikube)

### Start Minikube:

Start Minikube with the necessary configuration.

``` bash
minikube start
```


### Apply Kubernetes Manifests:

1. Navigate to kubernetes directory

``` bash
cd kubernetes
```

2. Configuration
   
   Update the configmap.yaml file with environment variables

3. Apply the Kubernetes manifests.

``` bash
kubectl apply -f persistent-volume.yaml
kubectl apply -f mongodb-persistent-volume-claim.yaml
kubectl apply -f mongodb-deployment.yaml
kubectl apply -f redis-deployment.yaml
kubectl apply -f configmap.yaml
kubectl apply -f nodejs-api-deployment.yaml
kubectl apply -f go-worker-deployment.yaml
kubectl apply -f nodejs-api-service.yaml
kubectl apply -f go-worker-service.yaml
kubectl apply -f ingress.yaml
```

### Access Services:

Get the Minikube IP.

``` bash
minikube ip
```


Access your services using the Minikube IP and specified ports:

Node.js API: http://<minikube-ip>:30080/api/your-endpoint
Go Worker: http://<minikube-ip>:30081/your-endpoint

### Stop Minikube:

Stop Minikube when done.

``` bash
minikube stop

```


## Troubleshooting

If you encounter issues, refer to the troubleshooting section for specific steps.
- Check logs for both Node.js API and Go worker using kubectl logs.
- Verify the status of pods with kubectl get pods.
- Ensure all necessary ports are accessible locally.

NOTE: edit `/etc/hosts` to add minikube ip
