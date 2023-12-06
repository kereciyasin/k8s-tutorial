# k8s-tutorial
SA-II Projekt

# Installation Guide for Prometheus and Grafana on Kubernetes

This guide provides a step-by-step tutorial for setting up Prometheus using Helm and deploying Grafana via YAML on a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster setup is required.
- Helm installation is necessary. Refer to [Helm Installation Guide](https://helm.sh/docs/intro/install/) for instructions.

## Installation Steps

1. **Update System Package Database**

    ```bash
    # Update system package database
    $ sudo apt update
    ```

2. **Install Docker**

    ```bash
    # Install Docker
    $ sudo apt install docker.io
    $ docker --version # Check Docker version
    ```

3. **Download and Setup Minikube**

    ```bash
    # Download Minikube
    $ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    $ sudo install minikube-linux-amd64 /usr/local/bin/minikube
    $ minikube version # Check Minikube version

    # Install Kubectl
    $ sudo snap install kubectl --classic
    $ kubectl version # Check Kubectl version

    # Start Minikube with Docker driver
    $ minikube start --driver=docker
    $ sudo usermod -aG docker $USER && newgrp docker
    $ minikube node add —worker
    $ kubectl label node <node_name> node-role.kubernetes.io/worker=worker
    $ kubectl get nodes # List Kubernetes nodes
    ```

4. **Install Helm and Prometheus**

    ```bash
    # Install Helm
    $ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    $ chmod 700 get_helm.sh
    $ ./get_helm.sh

    # Add Prometheus Helm repository
    $ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    $ helm repo list
    $ helm repo update
    $ helm install prometheus prometheus-community/prometheus
    $ kubectl get pod # List Kubernetes pods
    ```

5. **Configure Prometheus and Grafana**

    ```bash
    # Edit Prometheus server configuration
    $ kubectl edit deploy prometheus-server
    $ kubectl edit svc prometheus-server

    # Create Grafana configuration file and apply
    $ sudo nano grafana.yaml
    $ kubectl apply -f grafana.yaml
    $ kubectl get pods # List Kubernetes pods
    ```

6. **Set Up Firewall Rules**

    ```bash
    # Check Uncomplicated Firewall status
    $ sudo ufw status 

    # Enable Uncomplicated Firewall
    $ sudo ufw enable

    # Allow ports for Prometheus and Grafana
    $ sudo ufw allow 30020 # Prometheus
    $ sudo ufw allow 30030 # Grafana
    ```

7. **Accessing Services**

    ```bash
    # Start Prometheus service
    $ minikube service prometheus-server
    # Start Grafana service
    $ minikube service prometheus-server
    ```

8. **Additional Configurations**

    ```bash
    # Install Socat
    $ sudo apt-get install -y socat

    # Redirect specific port
    $ sudo socat TCP-LISTEN:30030,fork TCP:192.168.49.2:30193

    # Access Grafana using a browser:
    # https://192.160.100.60:30030
    ```

## Useful Links

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Grafana Installation on Kubernetes](https://grafana.com/docs/grafana/latest/setup-grafana/installation/kubernetes/)
- [Grafana Installation using Docker](https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/#migrate-to-v51-or-later)
- [Bitnami Helm Charts Values](https://github.com/bitnami/charts/blob/22f189d36c70da0f46733fd5da548f6ea6ec5d28/bitnami/grafana/values.yaml#L250-L259)
- [Bitnami Helm Charts Issues](https://github.com/bitnami/charts/issues/7662)
