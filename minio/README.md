# Setup Minio into K8S
## Create namespace then create storage account and pv,pvc
```
  kubectl create ns minio
  kubectl appply -f storageclass.yaml
  kubectl appply -f pv.yaml
  kubectl appply -f pv.yaml -n minio
```

## Create deployment and service for minio
### You need to update MINIO_ACCESS_KEY and  MINIO_SECRET_KEY for your credential and URL if you want to use domain
```
  kubectl apply -f deployment.yaml -n minio
  kubectl apply -f svc.yaml -n minio
```

## Create Ingress with your service
```
  kubectl apply -f ingress_api.yaml -n minio
  kubectl apply -f ingress.yaml -n minio
```