# jboss-app

# JBoss on Kubernetes with Custom HTTP and HTTPS Ports

This project deploys a JBoss application server in a Kubernetes environment with customized HTTP and HTTPS ports. The default ports for JBoss HTTP (8080) and HTTPS (8443) have been modified to HTTP (30555) and HTTPS (30777) for this deployment.

## Prerequisites

- A running Kubernetes cluster (Minikube, GKE, AKS, EKS, or any other Kubernetes service)
- Docker or podman installed locally for building the JBoss image (optional)
- kubectl configured to interact with your Kubernetes cluster
- JBoss/Wildfly Docker image (used in this project)

## Project Structure

The project contains the following main files:
- `jboss-deployment.yaml`: Kubernetes deployment file for the JBoss application which contains also the service and secret required  .
- `Dockerfile` (optional): A Dockerfile to build a custom JBoss image if needed.

## Steps to Deploy JBoss with Custom Ports

## Prerequisites

install minikube you can check the installation details from this link 
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download

1. Clone the Repository
```bash

git clone https://github.com/islamaboalkhair/jboss-app.git
cd jboss-k8s-deployment

. Build the Custom JBoss Docker Image (Optional)

If you want to use a custom JBoss image with specific configurations, Dockerfile already shared you can customize the admin console user there 
build the image:

for docker 
docker build -t custom-jboss:latest .
for podman 
podman build --taq=jboss/wildfly-admin .

2. you can create your own keystore to use it

keytool -genkeypair -alias server -keyalg RSA -keystore application.keystore -validity 365 -keysize 2048

3- encoded the security

base64 application.keystore  | awk '{printf "%s", $0}' > encoded_keystore_no_newlines.txt

4- copy the content of the file and added in the yaml file with the secret part 
 
5. Deploy JBoss to Kubernetes
Use the deployment YAML file to deploy JBoss with the custom ports (30555 for HTTP and 30777 for HTTPS).

bash

kubectl apply -f jboss-deployment.yaml

5. Verify Deployment

kubectl get pods
kubectl get svc

6. Access JBoss
Once deployed, access the JBoss application links with :

minikube service jboss-service --url 

http:// <server-ip>:30555
https://<server-ip>:30777
http://<server-ip>:30990

7. Clean Up Resources
To delete the deployment, run:

bash
kubectl delete -f jboss-deployment.yaml


Customization
Port Changes: If you want to use different HTTP and HTTPS ports, modify the ports in both jboss-deployment.yaml
