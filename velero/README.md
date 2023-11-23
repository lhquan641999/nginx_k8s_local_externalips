# Setup Velero into K8S and Backup / Migrate

## Remove prevous version of velero on cluster
```
  kubectl delete namespace/velero clusterrolebinding/velero
  kubectl delete crds -l component=velero
```

## Install Velero CLI
```
  wget https://github.com/vmware-tanzu/velero/releases/download/v1.9.5/velero-v1.9.5-linux-amd64.tar.gz
  tar -xvf velero-v1.9.5-linux-amd64.tar.gz 
  cp -rf velero-v1.9.5-linux-amd64/velero /usr/local/bin/
```

## Create credential file for S3 bucket / Minio
```
[default]
aws_access_key_id = minio
aws_secret_access_key = P@ssword123
```


## Install Velero into cluster
```
Example:
  velero install --provider aws --plugins velero/velero-plugin-for-aws:v1.5.5  --use-restic --use-volume-snapshots=false --bucket k8s-backup --secret-file ./credentials --backup-location-config region=minio,s3ForcePathStyle="true",s3Url=https://minio.rbvh-etm-cloud.com --velero-pod-mem-limit=1024Mi
```

## Create Backup, if you want to backup all workloads we dont need to specific namepspace,resources
```
  velero backup create app --default-volumes-to-restic --include-namespaces app 
  velero backup create k8s
```

## Check State
```
  velero backup describe argocd
  velero backup logs argocd
```

## Restore Backup
```
  velero restore create --include-resources pvcs,pvs --from-backup argocd
  velero restore create --from-backup argocd 
```

## Check State
```
 velero restore describe argocd-20231121084315
 velero restore logs argocd-20231121084315
```

## Backup with schedule
```
 velero schedule create argocd-daily --schedule="*/5 * * * *" --include-namespaces argocd
 velero schedule create nginx-daily --schedule="@daily" --selector app=nginx
 velero schedule create weekly-backup --schedule="@weekly" --ttl 336h
```

## Backup with specific resources
```
  https://velero.io/docs/v1.4/resource-filtering/ 
```
## Backup and restore data in pv/pvc
```
  velero backup create pv --default-volumes-to-restic --include-resources pv --selector app=nginx
  velero backup create backup-test  --default-volumes-to-restic --include-namespaces test --ttl 0h10m
  velero restore create --from-backup backup-test
```