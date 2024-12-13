##
## Create SSL certificates for securing the connection between kubernetes to RDS. Execute steps in ssl.readme.md 
## Steps
### Step 1-  Download the RDS Root CA Certificate

1. **Download the root CA certificate** for your RDS region:

   - [Download RDS Root CA](https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem).

2. **Store the Certificate**: Upload the certificate to your on-prem Kubernetes cluster. You can create a Kubernetes secret to hold the certificate for secure access.

   ```bash
   kubectl create secret generic rds-ca-certificate --from-file=rds-combined-ca-bundle.pem=/path/to/rds-combined-ca-bundle.pem --namespace staging
   kubectl create secret generic rds-ca-certificate --from-file=rds-combined-ca-bundle.pem=/path/to/rds-combined-ca-bundle.pem --namespace production
##

## Step 2 
## Steps to Encode the values using:

echo -n 'value' | base64
echo -n "staginguser" | base64

## To apply these secrets
```
kubectl apply -f staging-db-credentials.yaml
kubectl apply -f production-db-credentials.yaml
```

## To reference these secrets in your deployment yaml
## YAML

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: staging  # Or "production" for production environment
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app-image
        env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: staging-db-credentials  # Or "production-db-credentials"
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: staging-db-credentials  # Or "production-db-credentials"
              key: password
        - name: DB_SSL_CERT
          valueFrom:
            configMapKeyRef:
              name: rds-ca-certificate  # ConfigMap storing the RDS SSL certificate
              key: rds-combined-ca-bundle.pem
```
### YAML End

```
## TODO

1. Keyvault
2. Rotation
3. Replace pipeline task to replace tokens in these secret yamls during pipeline or deloyment

```

## Best Practices

#### Do not commit any senstive data like passwords and also optionally user and db-endpoint in to your git repo ####
#### Use KV Task or Hashicorp Vault during the pipeline ####
#### Setup Rotation policies for DB credentials stored in KeyVaults ####
### Optional use Credential providers concepts equivalent in AWS ??###
