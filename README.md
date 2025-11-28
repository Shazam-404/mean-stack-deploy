# MEAN Stack CRUD Application - DevOps Task

This repository contains a full-stack MEAN (MongoDB, Express, Angular, Node.js) application that has been containerized and automated for cloud deployment. This project was completed as part of the selection process for the DevOps Engineer Intern role.

## Infrastructure & Architecture
The application is designed as a microservices-style architecture running on **Google Cloud Platform (GCP)**. It consists of three main containers orchestrated via Docker Compose:

1.  **Frontend (Angular + Nginx):**
    * The Angular application is built using a multi-stage Docker build to keep the image size optimized.
    * **Nginx Configuration:** Nginx is used as the web server to serve the static Angular files. Crucially, it also acts as a **Reverse Proxy**. It listens on Port 80 and forwards any request starting with `/api/` to the backend container on port 8080. This eliminates CORS issues and creates a unified entry point for the application.
2.  **Backend (Node.js + Express):**
    * Exposes REST APIs on port 8080 to handle CRUD operations.
    * Connects to the database using the internal Docker hostname `mongo`.
3.  **Database (MongoDB):**
    * A dedicated MongoDB container with a persistent volume to ensure data safety across restarts.

## Step-by-Step Setup & Deployment

### 1. Running Locally
I have configured the project so it can be run instantly on any machine with Docker, without needing to install Node.js or MongoDB locally.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/Shazam-404/mean-stack-deploy.git]
    cd mean-devops-task
    ```
2.  **Start the application:**
    ```bash
    docker-compose up -d
    ```
3.  **Access the App:**
    Open your browser and navigate to `http://localhost`.

### 2. CI/CD Pipeline (GitHub Actions)
I implemented a complete CI/CD pipeline using **GitHub Actions** to automate the deployment process.
* **Trigger:** Pushing to the `main` branch triggers the pipeline.
* **Build Phase:** The workflow builds new Docker images for the frontend and backend.
* **Push Phase:** Images are tagged and pushed to **Docker Hub**.
* **Deploy Phase:** The action connects to the GCP Virtual Machine via SSH, pulls the latest images, and restarts the containers using Docker Compose.

##  Screenshots

### 1. Application UI
*The application running live on the GCP VM (Port 80).*
![Application UI](https://github.com/user-attachments/assets/96e4bd1c-da9f-4b7d-8f20-fd4911e3fb64)

### 2. CI/CD Pipeline Success
*GitHub Actions workflow successfully building and deploying the app.*
![CI/CD Pipeline](https://github.com/user-attachments/assets/8e81fc7c-bb13-422b-8c0e-f5a10923fa7c)

### 3. Docker Hub Repository
*Container images successfully pushed to the registry.*
![Docker Hub](https://github.com/user-attachments/assets/ac5fe421-b7ce-478f-a724-eae8deff4a91)

### 4. Nginx & Infrastructure Status
*Terminal output (`docker ps`) showing the Nginx frontend, Backend
![Nginx_ps](https://github.com/user-attachments/assets/9a3f8d86-605c-44de-9bef-38217a3d250f)
