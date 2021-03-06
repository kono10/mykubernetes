# mykubernetes
repo for my kubernetes notes

<img width="824" alt="Screen Shot 2022-07-05 at 3 20 15 PM" src="https://user-images.githubusercontent.com/19543073/177409563-aa3a262e-74fe-4283-8097-ab1eec14c3fc.png">

Command Summary

`kubectl describe <object type> <object name>`
* prints out info about the config, can use it to check the image that is being used in the pod

`kubectl delete -f <config file>`
`kubectl delete <object type> <object name>`
* delete a kubernetes object

`kubectl get pods -o wide`
* get an extended list of pod info

`kubectl get services -o wide`
`kubectl get deployments -o wide`

```
❯ kubectl get pods -o wide
NAME                                 READY   STATUS    RESTARTS   AGE    IP          NODE             NOMINATED NODE   READINESS GATES
django-deployment-7fcf74dd4f-w58t8   1/1     Running   0          5m7s   10.1.0.15   docker-desktop   <none>           <none>
```

`kubectl get secrets`


-> Pod ip address inside of the node, as nodes are added and deleted the ip will change, node-port service will have a static ip address that will make our app easy to access via a web browser.

## K8s Summary

<img width="545" alt="Screen Shot 2022-07-16 at 7 47 32 AM" src="https://user-images.githubusercontent.com/19543073/179355492-d134ddaf-09ca-42bd-8f00-4b4da9587cf0.png">

* Nodes: collection of Pods/Deployments
* Master: Passes commands to Nodes
* Cluster: Master + Nodes
* Kubectl: program used for interacting with nodes in a cluster
* Pod: tightly grouped containers with a common purpose, in kubernetes cannot run a container alone a container must have a pod, difficult to update pod config
* Object: a thing that exists inside of a kubernetes cluster (Pod, Service, Deployment)
* Imperative Deployments: Request specific steps, ex: run a kubectl command to update the version of a container in an already running pod
* Declarative Deployments: Request base config and let kubernetes do the work
* Controller: any object that constantly works to make a desired state a reality (deployments)
* Deployments: Kubernetes object meant to maintain a set of identical pods
* Secrets: Stores a piece of information in the cluster such as a database password, or the secret key for a django app. Manually created via an imperative command


<img width="991" alt="Screen Shot 2022-07-13 at 4 36 12 PM" src="https://user-images.githubusercontent.com/19543073/178840906-04f38b19-ecfa-474f-891d-1bdfffffd016.png">

## Services
* Service: routes traffic to pods, example
* ClusterIP Service: exposes a set of pods to other objects in the cluster, no outside world
* NodeportIP Services: exposes a set of pods to the outside world, only good for dev
* LoadBalancer: Legacy way of getting network traffic into a cluster, only allows access to one specific set of pods.
* Ingress Service: Exposes a set of services to the outside world. Can route requests to multiple services.
  * Nginx -> community led project endorsed by kubernetes, setup will change depending on the environment (local, google cloud etc.). Ingress controller will create an nginx pod that handles routing. Nginx will bypass cluster IP and go directly to a pod. Cluster IP will keep track of pods.
  * kubernetes -> project led by the company nginx

## Volumes, Pv's, and PVC's

* Volume: in kubernetes, an object that allow a container to store data at the pod level. Volume can be accessed by any container inside the pod.
* Persistent Volume: Durable storage that is not tied to a pod or container. Similar to a docker volume. Database solution for k8s. If a container/pod ever crashes all the data inside that container would be lost! Once a new pod starts none of the old data gets carried over. The file system for postgres is housed outside the deployment so if it crashes the data does not get lost. 
* Persistent Volume Claim: options available to your pod for storage, in a cluster could have pv's created ahead of time (statically provisioned pv), also have an option that can be created on the fly (dynamically provisioned pv)
* PVC is an advertisement of optionvs PV are the options

<img width="703" alt="Screen Shot 2022-07-13 at 3 46 18 PM" src="https://user-images.githubusercontent.com/19543073/178831249-081af41a-bdf8-40fb-8c94-fed890dfeaec.png">

<img width="550" alt="Screen Shot 2022-07-05 at 3 21 11 PM" src="https://user-images.githubusercontent.com/19543073/177409696-e394c688-8665-40cb-816b-39b0d1e61899.png">

<img width="343" alt="Screen Shot 2022-07-05 at 3 33 19 PM" src="https://user-images.githubusercontent.com/19543073/177411542-4798b2c2-be0f-4ddc-91b9-8eb10beaf071.png">

<img width="811" alt="Screen Shot 2022-07-06 at 7 14 25 PM" src="https://user-images.githubusercontent.com/19543073/177663101-d5467224-f836-442a-85ac-5de344c5fe5e.png">

<img width="528" alt="Screen Shot 2022-07-06 at 7 16 25 PM" src="https://user-images.githubusercontent.com/19543073/177663248-7749a997-fffb-4bbf-be56-5384be9a2e0e.png">

<img width="481" alt="Screen Shot 2022-07-06 at 7 18 55 PM" src="https://user-images.githubusercontent.com/19543073/177663396-2ddcfd65-8d8b-4a9d-bff2-17ed8233b88c.png">

<img width="504" alt="Screen Shot 2022-07-06 at 7 57 23 PM" src="https://user-images.githubusercontent.com/19543073/177667069-b664e3d5-5c05-4ea5-9946-c3f6c61e52a0.png">

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
Scale up the number of pods
* change the replicas line in the *deployments.yml file
* these replicas are all managed by the designated deployment
```
❯ kubectl apply -f django-app-deployment.yml
deployment.apps/django-deployment configured
❯ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
django-deployment-7fcf74dd4f-2khss   1/1     Running   0          2m55s
django-deployment-7fcf74dd4f-2rrc8   1/1     Running   0          2m55s
django-deployment-7fcf74dd4f-66lh5   1/1     Running   0          2m55s
django-deployment-7fcf74dd4f-7v987   1/1     Running   0          5s
django-deployment-7fcf74dd4f-qltlr   1/1     Running   0          5s
django-deployment-7fcf74dd4f-t2cft   1/1     Running   0          2m55s
django-deployment-7fcf74dd4f-w58t8   1/1     Running   0          24h
❯ kubectl get deployments
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
django-deployment   7/7     7            7           24h
```

## Get log info from a pod

```
❯ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
django-deployment-7fcf74dd4f-6hntd   1/1     Running   0          20h
django-deployment-7fcf74dd4f-6w6vx   1/1     Running   0          20h
django-deployment-7fcf74dd4f-8rwpv   1/1     Running   0          20h

❯ kubectl logs django-deployment-7fcf74dd4f-6hntd
[2022-07-12 23:45:19 +0000] [1] [INFO] Starting gunicorn 20.1.0
[2022-07-12 23:45:19 +0000] [1] [INFO] Listening at: http://0.0.0.0:8000 (1)
[2022-07-12 23:45:19 +0000] [1] [INFO] Using worker: sync
[2022-07-12 23:45:19 +0000] [9] [INFO] Booting worker with pid: 9
[2022-07-12 23:45:19 +0000] [10] [INFO] Booting worker with pid: 10
[2022-07-12 23:45:19 +0000] [11] [INFO] Booting worker with pid: 11
[2022-07-12 23:45:19 +0000] [13] [INFO] Booting worker with pid: 13
[2022-07-13 00:11:32 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:10)
[2022-07-13 00:11:32 +0000] [10] [INFO] Worker exiting (pid: 10)
[2022-07-13 00:11:32 +0000] [17] [INFO] Booting worker with pid: 17
[2022-07-13 02:11:51 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:9)
[2022-07-13 02:11:51 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:11)
[2022-07-13 02:11:51 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:13)
[2022-07-13 02:11:51 +0000] [13] [INFO] Worker exiting (pid: 13)
[2022-07-13 02:11:51 +0000] [11] [INFO] Worker exiting (pid: 11)
[2022-07-13 02:11:46 +0000] [9] [INFO] Worker exiting (pid: 9)
[2022-07-13 02:11:46 +0000] [1] [WARNING] Worker with pid 9 was terminated due to signal 9
[2022-07-13 02:11:46 +0000] [19] [INFO] Booting worker with pid: 19
[2022-07-13 02:11:46 +0000] [20] [INFO] Booting worker with pid: 20
[2022-07-13 02:11:46 +0000] [1] [WARNING] Worker with pid 13 was terminated due to signal 9
[2022-07-13 02:11:46 +0000] [21] [INFO] Booting worker with pid: 21
[2022-07-13 02:45:07 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:17)
[2022-07-13 02:45:07 +0000] [17] [INFO] Worker exiting (pid: 17)
[2022-07-13 02:45:06 +0000] [25] [INFO] Booting worker with pid: 25
[2022-07-13 20:04:04 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:19)
[2022-07-13 20:04:04 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:20)
[2022-07-13 20:04:04 +0000] [1] [CRITICAL] WORKER TIMEOUT (pid:21)
[2022-07-13 20:04:04 +0000] [21] [INFO] Worker exiting (pid: 21)
[2022-07-13 20:04:04 +0000] [19] [INFO] Worker exiting (pid: 19)
[2022-07-13 20:04:05 +0000] [1] [WARNING] Worker with pid 19 was terminated due to signal 9
[2022-07-13 20:04:05 +0000] [27] [INFO] Booting worker with pid: 27
[2022-07-13 20:04:05 +0000] [28] [INFO] Booting worker with pid: 28
[2022-07-13 20:04:05 +0000] [1] [WARNING] Worker with pid 20 was terminated due to signal 9
[2022-07-13 20:04:05 +0000] [29] [INFO] Booting worker with pid: 29
```
### Can condense configs into single file by seperating configs for different objects by a '---'


### Production ingress structure

<img width="474" alt="Screen Shot 2022-07-16 at 7 51 45 AM" src="https://user-images.githubusercontent.com/19543073/179355649-274af288-21d3-4b45-b410-564ffa71337a.png">


