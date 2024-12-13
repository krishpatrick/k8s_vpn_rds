Here’s how you can configure **SSL/TLS encryption** for database connections from your **on-prem Kubernetes cluster** to **AWS RDS**:

### Step 1: Enable SSL for Your RDS Instance

Ensure SSL is enabled on your RDS instance:

1. **Check SSL Status**: SSL is enabled by default for most RDS engines. You can verify with the AWS CLI:

   ```bash
   aws rds describe-db-instances --db-instance-identifier <your-db-instance-id>
   ```

   If SSL isn’t enabled, modify the instance:

   ```bash
   aws rds modify-db-instance --db-instance-identifier <your-db-instance-id> --enable-ssl --apply-immediately
   ```

### Step 2: Download the RDS Root CA Certificate

1. **Download the root CA certificate** for your RDS region:

   - [Download RDS Root CA](https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem).

2. **Store the Certificate**: Upload the certificate to your on-prem Kubernetes cluster. You can create a Kubernetes secret to hold the certificate for secure access.

   ```bash
   kubectl create secret generic rds-ca-certificate --from-file=rds-combined-ca-bundle.pem=/path/to/rds-combined-ca-bundle.pem --namespace <your-namespace>
   ```

### Step 3: Configure Your Database Client to Use SSL/TLS

Ensure your application or database client is set up to use SSL:

1. **PostgreSQL Example**: Add SSL mode and specify the CA certificate.

   ```bash
   psql "sslmode=verify-full sslrootcert=/path/to/rds-combined-ca-bundle.pem host=<your-db-hostname> dbname=<your-db-name> user=<your-db-user> password=<your-db-password>"
   ```

2. **MySQL Example**: Enable SSL with the CA certificate.

   ```bash
   mysql -h <your-db-hostname> -u <your-db-user> -p --ssl-ca=/path/to/rds-combined-ca-bundle.pem --ssl-mode=REQUIRED
   ```

3. **JDBC Example**: For Java applications, configure JDBC to use SSL with the certificate.

   ```java
   String url = "jdbc:postgresql://<your-db-hostname>:5432/<your-db-name>?ssl=true&sslmode=verify-full&sslrootcert=/path/to/rds-combined-ca-bundle.pem";
   ```

### Step 4: Configure Kubernetes Pods

For Kubernetes applications, pass the SSL settings via environment variables or mount the certificate file directly:

1. **Example for PostgreSQL**:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-app
   spec:
     containers:
     - name: my-app
       image: my-app-image
       env:
       - name: PGHOST
         value: "<your-db-hostname>"
       - name: PGUSER
         value: "<your-db-user>"
       - name: PGDATABASE
         value: "<your-db-name>"
       - name: PGSSLMODE
         value: "verify-full"
       - name: PGSSLROOTCERT
         valueFrom:
           secretKeyRef:
             name: rds-ca-certificate
             key: rds-combined-ca-bundle.pem
   ```

### Step 5: Verify SSL/TLS Encryption

1. **PostgreSQL**: Connect and check the SSL connection status:

   ```sql
   SHOW ssl;
   ```

   This should return `on` if SSL is used.

2. **MySQL**: Run the following command to verify SSL:

   ```sql
   SHOW VARIABLES LIKE '%ssl%';
   ```

3. **Test from Pod**: Connect from a Kubernetes pod to ensure SSL is working.

   ```bash
   kubectl exec -it my-app -- psql "sslmode=verify-full sslrootcert=/path/to/rds-combined-ca-bundle.pem host=<your-db-hostname> dbname=<your-db-name> user=<your-db-user> password=<your-db-password>"
   ```

By following these steps, you can ensure that your **on-prem Kubernetes cluster** securely connects to **AWS RDS** using **SSL/TLS encryption** for all database traffic.
