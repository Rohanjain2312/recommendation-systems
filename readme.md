# ML Recommendation System with MLOps and Microservices

A modular, scalable machine learning recommendation system built with FastAPI microservices, Docker containers, Kubernetes orchestration, AWS S3 for model storage, and ClearML for experiment tracking.

## Overview

This project demonstrates the design, deployment, and management of a real-time ML recommendation system using MLOps principles. It includes:

- Collaborative Filtering
- Hybrid Recommendations
- Popularity-Based Recommendations
- Input Handling Service
- Automated Model Update Service

Each service is containerized and deployed as a microservice within a Kubernetes cluster.

## Features

- FastAPI-based APIs for each recommendation module
- Asynchronous, modular architecture
- Automated model retraining via Kubernetes CronJob
- Experiment tracking using ClearML
- Cloud storage and model persistence via AWS S3
- Kubernetes deployment-ready YAML files
- Postman-compatible API testing
- CI/CD pipeline support (planned for future)

## System Architecture

Each pod is responsible for a specific service and interacts with S3 to load or update models. The model update job runs on a scheduled CronJob and logs metrics via ClearML.

## Tech Stack

| Component             | Tool/Framework         |
|----------------------|------------------------|
| Web Framework        | FastAPI                |
| Containerization     | Docker                 |
| Orchestration        | Kubernetes (Minikube)  |
| Storage              | AWS S3                 |
| Experiment Tracking  | ClearML                |
| Automation           | Kubernetes CronJob     |
| Development          | Python, Jupyter        |

## Recommendation Modules

### Collaborative Filtering

Uses matrix factorization techniques (like ALS or SVD) to learn latent user and item features based on past user-item interactions. This approach helps personalize recommendations by identifying similar behavior patterns across users.

### Hybrid Recommendations

Combines collaborative filtering with content-based features like category, brand, and item metadata. This balances personalization with coverage and is especially helpful in cold-start scenarios.

### Popularity-Based Recommendations

Provides fallback recommendations for new or anonymous users by ranking items globally based on popularity (e.g., top-selling products). This module ensures a response is always available even without user history.


### Folder Structure

### Root Directory
- **`.gitignore`**: Specifies files and directories to be ignored by Git.
- **`data.ipynb`**: A Jupyter notebook for data exploration or preprocessing.
- **`model_query.py`**: A Python script for querying models.
- **`readme.md`**: This file, providing an overview of the project.
- **`Rohan_v2.py`**: A Python script, likely related to a specific functionality or experiment.
- **`s3_data.py`**: A script for interacting with AWS S3.
- **`setup.py`**: A setup script for packaging the project.

---

### `colab/`
Contains files related to collaborative filtering models.
- **`api_query_example.py`**: Example script for querying the API.
- **`app/`**: Contains the FastAPI application code for collaborative filtering.
- **`build.sh`**: A script to build and run the Docker container for the collaborative filtering app.
- **`collab_model.ipynb`**: Jupyter notebook for training or analyzing the collaborative filtering model.
- **`collab_model_retrained.ipynb`**: Jupyter notebook for retraining the collaborative filtering model.
- **`Dockerfile`**: Dockerfile for building the collaborative filtering app container.
- **`fake.py`**: A placeholder or utility script.
- **`k8s/`**: Kubernetes configuration files for deploying the collaborative filtering app.
- **`requirements.txt`**: Python dependencies for the collaborative filtering app.

---

### `hybrid/`
Contains files for hybrid recommendation models.
- **`app/`**: Contains the FastAPI application code for hybrid models.
- **`Dockerfile`**: Dockerfile for building the hybrid model app container.
- **`requirements.txt`**: Python dependencies for the hybrid model app.

---

### `input/`
Contains files for input processing.
- **`app/`**: Contains the FastAPI application code for input processing.
- **`Dockerfile`**: Dockerfile for building the input processing app container.
- **`requirements.txt`**: Python dependencies for the input processing app.

---

### `popularity/`
Contains files for popularity-based recommendation models.
- **`app/`**: Contains the FastAPI application code for popularity-based recommendations.
- **`Dockerfile`**: Dockerfile for building the popularity-based app container.
- **`requirements.txt`**: Python dependencies for the popularity-based app.

---

### `update_model/`
Contains files for updating models.
- **`app/`**: Contains the application code for model updates.
- **`build.sh`**: A script to build and run the Docker container for the model update app.
- **`Dockerfile`**: Dockerfile for building the model update app container.

---

### `k8s/`
Contains Kubernetes deployment configurations.
- **`colab-deployment.yaml`**: Deployment configuration for the collaborative filtering app.
- **`hybrid-deployment.yaml`**: Deployment configuration for the hybrid model app.
- **`input-deployment.yaml`**: Deployment configuration for the input processing app.
- **`popularity-deployment.yaml`**: Deployment configuration for the popularity-based app.
- **`update-deployment.yaml`**: Deployment configuration for the model update app.


## How to Use

### 1. Build and Run Docker Containers

For services that include a `build.sh` script:

```bash
cd colab/
./build.sh
```

Or build and run manually using Docker:

```bash
docker build -t service-name .
docker run -p 8000:8000 service-name
```

> Replace `service-name` with the appropriate folder (e.g., `colab`, `hybrid`, etc.)

---

### 2. Deploy to Kubernetes

Make sure your Kubernetes cluster (e.g., Minikube) is running. Then apply the deployment YAML files:

```bash
kubectl apply -f k8s/colab-deployment.yaml
kubectl apply -f k8s/hybrid-deployment.yaml
kubectl apply -f k8s/input-deployment.yaml
kubectl apply -f k8s/popularity-deployment.yaml
kubectl apply -f k8s/update-deployment.yaml
```

Each service will be exposed within the cluster and available for internal routing or testing via tools like Postman.

---

### 3. Install Python Dependencies

Each microservice has a `requirements.txt` file. To install dependencies:

```bash
pip install -r requirements.txt
```

> You may want to use a virtual environment for dependency isolation.

---

### 4. Access FastAPI Swagger Docs

After running any service (e.g., on port 8000), open:

```
http://localhost:8000/docs
```

to view and test the API endpoints interactively.

---

### 5. Trigger Model Retraining (Optional)

Model retraining is scheduled via Kubernetes CronJobs, but you can manually trigger it by re-applying the job file or running:

```bash
kubectl create job --from=cronjob/model-update-job model-update-job-manual
```

This will:
- Fetch new data
- Retrain the collaborative filtering model
- Upload the model to S3
- Log the run in ClearML

## Evaluation

- Fast API response time: ~186ms average latency
- Improved category diversity and recommendation quality post-retraining
- Reliable performance thanks to container orchestration and automation

## Limitations & Future Scope

- Cold-start problem for new users remains a challenge
- No frontend interface yet — future work includes web-based UI
- CI/CD integration (e.g., GitHub Actions) planned for automation
- Load testing under high concurrency is yet to be done

## Team

- Rohan Jain   
- Hsing-Hao Wang  
- Madhur Lakshmanan  
- Joshua Liu