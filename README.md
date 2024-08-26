# Kubernetes-DVWA-Setup
Overview
This repository provides instructions to deploy the Damn Vulnerable Web Application (DVWA) on a local Kubernetes cluster using Minikube. DVWA is an intentionally vulnerable web application used for testing web application security.

Prerequisites
Minikube - A local Kubernetes cluster.
kubectl - The Kubernetes command-line tool.
Git - To clone the repository.
Setup
1. Install Minikube and kubectl
Follow the installation steps for Minikube and kubectl:

For macOS:

bash
Copy code
# Download and install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube

# Download and install kubectl
curl -LO "https://dl.k8s.io/release/v1.27.1/bin/darwin/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
For other OS: Follow the Minikube installation guide and the kubectl installation guide.

2. Start Minikube
Start the Minikube cluster:

bash
Copy code
minikube start
Deploy DVWA
1. Clone the Repository
Clone the repository containing DVWA:

bash
Copy code
git clone https://github.com/digininja/DVWA.git
cd DVWA
2. Create Kubernetes Manifests
Create Kubernetes deployment and service manifests for DVWA.

dvwa-deployment.yaml

yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: dvwa
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dvwa
  template:
    metadata:
      labels:
        app: dvwa
    spec:
      containers:
      - name: dvwa
        image: vulnerables/web-dvwa
        ports:
        - containerPort: 80
dvwa-service.yaml

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: dvwa
spec:
  type: NodePort
  selector:
    app: dvwa
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30001
3. Apply the Manifests
Deploy DVWA to your Minikube cluster:

bash
Copy code
kubectl apply -f dvwa-deployment.yaml
kubectl apply -f dvwa-service.yaml
4. Verify the Deployment
Check the status of the pods and services:

bash
Copy code
kubectl get pods
kubectl get services
5. Access DVWA
To access the DVWA application, open the following URL in your browser:

plaintext
Copy code
http://localhost:30001
Demonstrate Attack Surfaces
DVWA showcases several vulnerabilities. Use the following URLs to demonstrate each vulnerability:

SQL Injection:

Navigate to: http://localhost:30001/dvwa/vulnerabilities/sqli/

Example Input: 1' OR '1'='1

Cross-Site Scripting (XSS):

Navigate to: http://localhost:30001/dvwa/vulnerabilities/xss_d/

Example Input: <script>alert('XSS')</script>

Command Injection:

Navigate to: http://localhost:30001/dvwa/vulnerabilities/exec/

Example Input: ; ls -la

Cleanup
To delete the DVWA deployment and service:

bash
Copy code
kubectl delete -f dvwa-service.yaml
kubectl delete -f dvwa-deployment.yaml
To stop Minikube:

bash
Copy code
minikube stop
License
This project is licensed under the MIT License - see the LICENSE file for details.

