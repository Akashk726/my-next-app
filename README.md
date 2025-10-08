Next.js Application with Docker, GitHub Actions, and Minikube Deployment
Overview This repository contains a containerized Next.js application,
automated build and deployment workflows using GitHub Actions and GitHub
Container Registry (GHCR), and Kubernetes manifests for deployment to
Minikube. The project demonstrates best practices in DevOps, including
Docker optimization, CI/CD automation, and Kubernetes configuration. The
application is a simple Next.js starter template, containerized for
efficient deployment.

Prerequisites Before setting up and running the application, ensure the
following tools are installed on your local machine:

-   Node.js (version 18 or higher)
-   Docker (for building and running containers)
-   Git (for cloning the repository)
-   Minikube (for local Kubernetes deployment)
-   kubectl (Kubernetes command-line tool, typically installed with
    Minikube)
-   A GitHub account with access to GitHub Container Registry (GHCR)

Setup Instructions

**Clone the Repository:**

``` bash
git clone https://github.com/your-username/your-repository-name.git
cd your-repository-name
```

**Install Dependencies:** Navigate to the project root and install the
Next.js dependencies:

``` bash
npm install
```

**Configure Environment Variables (if applicable):** This starter
application does not require custom environment variables. If extending
the application, add them to a .env file and reference them in the
Dockerfile.

------------------------------------------------------------------------

## Local Run Commands

**Running the Application Locally (without Docker):** To run the Next.js
application in development mode:

``` bash
npm run dev
```

Access the application at http://localhost:3000.

To build and run in production mode:

``` bash
npm run build
npm run start
```

Access the application at http://localhost:3000.

------------------------------------------------------------------------

## Building and Running with Docker

**Build the Docker Image:**

``` bash
docker build -t your-username/nextjs-app:latest .
```

**Run the Docker Container:**

``` bash
docker run -p 3000:3000 your-username/nextjs-app:latest
```

Access the application at http://localhost:3000.

The Dockerfile follows best practices, including multi-stage builds for
optimization, using a non-root user, and exposing only necessary ports.

------------------------------------------------------------------------

## GitHub Actions Workflow

The repository includes a GitHub Actions workflow defined in
`.github/workflows/docker-build-push.yml`. This workflow automates the
following on every push to the main branch:

-   Building the Docker image.
-   Tagging the image with the commit SHA and latest.
-   Pushing the image to GitHub Container Registry (GHCR).

To view the workflow:

1.  Navigate to the "Actions" tab in the GitHub repository.
2.  Ensure you have granted the necessary permissions for GHCR in your
    GitHub account settings.

The pushed image can be found at:

    ghcr.io/your-username/your-repository-name:latest

------------------------------------------------------------------------

## Deployment Steps for Minikube

**Start Minikube:**

``` bash
minikube start
```

**Apply Kubernetes Manifests:** Navigate to the `k8s/` directory and
apply the manifests:

``` bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

-   `deployment.yaml`: Defines a Deployment with 3 replicas, readiness
    and liveness probes for health checks, and pulls the image from
    GHCR.
-   `service.yaml`: Defines a NodePort Service to expose the application
    on port 3000.

**Verify Deployment:**

``` bash
kubectl get pods
kubectl get services
```

------------------------------------------------------------------------

## How to Access the Deployed Application

**Get the Minikube IP:**

``` bash
minikube ip
```

**Access via NodePort:** The service exposes the application on a
NodePort (typically 30007 or as specified in service.yaml). Use the
following URL:

    http://<minikube-ip>:<node-port>

Alternatively, use Minikube's built-in command:

``` bash
minikube service nextjs-service
```

This will open the application in your default browser.

------------------------------------------------------------------------

## Troubleshooting

-   **Docker Build Issues:** Ensure Docker is running and you have
    sufficient permissions.
-   **GHCR Push Failures:** Verify GitHub token permissions for
    `packages:write`.
-   **Kubernetes Errors:** Use `kubectl describe pod <pod-name>` for
    detailed logs.
-   **Image Pull Errors:** Ensure the image is publicly accessible or
    provide credentials if private.

------------------------------------------------------------------------

## Additional Notes

This project adheres to the requirements of the DevOps Internship
Assessment, focusing on:

-   Docker optimization (e.g., multi-stage builds, minimal image size)
-   Robust GitHub Actions implementation
-   High-quality Kubernetes configurations (including health checks and
    replicas)
-   Clear documentation

For any questions, refer to the assessment PDF or contact the evaluator.
