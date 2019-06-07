# Happy - Azure DNS Example

```
# Install ingress controller via helm chart
helm install stable/nginx-ingress --namespace happyday --set controller.replicaCount=2

# get public IP from ingress controller (.ipAddress)
kubectl get service -l app=nginx-ingress --namespace happyday
$INGRESS_CONTROLLER_IP="40.68.184.51"
$DNS_NAME="eagle"

# Get the resource-id of the public-ip matching $INGRESS_CONTROLLER_IP (.id)
az network public-ip list
$PUBLIC_IP_RESOURCE_ID="/subscriptions/4427b046-b93b-421e-b487-f1bf5214fcb1/resourceGroups/mc_eagle-cluster_eagle_westeurope/providers/Microsoft.Network/publicIPAddresses/kubernetes-a0a7630b3765411e9a386d245c3357cc"

# Update public ip address with dns name
az network public-ip update --ids $PUBLIC_IP_RESOURCE_ID --dns-name $DNS_NAME

# Get fully qualified domain name (.dnsSettings.fqdn)
az network public-ip show --ids $PUBLIC_IP_RESOURCE_ID
$FQDN="eagle.westeurope.cloudapp.azure.com"

# generate TLS
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -out aks-ingress-tls.crt -keyout aks-ingress-tls.key -subj "/CN=$FQDN/O=aks-ingress-tls"

# create namespace
kubectl create namespace happyday

# create k8s secret
kubectl create secret tls aks-ingress-tls --key aks-ingress-tls.key --cert aks-ingress-tls.crt -n happyday

# deploy application
kubectl apply -f happyday.yaml

kubectl get all -n happyday
```

visit:
* https://eagle.westeurope.cloudapp.azure.com
* https://eagle.westeurope.cloudapp.azure.com/day
(may take some time to resolve, proceed security warning)