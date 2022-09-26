https://www.youtube.com/watch?v=X48VuDVv0do

- Slow start or failures
https://stackoverflow.com/questions/56327843/minikube-is-slow-and-unresponsive
https://github.com/kubernetes/minikube/issues/12920

- FRESH START
```
minikube delete --all --purge
minikube start
kubectl get all
```


- Commands, 00nginx
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


- Commands, 01mongo
    - Secret needs to be applied before deployment, or the deployment will fail
```
kubectl apply -f ./mongodb-secret.yaml
kubectl apply -f ./mongo-deployment.yaml
kubectl get secret
kubectl get service
kubectl describe service mongodb-service
kubectl get pod -o wide # Check that service "Endpoints" match pods IPs
```
```
```