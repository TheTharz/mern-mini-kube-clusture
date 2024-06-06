# MERN Mini Kube Cluster

This project sets up a MERN stack application (MongoDB, Express.js, React, Node.js) on a Kubernetes cluster using Minikube. Follow the steps below to deploy the application.

## Prerequisites

- Docker
- Minikube
- Kubectl
- AWS EC2 instance (if deploying on AWS)

## Setup Instructions

### 1. Start Minikube

Make sure you have Minikube installed and start it with Docker driver:

```sh
minikube start --driver=docker
```

### 2. Clone the Repository

Clone this repository to your local machine:

```sh
git clone https://github.com/your-username/mern-mini-kube-cluster.git
cd mern-mini-kube-cluster
```

### 3. Apply Kubernetes Configurations

Apply the Kubernetes configuration files in the following order:

#### a. Create the Secret

Create the Kubernetes Secret to store sensitive information:

```sh
kubectl apply -f secret.yaml
```

#### b. Apply MongoDB Configuration

Apply the MongoDB ConfigMap or other configuration:

```sh
kubectl apply -f mongo-config.yaml
```

#### c. Deploy MongoDB Application

Deploy the MongoDB application, which includes deployment and service definitions:

```sh
kubectl apply -f mongo-app.yaml
```

#### d. Deploy Web Application

Finally, deploy the web application, which depends on MongoDB:

```sh
kubectl apply -f web-app.yaml
```

### 4. Verify the Deployment

Check that all pods and services are running correctly:

```sh
kubectl get pods
kubectl get services
```

To get detailed information about a specific pod or service:

```sh
kubectl describe pod <pod-name>
kubectl describe service <service-name>
```

### 5. Access the Web Application

Retrieve the URL of the web application service and open it in your browser:

```sh
minikube service webapp-service
```

### Notes

- Ensure that your AWS EC2 instance has the necessary permissions and security group settings to allow traffic to and from Minikube.
- If deploying on AWS, you may need to set up proper networking configurations and security group settings.

To access the web page running on your Minikube cluster from your local machine, you need to ensure that your AWS EC2 instance is reachable from your local machine, and then you can forward the port from your EC2 instance to your local machine using SSH port forwarding.

1. **Ensure AWS EC2 Instance is Reachable**: Make sure that your AWS EC2 instance is accessible from your local machine. You should be able to SSH into your EC2 instance from your local terminal.

2. **SSH Port Forwarding**: Once connected to your EC2 instance via SSH, you can forward traffic from a port on your EC2 instance to a port on your local machine using SSH port forwarding.

   ```bash
   ssh -i <path-to-your-aws-keypair> -L <local-port>:<minikube-ip>:30100 ubuntu@<ec2-instance-public-ip>
   ```

   Replace:
   - `<path-to-your-aws-keypair>`: Path to your AWS key pair file.
   - `<local-port>`: The port on your local machine where you want to access the web page.
   - `<minikube-ip>`: The IP address of your Minikube cluster (`192.168.49.2` in your case).
   - `<ec2-instance-public-ip>`: The public IP address of your EC2 instance.

   This command will establish an SSH connection to your EC2 instance and forward traffic from `<local-port>` on your local machine to port `30100` on your Minikube cluster, allowing you to access the web page running on your Minikube cluster from your local machine.

3. **Access Web Page**: Once the SSH connection is established, you can open a web browser on your local machine and navigate to `http://localhost:<local-port>` to access the web page.

## Troubleshooting

- If you encounter issues with starting Minikube, ensure Docker is running and configured correctly.
- For issues with Kubernetes resources, use `kubectl logs <pod-name>` to check the logs of a specific pod.

## Cleanup

To delete all resources created by these configurations:

```sh
kubectl delete -f web-app.yaml
kubectl delete -f mongo-app.yaml
kubectl delete -f mongo-config.yaml
kubectl delete -f secret.yaml
minikube stop
```

## License

This project is licensed under the MIT License.
