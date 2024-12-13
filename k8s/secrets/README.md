#### Do not commit any senstive data like passwords and also optionally user and db-endpoint in to your git repo ####
#### Use KV Task or Hashicorp Vault during the pipeline ####
#### Setup Rotation policies for DB credentials stored in KeyVaults ####
### Optional use Credential providers cocepts equivalent in AWS ??###

TODO 
1. Keyvault
2. Rotation
3. Replace pipeline task to replace tokens in these secret yamls during pipeline or deloyment

## Encode the values using:

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
              name: rds-ssl-cert  # ConfigMap storing the RDS SSL certificate
              key: rds-combined-ca-bundle.pem
```
### YAML End

```
## TODO

1. Keyvault
2. Rotation
3. Replace pipeline task to replace tokens in these secret yamls during pipeline or deloyment

```
