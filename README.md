# WayBack APK

Welcome to the **WayBack APK Project**. This repository contains the **frontend** and **backend** submodules along with Kubernetes configurations needed to deploy the application on a Kubernetes cluster. This project enables users to search, view, and download historical APK files from a dataset. The files are served through a secure environment hosted on Synology and can be accessed via an Nginx frontend.

## Repository Structure

- **frontend**: React-based user interface for searching and viewing APK details.
- **backend**: FastAPI-based service that provides an API to fetch APK data and manage user authentication.
- **k8s**: Kubernetes configurations for deploying the frontend, backend, and additional resources.

## Prerequisites

Before running the application, make sure you meet the following requirements:

1. **Access to Honeynet Lab VPN**:  
   The backend services like database and APK store require secure access through Honeynet Lab VPN. Please ensure you have VPN access to interact with these resources.

2. **Kubernetes Configuration**:  
   You need to have the **k8s configmap** and **secret** to run the backend. The secrets contain sensitive information such as the database credentials and Token keys.  
   - **Note**: You can request access to these config files from the project administrator if required.

3. **Docker Setup**:  
   Both the frontend and backend need to be containerized into Docker images for deployment on Kubernetes.

   - To build the Docker images:
     - For the backend:
       ```
       docker build -t wayback-backend:latest .
       ```
     - For the frontend:
       ```
       docker build -t wayback-frontend:latest .
       ```

   - After building the images, ensure they are referenced correctly in the Kubernetes deployment YAML files within the `k8s` folder.

4. **Mounting the APKs Volume**:  
   To serve APK files, you must mount the **Synology server** (request access) where the APK files are stored onto your local machine. The mount point should be `/Volumes/apks` on your host machine, as the backend also mounts this directory during deployment.  
   Ensure the same volume is mounted on the backend-deployment in Kubernetes to serve the files properly.
   

## Deployment Instructions

Once the prerequisites are met, deploy the backend, frontend, cache and other resources to your Kubernetes cluster.

```
kubectl apply -f k8s/backend-config.yaml
kubectl apply -f k8s/backend-secret.yaml
kubectl apply -f k8s/redis.yaml
kubectl apply -f k8s/backend.yaml
kubectl apply -f k8s/frontend.yaml
```

## Support

For assistance or to request access to the necessary configuration files, please contact the project admin.