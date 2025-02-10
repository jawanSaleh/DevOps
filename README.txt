MyApp Deployment - README

Project Overview

This project is a simple web application deployed using Docker and Kubernetes. The application consists of an Nginx server serving a static HTML page containing my name and a short motivation statement. The deployment is managed using Kubernetes with a Deployment and a Service for exposing the application.

Project Structure

├── Dockerfile          # Docker configuration for building the image
├── index.html         # Simple HTML file with personal details and motivation
├── deployment.yaml    # Kubernetes Deployment configuration
├── service.yaml       # Kubernetes Service configuration
└── README.md          # Project documentation

Docker Setup

The project uses an official Nginx base image and serves the static index.html file.

Dockerfile

# Use the official Nginx base image
FROM nginx:alpine

# Set the working directory
WORKDIR /usr/share/nginx/html

# Copy the HTML file into the container
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]




Kubernetes Deployment
The application is deployed on Kubernetes using a Deployment and a Service.

Deployment Configuration (deployment.yaml)

apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3  # Number of running pods
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: jawansaleh/secondrepo:latest
        ports:
        - containerPort: 80
        
        
        
        
        
Service Configuration (service.yaml)

apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
      
      
      
 Steps to Deploy the Project
 
 1. Build and Push the Docker Image
 
 docker build -t jawansaleh/secondrepo:latest .
 
 2. Push the Image to Docker Hub
 
 While pushing the image, I faced an issue with Docker login permissions. I resolved it using the following command:
 
 docker login -u username -p password docker.io
 
 After successful login, I pushed the image:
 
 docker push jawansaleh/secondrepo:latest
 
 3. Apply Kubernetes Configurations
 
 Deploy the application using the following commands:
 
 kubectl apply -f deployment.yaml
kubectl apply -f service.yaml


Test the service:
 
minikube start

kubectl get service myapp-service

minikube service myapp-service --url 

After getting the url, you can visit it using your brwoser
