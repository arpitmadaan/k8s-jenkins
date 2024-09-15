# Kubernetes Manifests for Jenkins Deployment

### Step 1: Create namsepace
```console
kubctl apply -f namespace.yaml
```
### Step 2: Create service accounts, cluster role and binding
The 'serviceAccount.yaml' creates a 'jenkins-admin' clusterRole, 'jenkins-admin' ServiceAccount and binds the 'clusterRole' to the service account.

The 'jenkins-admin' cluster role has all the permissions to manage the cluster components. You can also restrict access by specifying individual resource actions.
```console
kubectl apply -f serviceAccount.yaml
```
### Step 3: Create volume for the persistence of data
Uncomment the storage class if you do not have it already in your cluster.
Replace the node with the actual node name where your NFS is attached
And give the actual path of the directory on your node
```console
kubectl apply -f volume.yaml
```
### Step 4: Create the deployment
```console
kubectl apply -f deployment.yaml
```

### Step 5: Create the service for accessing the Jenkins UI
Here, we are using the type as 'NodePort' which will expose Jenkins on all kubernetes node IPs on port 32000. If you have an ingress setup, you can create an ingress rule to access Jenkins. Also, you can expose the Jenkins service as a Loadbalancer if you are running the cluster on AWS, Google, or Azure cloud.
kubectl apply -f service.yaml

Now, when browsing to any one of the Node IPs on port 32000, you will be able to access the Jenkins dashboard.

http://<node-ip>:32000

Jenkins will ask for the initial Admin password when you access the dashboard for the first time.

You can get that from the pod logs.
With the pod name, you can get the logs as shown below. Replace the pod name with your pod name.

kubectl logs jenkins-deployment-2539456353-j00w5 --namespace=devops-tools
The password can be found at the end of the log.

Alternatively, you can run the exec command to get the password directly from the location as shown below.

```console
kubectl exec -it jenkins-559d8cd85c-cfcgk cat /var/jenkins_home/secrets/initialAdminPassword -n devops-tools
```