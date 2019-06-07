# Tree - DNS with Ingress

```
$NAMESPACE="tree"
kubectl create namespace $NAMESPACE

# install nginx-ingress
helm install stable/nginx-ingress --name $NAMESPACE-ingress --set controller.scope.enabled=true,controller.scope.namespace=$NAMESPACE,controller.service.externalTrafficPolicy=Local --namespace $NAMESPACE

# deploy the app
kubectl apply -f .

# get the ExternalIP and update the A-Record in ddnss.de
kubectl get all -n $NAMESPACE
$INGRESS_CONTROLLER_IP="40.68.184.51"
```

visit:
* $INGRESS_CONTROLLER_IP
* fnbk.ddnss.de
* fnbk.ddnss.de/orange
* banana.fnbk.ddnss.de


side note:
=> best practice: give each namespace a separate ingress controller, because things get messy and start to fall into reload issues