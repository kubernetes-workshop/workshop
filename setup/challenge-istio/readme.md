
```
# setup
k apply -f .\1-base
k apply -f .\2-request-routing

```


2) request routing
* destination + DestinationRule + Subsets 
* Gateway 
* VirtualService 


```
# check: DestinationsRule + VirtualService + Gateway
kubectl get destinationrules -n challengeistio
kubectl get virtualservices -n challengeistio
kubectl get gateways -n challengeistio

# IP adress of istio gateway
kubectl describe svc/istio-ingressgateway -n istio-system
```






