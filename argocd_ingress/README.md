# set up ssl/tls nginx for k8s local with argocd


## Create Ingress with your service
```
  kubectl apply -f ingress.yaml
```

## Note: Don't change annotation or secret name