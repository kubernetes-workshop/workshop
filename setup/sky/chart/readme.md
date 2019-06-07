

deploy manualy
```
$NAMESPACE="sky"
kubectl create namespace $NAMESPACE
helm upgrade . $NAMESPACE --install
helm delete --purge $NAMESPACE
```