# azureFile Volumes


* network filesystem (fileshare)
* ...


## 1 - Static Volume

prerequisites:
* storage account exists
* inside the storage account a file share `static-azure-file-volume` exists
* the kubernetes secet object `handelsblatt-storage-secret` 
 - see [create storage account](./create-storage-account.md)

The file share `static-azure-file-volume` will be mounted as a volume into the pod.

```
kubectl config set-context $(kubectl config current-context) --namespace=snippets
kubectl apply -f 1-static.yaml
```

write into volume and check results
```
kubectl get pods -n snippets

kubectl exec -n snippets -it static-azure-file-volume  -- /bin/bash
cd /mnt/azure
echo "whats up" > zeit.txt

kubectl delete pod static-azure-file-volume -n snippets
```


## 2 - Static Volume (using PersistentVolumeClaim)

* PersistentVolume: define access type (read/write)
```
kubectl apply -f 2-static-pvc.yaml

kubectl exec -n snippets -it static-pvc-azure-file-volume  -- /bin/bash
cd /mnt/azure
cat zeit.txt
```



## 3 - Dynamic Persistent Volume

* Kubernetes will create the volume if it does not exists
* it will continue to exist as log as the PersistentVolumeClaim exists

```
kubectl apply -f 3-dynamic-persistent.yaml

kubectl exec -n snippets -it dynamic-persistent-azure-file-volume  -- /bin/bash
cd /mnt/azure
echo "hello world" > handelsblatt.txt
```

check if volume still exists:
```
# 1) delete pod
kubectl delete pod dynamic-persistent-azure-file-volume -n snippets
kubectl get pods -n snippets

# 2) recreate pod
kubectl apply -f 3-dynamic-persistent.yaml

```

Once the PersistentVolumeClaim is deleted the file share gets delete in azure as well:
```
# 1) check file share exists in azure

# 2) delete pvc & check file share still exists in azure 
kubectl get pvc -n snippets
kubectl delete pod dynamic-persistent-azure-file-volume -n snippets
kubectl delete pvc dynamic-persistent-azure-file-volume -n snippets
kubectl get pvc -n snippets
```