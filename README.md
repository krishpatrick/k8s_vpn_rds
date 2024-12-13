## K8S to RDS

### Objective ---> 

• Setting up RBAC for Kubernetes
• Secure Connection between k8s and RDS and secure access on Both env

### Requirements --->

```

Namespace => 	Staging 	Production
Developer =>	Full Access 	No access
Admin 	  =>	Full Access 	Full Access

```

### AWS RDS Access:

• Configure secure access to RDS instances for each environment.
• Use Kubernetes Secrets to store RDS credentials, with separate secrets for Staging and Production

Scenario 1: On-Premises Kubernetes Cluster Connecting to AWS RDS, using DB/user password secured as k8s secret using secure connection
Scenario 2: On-Premises Kubernetes Cluster Connecting to AWS RDS. Using IAM Authentication for RDS, Little complex setup (Optional/Out-of-Scope for this Demo)

### Recommendations and considerations:

• On-Prem Kubernetes to AWS RDS: You need to establish a VPN or Direct Connect link between your on-prem network and AWS to route traffic securely. You also need to store RDS credentials in Kubernetes secrets for secure access from pods.

• Kubernetes in AWS EKS to AWS RDS: The connectivity is easier as long as your Kubernetes cluster (e.g., Amazon EKS) and RDS are in the same VPC or peered VPCs. You’ll also likely use IAM roles for secure database access, use Serivce Accounts and configure security groups to allow traffic from your Kubernetes nodes to the RDS instance.

## Setup

We are mainly using On-prem to RDS scenario where we need create a site to site VPN, AWS Privatelink, Required gateways both side, routing and Security groups for RDS

1) RDS - rds folder README.md explains SSL and network folder has Networking setup required at both ends including On-Prem kubernetes
README.md	network
2) Kubernetes - k8s folder with subfolders 
deployment	eks_rds_infra	namespaces	rbac		secrets
