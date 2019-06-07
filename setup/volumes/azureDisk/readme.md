# azureDisk Volumes

What are the differences between the disk types?

* Managed (Azure managed data disk)
* Shared (multiple disks per storage account)
* Dedicated (single blob disk per storage account)

**Managed:**
This is a virtual disk that is attached to the VM (direct attached disk) and uses real dedicated disk hardware. Unlike the other azureFile volumes which are network file systems, this disk can only be attached to one node and only one pod should access it (the underlying filesystem is not aware of it and it may lead to conflicts).

**Shared and Dedicated:**
Disk wrapped with a storage account.


# 1 - Static Managed Volume

When creating Azure Disks make sure the Kubernetes Cluster Service Principal has access to this resource. The easiest way is to just use the cluster node resource group, which is accessible by the kubernetes:

```
$DISK_NAME="static-managed-azure-disk-volume"
$AKS_NODE_RG=$(az aks show --resource-group $AKS_RG --name $AKS_NAME --query nodeResourceGroup -o tsv)
az disk create --resource-group $AKS_NODE_RG --name $DISK_NAME --size-gb 20 --query id --output tsv
```

```
# please make sure 'diskName' and 'diskURI' are correct
kubectl apply -f 1-static-managed.yaml
```


# 2 - Managed Volume

Kubernetes will create a persistent disk. As long as the PersistentVolumeClaim (PVC) exists, the disk will be available in azure. Once the PVC is deleted the disk will also disappear in azure.

```
kubectl apply -f 2-managed.yaml
```


# 3 - Dedicated and Shared Volume

**Please beware when attaching azureDisk**

When attaching an Azure Disk to a Kubernetes VM (node), the OS disk type needs to match the Azure Disk type. 
In this setup the OS disk is of kind "Managed". So only "Managed" disks can be attatched to the cluster nodes.

Attaching disks of kind "Shared" or "Dedicated" will fail with the following error message:

`"Addition of a blob based disk to VM with managed disks is not supported." Target="dataDisk"`

quote: "[...] pls note that managed disk could only be attached to VM with managed os disk, blob based disk could only be attached to VM with blob based os disk." - https://github.com/Azure/acs-engine/issues/2299


# Demonstration

## 0 - getting started

```
kubectl config set-context $(kubectl config current-context) --namespace=snippets
```

## 1 - static managed azure disk

static-managed-azure-disk-volume

```
# create static azure disk
portal -> disk -> add: 
name: static-managed-azure-disk-volume
resource group: 

# edit 1-static-managed.yaml
replace diskURI

kubectl apply -f .\1-static-managed.yaml
kubectl describe pod/static-managed-azure-disk-volume

kubectl exec -n snippets -it static-managed-azure-disk-volume  -- /bin/bash
cd /mnt/azure
echo "whats up" > zeit.txt
exit
```

## 2 - dynamic managed azure disk

```
kubectl config set-context $(kubectl config current-context) --namespace=snippets

kubectl apply -f .\2-managed.yaml
kubectl describe pod/managed-azure-disk-volume

kubectl exec -n snippets -it managed-azure-disk-volume  -- /bin/bash
cd /mnt/azure
echo "whats up" > zeit.txt
```
