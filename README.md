# Kubernetes DVWA Setup

This repository provides a setup for deploying a local Kubernetes cluster with DVWA (Damn Vulnerable Web Application) using Minikube. It also includes instructions for demonstrating common web vulnerabilities.

## Table of Contents

1. [Setup Instructions](#setup-instructions)
    - [Install Minikube](#install-minikube)
    - [Start Minikube](#start-minikube)
    - [Install kubectl](#install-kubectl)
  
![Screenshot (121)](https://github.com/user-attachments/assets/9cbd2957-69ce-4894-8c80-e927f687ca91)

      
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

![Screenshot (122)](https://github.com/user-attachments/assets/f2ff50ab-727f-43d8-a004-260bdd3c8ea8)

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


![Screenshot (123)](https://github.com/user-attachments/assets/7d2e7e80-9ec0-4b1f-afb6-e9d8007b9814)



### Demonstrate Attack Vectors
SQL Injection
Attack Vector: SQL Injection

Steps:
Open the DVWA login page in your browser (provided by Minikube service command).
In the login form, enter the following payload in the username field: ' OR 1=1 --.
Leave the password field empty.
Click "Login."
Observation: The payload bypasses authentication, indicating an SQL Injection vulnerability.

![Screenshot (125)](https://github.com/user-attachments/assets/c45e8ef8-9bab-4c76-bda0-ef41b09065e9)


Cross-Site Scripting (XSS)
Attack Vector: Cross-Site Scripting (XSS)

Steps:
Navigate to the “XSS (Stored)” section in DVWA.
In the message field, enter the following payload: <script>alert('XSS')</script>.

![Screenshot (129)](https://github.com/user-attachments/assets/5451766e-3cc1-4f71-85c6-d28a1b35eb54)


Submit the form.
Observation: An alert box pops up when the page is loaded, indicating an XSS vulnerability.

Command Injection
Attack Vector: Command Injection

Steps:
Navigate to the “Command Injection” section in DVWA.
In the input field, enter the following payload: ; ls -la.

![Screenshot (128)](https://github.com/user-attachments/assets/16516cf3-a4a8-4865-a6eb-3a94c6e521e9)


Submit the form.
Observation: The directory listing of the server is displayed, indicating a command injection vulnerability.



### 📄 License

### MIT License

Copyright (c) 2024 [Kubernetes DVWA Setup]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:



