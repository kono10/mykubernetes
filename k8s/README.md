
```
kubectl apply -f <filename>
```

* when creating a service kubernetes creates its own service

```
❯ kubectl get services
NAME                   TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
django-app-node-port   NodePort    10.99.129.73   <none>        3050:31515/TCP   67s
kubernetes             ClusterIP   10.96.0.1      <none>        443/TCP          3d5h
❯ kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
django-app   1/1     Running   0          18m
```

Visit app at http://localhost:31515/
