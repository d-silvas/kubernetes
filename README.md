https://www.youtube.com/watch?v=X48VuDVv0do

- Slow start or failures
https://stackoverflow.com/questions/56327843/minikube-is-slow-and-unresponsive
https://github.com/kubernetes/minikube/issues/12920

# FRESH START
```
minikube delete --all --purge
minikube start
kubectl get all
```


# [00] nginx
```
kubectl apply -f ./nginx-deployment.yaml
kubectl delete -f ./nginx-deployment.yaml#

kubectl get nodes
kubectl get pod
kubectl get pod -o wide
kubectl get deployments
kubectl get services
kubectl get deployment nginx-deployment -o yaml > nginx-deployment-status.yaml

kubectl describe service nginx-service
kubectl describe deployments
kubectl describe pods
kubectl describe services
```


# [01] mongo
- Secret needs to be applied before deployment, or the deployment will fail
```
kubectl apply -f ./mongodb-secret.yaml
kubectl apply -f ./mongo-deployment.yaml
kubectl get secret
kubectl apply -f ./mongo-service.yaml
kubectl get service
kubectl describe service mongodb-service
kubectl get pod -o wide # Check that service "Endpoints" match pods IPs
```
- Create deployment of mongo-express. Needs database user, password and url.
- Create a config map to store database url.
```
apply -f ./mongodb-configmap.yaml
kubectl apply -f ./mongoexpress-depl.yaml
kubectl get pods
kubect logs mongoexpress-deployment-65c56f4c98-g7cgk
```
- To access mongo express from a browser, we need to implement an external service. Add it to the same file as the deployment (`mongoexpress-depl.yaml`)
```
kubectl get service
```
- See that our internal mongodb-service has `ClusterIP` type, whereas the new mongo-express-service has `LoadBalancer` type. Kubernetes assigns an external IP address to the LoadBalancer service.
- With minikube, it doesn't exactly work like that, so we see `<pending>` on the screen. We need to tell minikube to assign this IP address:
```
minikube service mongoexpress-service 
```

# [02] Namespaces
```
kubectl get namespace
```
- `kube-*` are kubernetes own namespaces. `kubernetes-dashboard` is used by minikube.
- We can create a custom namespace via command line
```
kubectl create namespace my-namespace
```
- We can create a custom namespace by adding it to the `metadata.namespace` property of a yaml file. Example, `mysql-configmap.yaml`:
```yaml
apiVersion: 1
kind: ConfigMap
metadata:
    name: mysql-configmap
    namespace: database # <--- THIS !!
data:
    db_url: mysql-service.database
```
- Alternatively, we can apply a component to a namespace with the command line:
```
kubectl apply -f mongodb-configmap.yaml --namespace=database
```
- By default, components are created in the default namespace.
- The following are equivalent
```
kubectl get pods
kubectl get pods --namespace default
```

# [03] Ingress

- External services are only good for testing. For prod environments we use an internal service and an ingress to expose our apps.
```
kubectl apply -f ./mongoexpress-ingress.yaml
```
- Note that the files under the 03ingress folder are only for demonstration and they don't work. We would need and ingress controller and an entrypoint for them to work.
    - An entrypoint is an external proxy server or load balancer; should be a server which is separate to the cluster and has a public ip address and open ports to expose apps/services.
- Enable ingress in minikube. We should see it running under the kube-system namespace.
```
minikube addons enable ingress
kubectl get pod --namespace kube-system
```

TODO follow course (from ~2:14) to implement an ingress for minikube dashboard.
