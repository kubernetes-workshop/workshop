

deploy manualy
```
helm upgrade sky .\helm\ --install --set dataApi.imageTag=73 --set flightsApi.imageTag=55 --set quakesApi.imageTag=64 --set serviceTrackerUi.imageTag=65 --set weatherApi.imageTag=62 
```

delete manifest deployments
```
k delete deployment data-api -n sky
k delete deployment flights-api -n sky
k delete deployment quakes-api -n sky
k delete deployment service-tracker-ui -n sky
k delete deployment weather-api -n sky

k delete svc data-api -n sky
k delete svc flights-api -n sky
k delete svc quakes-api -n sky
k delete svc service-tracker-ui -n sky
k delete svc weather-api -n sky
```