# Flask CI/CD with Helm

This project demonstrates a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a simple Flask application using Jenkins and Helm. The application is containerized using Docker and deployed to a local Kubernetes cluster (Minikube).

## Project Structure

```
flask-cicd-helm
├── app.py                # Flask application code
├── Dockerfile            # Dockerfile for building the Flask app image
├── Jenkinsfile           # Jenkins pipeline configuration
├── helm-chart            # Helm chart for deploying the application
│   ├── Chart.yaml        # Metadata about the Helm chart
│   ├── values.yaml       # Default configuration values for the Helm chart
│   └── templates         # Kubernetes resource templates
│       ├── deployment.yaml # Deployment resource for the Flask app
│       └── service.yaml    # Service resource for the Flask app
└── README.md             # Project documentation
```

## Prerequisites

- Docker installed on your machine
- Minikube installed and running
- Jenkins installed and configured
- Helm installed

## Setup Instructions

1. **Clone the Repository**
   Clone this repository to your local machine.

   ```
   git clone https://github.com/neilsamaa/flask-cicd-helm
   cd flask-cicd-helm
   ```

2. **Build the Docker Image**
   Use the following command to build the Docker image for the Flask application.

   ```
   docker build -t demo-app .
   ```

3. **Push the Docker Image to DockerHub**
   Tag the image and push it to your DockerHub repository.

   ```
   docker tag demo-app <your-dockerhub-username>/demo-app:latest
   docker push <your-dockerhub-username>/demo-app:latest
   ```

4. **Configure Jenkins**
   - Create a new pipeline job in Jenkins.
   - Add your DockerHub credentials in Jenkins.
   - Configure the pipeline to use the `Jenkinsfile` in this repository.

5. **Run the Jenkins Pipeline**
   Trigger the pipeline in Jenkins. It will:
   - Build the Docker image
   - Push the image to DockerHub
   - Deploy the application to Minikube using Helm
   - Verify the deployment
   - Test Application


## Manual Install Application using Helm
```
helm install demo-app helm-chart
```

## Manual Update Application using Helm
```
helm upgrade demo-app helm-chart
```

## Uninstall Update Application using Helm
```
helm uninstall demo-app
```

## Accessing the Application

Once the deployment is successful, you can access the Flask application by running:

```
kubectl port-forward service/demo-app 5000:5000
```

Then, open your browser and navigate to `http://localhost:5000` to see the "Hello, World!" message.

## Conclusion

This project provides a complete CI/CD pipeline for deploying a Flask application to a Kubernetes cluster using Jenkins and Helm. You can customize the application and the deployment configurations as needed.