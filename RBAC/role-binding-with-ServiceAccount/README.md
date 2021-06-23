```
kubectl auth can-i get cronjobs -n webapp-namespace --as system:serviceaccount:webapp-namespace:webapp-service-account
no
```


```
kubectl auth can-i get pods -n webapp-namespace --as system:serviceaccount:webapp-namespace:webapp-service-account
yes
```
