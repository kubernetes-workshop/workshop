
# How to create a storage account

* create storage account
* get storage account name and key

create storage account:
```
$REGION="west europe"
$LOCATION="westeurope"
$AKS_RG="eagle-cluster"
$AKS_NAME="eagle"
$STORAGE_ACCOUNT_NAME="handelsblatt" # must be unique across azure

# Create a storage account
az storage account create -n $STORAGE_ACCOUNT_NAME -g $AKS_RG -l $LOCATION --sku Standard_LRS

# Get storage account key
$STORAGE_ACCOUNT_KEY=$(az storage account keys list --resource-group $AKS_RG --account-name $STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
Write-Host "Storage account name: $STORAGE_ACCOUNT_NAME"
Write-Host "Storage account key: $STORAGE_ACCOUNT_KEY"
```

# How to create a file share in an existing storage account

```
$STORAGE_SHARE_NAME="static-azure-file-volume"
$STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n $STORAGE_ACCOUNT_NAME -g $AKS_RG -o tsv)
az storage share create -n $STORAGE_SHARE_NAME --connection-string $STORAGE_CONNECTION_STRING
```

create a secret object in kubernetes:
```
$KUBERNETES_SECRET_OBJECT_NAME="handelsblatt-storage-secret"
kubectl create secret generic $KUBERNETES_SECRET_OBJECT_NAME --from-literal=azurestorageaccountname=$STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_ACCOUNT_KEY --namespace snippets
kubectl get secret -n snippets
```
