## Access Explanation

```
Namespace =>  	Staging 	Production

Developer     	Full Access 	No access
Admin 		Full Access 	Full Access

```

## Applu the yaml files to create RBAC

``` 

kubectl apply -f staging-admin-role.yaml
kubectl apply -f staging-developer-role.yaml
kubectl apply -f production-admin-role.yaml
kubectl apply -f staging-admin-binding.yaml
kubectl apply -f staging-developer-binding.yaml
kubectl apply -f production-admin-binding.yaml

```

## To Verify your access - you can use one of the below commands ('kubectl auth can-i' commands are very handy)

```
  # Check to see if I can create pods in any namespace
  kubectl auth can-i create pods --all-namespaces
  
  # Check to see if I can list deployments in my current namespace
  kubectl auth can-i list deployments.apps
  
  # Check to see if service account "foo" of namespace "dev" can list pods in the namespace "prod"
  # You must be allowed to use impersonation for the global option "--as"
  kubectl auth can-i list pods --as=system:serviceaccount:dev:foo -n prod
  
  # Check to see if I can do everything in my current namespace ("*" means all)
  kubectl auth can-i '*' '*'
  
  # Check to see if I can get the job named "bar" in namespace "foo"
  kubectl auth can-i list jobs.batch/bar -n foo
  
  # Check to see if I can read pod logs
  kubectl auth can-i get pods --subresource=log
  
  # Check to see if I can access the URL /logs/
  kubectl auth can-i get /logs/
  
  # Check to see if I can approve certificates.k8s.io
  kubectl auth can-i approve certificates.k8s.io
  
  # List all allowed actions in namespace "foo"
  kubectl auth can-i --list --namespace=foo

```
