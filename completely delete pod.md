```yml
Completely delete pod
kubectl delete --all pods
 #kubectl delete --all deployments 
#kubectl delete --all svc
```

Completely delete pod, pv, pvc  

Delete Your MySQL Instance

```yml
kubectl get pvc

kubectl delete pvc  mysql-pv-claim [pvc-name]

kubectl get pv 

 kubectl delete pv  mysql-pv [pv-name]

kubectl get deployment

kubectl delete deployment mysql [deployment name]

kubectl get service  or (svc)

kubectl delete service  mysql [service name] 
```
