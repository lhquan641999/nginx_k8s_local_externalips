# set up ssl/tls nginx for k8s local with container outside k8s cluster

## Create service with none type and change your port in file service none
```
  kubectl apply -f service_none.yaml
```

## Create endpoint with your IP and port mapping
```
  kubectl apply -f endpoint.yaml
```

## Create Ingress with your service
```
  kubectl apply -f ingress.yaml
```