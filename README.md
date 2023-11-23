# set up ssl/tls nginx for k8s local with externalIPs
![](/images/result.png)

# Install nginx controller
```shell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl create nginx-controller
helm install ingress-nginx ingress-nginx/ingress-nginx -n  nginx-controller
```
# Create self self signed ssl certificate using openssl
 -  Modify file cert-1/req.conf change CN,alt_names ...
 -  at folder cert-1 excute command:
 ```shell
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycertificate.crt -config req.conf
 ```
 - It's will generate files mykey.key and mycertificate.crt
# Modify service nginx controller

``` shell
kubectl edit  svc ingress-nginx-controller -n  nginx-controller
```
Write the line the yaml config with it similarly ingress-nginx-controller
```shell
  externalIPs: 
    - A.B.C.D
```
# Create the secret ssl/tls for ingress
Frist apply the app
```shell
kubectl create ns app
kubectl apply -f nginx.yaml -n app
```
```shell
kubectl create secret tls nginx-svc-tls --key mykey.key --cert mycertificate.crt -n app
```
Configure hostname nginx-ingress.yaml before apply

# Install self-cert windows
![](/images/install_cert_local.png)
# Configure hosts file in windows 
![](/images/changehost.png)

# Make sure Domain is mapped to public IP

# Install Cert Manager for latest version

```shell
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.yaml
```

# Install Cluster Issuer

```shell
kubectl apply -f clusterissuer.yaml
```

# Install Ingress


# Command taint node
```
  kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```