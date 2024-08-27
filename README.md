# Kubernetes DVWA Setup

This repository provides a setup for deploying a local Kubernetes cluster with DVWA (Damn Vulnerable Web Application) using Minikube. It also includes instructions for demonstrating common web vulnerabilities.

## Table of Contents

1. [Setup Instructions](#setup-instructions)
    - [Install Minikube](#install-minikube)
    - [Start Minikube](#start-minikube)
    - [Install kubectl](#install-kubectl)
  ![Uploading Screenshot (121).png…]()

      
2. [Deploy DVWA](#deploy-dvwa)
    - [Create Deployment and Service Files](#create-deployment-and-service-files)
    - [Deploy DVWA](#deploy-dvwa)
    - [Access DVWA](#access-dvwa)
3. [Demonstrate Attack Vectors](#demonstrate-attack-vectors)
    - [SQL Injection](#sql-injection)
    - [Cross-Site Scripting (XSS)](#cross-site-scripting-xss)
    - [Command Injection](#command-injection)
4. [Cleanup](#cleanup)

## Setup Instructions

### Install Minikube

To install Minikube, run the following script:

bash setup-scripts/install-minikube.sh

Start Minikube-Start Minikube to create a local Kubernetes cluster: 
bash setup-scripts/start-minikube.sh

Install kubectl-To install kubectl, run the following script:
bash setup-scripts/install-kubectl.sh

Verify the installation with:
kubectl version --client

### Deploy DVWA
Create Deployment and Service Files
Create a file named dvwa-deployment.yaml with the following content:
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

Deploy DVWA
Apply the deployment with:
kubectl apply -f dvwa-deployment.yaml

Access DVWA
To access DVWA, use the Minikube service command:
minikube service dvwa-service
This will open a browser window with the DVWA login page.

### Demonstrate Attack Vectors
SQL Injection
Attack Vector: SQL Injection

Steps:
Open the DVWA login page in your browser (provided by Minikube service command).
In the login form, enter the following payload in the username field: ' OR 1=1 --.
Leave the password field empty.
Click "Login."
Observation: The payload bypasses authentication, indicating an SQL Injection vulnerability.

Cross-Site Scripting (XSS)
Attack Vector: Cross-Site Scripting (XSS)

Steps:
Navigate to the “XSS (Stored)” section in DVWA.
In the message field, enter the following payload: <script>alert('XSS')</script>.
Submit the form.
Observation: An alert box pops up when the page is loaded, indicating an XSS vulnerability.

Command Injection
Attack Vector: Command Injection

Steps:
Navigate to the “Command Injection” section in DVWA.
In the input field, enter the following payload: ; ls -la.
Submit the form.
Observation: The directory listing of the server is displayed, indicating a command injection vulnerability.



### 📄 License

### MIT License

Copyright (c) 2024 [Kubernetes DVWA Setup]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:



