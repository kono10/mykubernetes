# mykubernetes
repo for my kubernetes notes

<img width="824" alt="Screen Shot 2022-07-05 at 3 20 15 PM" src="https://user-images.githubusercontent.com/19543073/177409563-aa3a262e-74fe-4283-8097-ab1eec14c3fc.png">

Command Summary

`kubernetes describe <object type> <object name>`
* prints out info about the config, can use it to check the image that is being used in the pod

`kubernetes delete -f <config file>`
* delete a kubernetes object

`kubernetes get pods -o wide`
* get an extended list of pod info

```
❯ kubectl get pods -o wide
NAME                                 READY   STATUS    RESTARTS   AGE    IP          NODE             NOMINATED NODE   READINESS GATES
django-deployment-7fcf74dd4f-w58t8   1/1     Running   0          5m7s   10.1.0.15   docker-desktop   <none>           <none>
```
* IP -> Pod ip address inside of the node, as nodes are added and deleted the ip will change, node-port service will have a static ip address that will make our app easy to access via a web browser.

Vocab Summary
* Nodes: collection of Pods
* Master: Passes commands to Nodes
* Cluster: Master + Nodes
* Minikube: used for local development, sets dev kubernetes cluseter
* Kubectl: program used for interacting with nodes in a cluster
* Pod: tightly grouped containers with a common purpose, in kubernetes cannot run a container alone a container must have a pod, difficult to update pod config
* Object: a think that exists inside of a kubernetes cluster (Pod, Service, Deployment)
* Imperative Deployments: Request specific steps
* Declarative Deployments: Request base config and let kubernetes do the work
* Deployments: Kubernetes object meant to maintain a set of identical pods
* Service: routes traffic to pods, example

<img width="550" alt="Screen Shot 2022-07-05 at 3 21 11 PM" src="https://user-images.githubusercontent.com/19543073/177409696-e394c688-8665-40cb-816b-39b0d1e61899.png">

<img width="343" alt="Screen Shot 2022-07-05 at 3 33 19 PM" src="https://user-images.githubusercontent.com/19543073/177411542-4798b2c2-be0f-4ddc-91b9-8eb10beaf071.png">

<img width="811" alt="Screen Shot 2022-07-06 at 7 14 25 PM" src="https://user-images.githubusercontent.com/19543073/177663101-d5467224-f836-442a-85ac-5de344c5fe5e.png">

<img width="528" alt="Screen Shot 2022-07-06 at 7 16 25 PM" src="https://user-images.githubusercontent.com/19543073/177663248-7749a997-fffb-4bbf-be56-5384be9a2e0e.png">

<img width="481" alt="Screen Shot 2022-07-06 at 7 18 55 PM" src="https://user-images.githubusercontent.com/19543073/177663396-2ddcfd65-8d8b-4a9d-bff2-17ed8233b88c.png">

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
