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

# Accessing Minikube Dashboard on EC2 Instance Locally

This guide outlines the steps to access the Minikube Dashboard, running on an EC2 instance, from your local machine through SSH tunneling.

## Prerequisites

1. **Minikube**: Ensure Minikube is installed on your EC2 instance.
2. **SSH Key Pair**: Have an SSH key pair (.pem file) to authenticate with your EC2 instance.
3. **Public IP of EC2 Instance**: Obtain the public IP address of your EC2 instance.

## Step 1: Get Minikube Dashboard URL

First, you need to retrieve the Minikube Dashboard URL.

```bash
$ minikube dashboard --url
```

## Step 2: Create SSH Tunnel

Open another terminal on your local machine and create an SSH tunnel to your EC2 instance.

```bash
$ ssh -i [private-key.pem] -L [local-port]:localhost:[remote-port] ubuntu@[public-ip]
```

Replace the placeholders with the actual values:
- `[private-key.pem]`: Path to your SSH private key file.
- `[local-port]`: Local port number to forward the traffic to.
- `[remote-port]`: Port number on which Minikube Dashboard is running on the EC2 instance.
- `[public-ip]`: Public IP address of your EC2 instance.

## Step 3: Access Minikube Dashboard

Once the SSH tunnel is established, open a web browser and navigate to `http://localhost:[local-port]`.

## Additional Notes

- Ensure Minikube is up and running on your EC2 instance.
- Make sure the specified local port is not already in use.
- Keep the SSH session open to maintain the tunnel connection.

## Example

For example, if your Minikube Dashboard URL is `http://192.0.2.123:8080`, and you want to access it locally on port `9090`, and your SSH private key is named `my-key.pem`, the command would look like this:

```bash
$ ssh -i my-key.pem -L 9090:localhost:8080 ubuntu@192.0.2.123
```

Then, you can access the Minikube Dashboard by visiting `http://localhost:9090` in your web browser.

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
The setup instructions you've provided for deploying a MERN stack application on a Kubernetes cluster using Minikube are detailed and well-organized. Including a troubleshooting section and cleanup instructions adds value to the guide, helping users navigate potential issues and maintain a clean environment.

Here's the added blog reference:

---

**Reference Blog:** [Deploy MERN App on Kubernetes](https://ghazanfaralidevops.medium.com/deploy-mern-app-on-kubernetes-ccacd628aacb)

## License

This project is licensed under the MIT License.
