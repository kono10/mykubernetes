# mykubernetes
repo for my kubernetes notes

<img width="824" alt="Screen Shot 2022-07-05 at 3 20 15 PM" src="https://user-images.githubusercontent.com/19543073/177409563-aa3a262e-74fe-4283-8097-ab1eec14c3fc.png">

* Nodes: collection of containers
* Master: Passes commands to Nodes
* Cluster: Master + Nodes
* Minikube: used for local development, sets dev kubernetes cluseter
* Kubectl: program used for interacting with nodes in a cluster
* Pod: grouping of tightly grouped containers with a common purpose, in kubernetes cannot run a container alone a container must have a pod
* a think that exists inside of a kubernetes cluster (Pod, Service

<img width="550" alt="Screen Shot 2022-07-05 at 3 21 11 PM" src="https://user-images.githubusercontent.com/19543073/177409696-e394c688-8665-40cb-816b-39b0d1e61899.png">

<img width="343" alt="Screen Shot 2022-07-05 at 3 33 19 PM" src="https://user-images.githubusercontent.com/19543073/177411542-4798b2c2-be0f-4ddc-91b9-8eb10beaf071.png">

### Force kubernetes to restart a killed docker container 

```
❯ docker ps
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS          PORTS     NAMES
3aa3406d318d   novskytech/djangoproject   "gunicorn --bind ' 0…"   24 seconds ago   Up 23 seconds             k8s_django-app_django-app_default_fd52b0ac-1c74-4f32-a45d-ba6a0c77a9b9_1
❯ kubectl get pods
NAME         READY   STATUS    RESTARTS      AGE
django-app   1/1     Running   1 (52s ago)   24h
❯ docker kill 3aa3406d318d
3aa3406d318d
❯ kubectl get pods
NAME         READY   STATUS   RESTARTS      AGE
django-app   0/1     Error    1 (71s ago)   24h
❯ kubectl get pods
NAME         READY   STATUS   RESTARTS      AGE
django-app   0/1     Error    1 (76s ago)   24h
❯ kubectl get pods
NAME         READY   STATUS   RESTARTS      AGE
django-app   0/1     Error    1 (79s ago)   24h
❯ kubectl get pods
NAME         READY   STATUS    RESTARTS      AGE
django-app   1/1     Running   2 (21s ago)   24h

```
