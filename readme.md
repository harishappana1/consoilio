# Kubernetes Cluster Setup and GPU Workload Deployment

## Objective

The goal of this project is to set up a Kubernetes cluster using Ansible and Helm for a web application.


## Architecture

### Kubernetes Cluster

- **Managed Azure Kubernetes Service (AKS)**: A scalable, managed Kubernetes cluster with node pools for web applications and GPU workloads.
- **Node Pools**:
  - **webapp**: Handles general web application workloads.
  - **gpu**: Dedicated to GPU-enabled workloads.

  Note: We can create kubernetes cluster in group of vms as well using kubeadm, then joining the worker nodes into the cluster.

### GPU Workload Deployment
-  To interact with GPU hardware, OS kernel requires supported drivers to communicate with GPU hardware.
- **NVIDIA Device Plugin**: Ensures Kubernetes can schedule GPU workloads on nodes with NVIDIA GPUs using node selector or labels.
- **NVIDIA Driver DaemonSet**: Installs and manages NVIDIA drivers on GPU nodes using node selector or labels.

## Setup Instructions

### 1. Configure Azure CLI

Make sure you are logged in to your Azure account:
```bash
az login
```
Set the appropriate subscription if you have multiple subscriptions:
```bash
az account set --subscription "your-subscription-id"
```

### 2. Run Ansible Playbook
Ensure you have your Azure credentials and other necessary details configured in k8s_cluster_setup.yml. Then, execute the following command to create and configure the AKS cluster:

```bash
ansible-playbook k8s_cluster_setup.yml
```

Create a resource group in Azure.
Deploy a managed Azure Kubernetes Service (AKS) cluster with the specified configurations, including node pools for web applications and GPU workloads.
### 3. Verify Cluster Creation
After the playbook runs successfully, verify the AKS cluster and its node pools:

```bash
az aks list --resource-group <your-resource-group> --output table
```

Check the status of the cluster and node pools to ensure they are correctly set up and running.

### 4. Deploy GPU Drivers and Plugins
After setting up the cluster, deploy the NVIDIA drivers and device plugin:

#### Deploy NVIDIA Driver DaemonSet:

```bash
kubectl apply -f nvidia_driver_daemonSet.yaml
```

This will install NVIDIA drivers on all GPU nodes.

#### Deploy NVIDIA Device Plugin DaemonSet:

```bash
kubectl apply -f nvidia_device_plugin_daemonSet.yaml
```

### 5. Deploy the Application
Prepare Helm Chart Values: Configure the values.yaml file in your Helm chart with appropriate settings for your application.

#### Install Application Using Helm:

```bash
helm install my-app ./helmcharts
```


This will deploy your application to the Kubernetes cluster.
This will ensure that Kubernetes can schedule GPU workloads on nodes with NVIDIA GPUs.

### Summary

GPU based application will schudule at GPU enabled applciation to run and utilise the hardware.