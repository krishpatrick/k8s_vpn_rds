## Provision Kubernetes Cluster on AWS (EKS). It is easy way or you can also use terraform

## Step 1:
## 1.1. Prerequisites

Install and configure the following tools locally:

AWS CLI
kubectl
eksctl
Helm

## 1.2. Create an EKS Cluster

Use eksctl to create a cluster:

```
eksctl create cluster \
  --name my-cluster \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
```

This creates an EKS cluster with: A managed node group of 3 nodes.
Auto-scaling enabled (min: 1, max: 4).

## 1.3. Verify the Cluster

Once the cluster is created, ensure kubectl is configured to use it:

```
aws eks update-kubeconfig --region us-east-1 --name my-cluster
kubectl get nodes
```

## Step 2 . Quick Steps to follow, Find out for each step the details
```
## 2.1 - How to securely connect EKS and RDS as it is AWS, 
## 2.2 - SAME VPC or configure VPC peering if they are in different VPC's
## 2.3 - Best practices to Use IAM , Use Service Accounts and attach those to pods and configure RDS with required IAM policies.
```
