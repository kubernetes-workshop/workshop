# Happy - Azure DNS Example

## 1. Create Kubernetes Application
```
kubectl create ns good-gate

kubectl apply -f .\good-gate.yaml
kubectl get svc -n good-gate --watch # get Private IP of the service for later use (EXTERNAL-IP)
```

## 2. Setup Application Gateway:

1) Add a Subnet in the Virtual Network of the Kubernetes Cluster 
* Virtual Networks -> Subnets -> Add (name: app_gw_subnet, range: 10.0.0.0/26)

2) Add Application Gateway
* use Cluster Virtual Network, select app_gw_subnet)
(it will take about 5min for the Application Gateway to setup)

3) Configure the Application Gateway (define target)
* Navigate to Backend Pools -> Select existing pool appGatewayBackendPool -> Add target [Private IP of the Service]
(takes about 5min for the changes to kick in)

## 3. Demonstrate

* visit: http://good-gate.westeurope.cloudapp.azure.com
* subdomains land in default nginx backend

source:
https://rinormaloku.com/prohibiting-direct-access-microservices-aks/



