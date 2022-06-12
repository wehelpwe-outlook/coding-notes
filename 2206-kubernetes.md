## What is Kubernetes
- Open source container orchestration tool
- Developed by Google
- Help you manage containerized applications in different environments (thousands in different machines)

## The need for a container orchestration tool
- Trend from Monolith to Microservices
- Increased usage of containers 
- Scripts is not possible to handle

## What features do orchestration tools offer ?
- High Availability or no downtime
- Scalability or high performance
- Disaster recovery - backup and restore 

## Kubernetes Basic Architecture 
- 1 Master node or many
    - API Server(api): Entrypoint to k8s cluster. UI,API,CLI tools talk to it. It is a container.
    - Controller Manager(c-m): keeps track of whats happening in the cluster
    - Schedular (sched): ensures Pods placement
    - (etcd): kubernetes backing key-value store,all status stored here
    - more important !
- n Worker nodes
    - each have **kubelet** run in it, as **node agent**  
    - run 1 to n dockers, with 1 to n containers
    - application runs in it 
    - better hardware, more resources
- Virtual Network: creates on unified machine

## Pods, Service, Ingress 
## Pods vs Node
## Pod
- Smallest unit of kubernetes
- Abstraction over container (you only interact with the Kubernetes layer)
- Usually 1 application per Pod 
- Each Pod(not container) gets its own IP address
- New IP on re-create
- eg: my-app talk to DB, DB die, new DB Pod replace, but the IP change, how to config, so the Service come

## Service and Ingress
- permanent IP
- lifecycle of Pod and Service NOT connected 
- Ingress: k8s internal DNS, interface to external network 

## Example: my-app talk to mongo-db
- Old way:
    - Database URL usually in the **bulid** application
    - DB URL change: my-app docker image re-build -> push it to repo -> pull it in your pod 
- K8s way:
    - **ConfigMap**: external configuration of your application
    > DB_URL = mongo-db

    - Don't put credential into ConfigMap, use **Secret**, used to store secret data, base64 encoded
    > DB_USER = mongo-user

    > DB_PWD = mongo-pwd
- Use **ConfigMap** and **Secret** as environment variables or as a properties file
- Summary: Pod, Service, Ingress, ConfigMap, Secret 

## Volumes
-  K8s use to persist data
- Can be on local machine, HD,
- Can be remote, outside of the K8s cluster
- think it as hard driver plugin to the K8s cluster, K8s doesn't manage data persistance! 

## Deployment and StatefulSet
- Case Study: user access my-app throught browser, now single my-app pod(container) is down, user can not reach my-app
- in K8s, Replicate everything
    - Node1: my-app + DB
    - Node2: my-app + DB
    - 2 my-app connect my-app **Service**
    - 2 DB connect DB **Service**
- 2 function of Service
    - permanent IP
    - load balancer
- Deployment: define blueprint for Pods
    - blueprint for my-app pods
    - you create Deployments
    - abstraction of Pods, used to replica and config each micro-service
- **DB can't be replicated via Deployment!**
    - DB has data, need mechanism to decide which DB pod is writing the database
- **StatefulSet** is used for DB
    - for STATEFUL apps: mongoDB,MySQL, Elastic Search
    - Deploying StatefulSet not easy
    - DB are often hosted outside of K8s cluster

## Main Kubernetes Components summarized 
- abstraction of container (Pod, Service)
- communication (Ingress)
- route traffic into cluster
- external configuration (ConfigMap, Secrets)
- data persistence (Volumes)
- pod blueprint mechanism (Deployment, StatefulSet)

## Minikube and Kubectl
## What is Minikube
- Minikube is an open source tool that allows you to set up a single-node Kubernetes cluster on your local machine. The cluster is run inside a virtual machine and includes Docker, allowing you to run containers inside the node.
- For test or learning
- Master and Node processes run on ONE machine
- Node has Docker pre-installed
- How minikube work:
    - creates Virtual Box on your laptop
    - Node runs in that Virtual Box
    - 1 Node K8s cluster
    - for testing purposes 
## What is kubectl: command line to for K8s cluster
-  Minikube run master processes and worker processed in one Node
- Master process -> API Server --> enables interaction with cluster
- 3 way to talk with API Server: UI, API, CLI(kubectl, most powerful)
- kubectl is used for both testing environment (minikube) and production environment(Cloud Cluster)

## Install Minikube 
- How to Install Minikube on Ubuntu 18.04 / 20.04 [https://phoenixnap.com/kb/install-minikube-on-ubuntu](https://phoenixnap.com/kb/install-minikube-on-ubuntu)

- follow the link to install Minikube
- verify the installation
> minikube version


    __$  minikube version
    minikube version: v1.25.2
    commit: 362d5fdc0a3dbee389b3d3f1034e8023e72bd3a7

> kubectl version -o json

    __$ kubectl version -o json
    {
      "clientVersion": {
        "major": "1",
        "minor": "24",
        "gitVersion": "v1.24.1",
        "gitCommit": "3ddd0f45aa91e2f30c70734b175631bec5b5825a",
        "gitTreeState": "clean",
        "buildDate": "2022-05-24T12:26:19Z",
        "goVersion": "go1.18.2",
        "compiler": "gc",
        "platform": "linux/amd64"
      },
      "kustomizeVersion": "v4.5.4",
      "serverVersion": {
        "major": "1",
        "minor": "23",
        "gitVersion": "v1.23.3",
        "gitCommit": "816c97ab8cff8a1c72eccca1026f7820e93e0d25",
        "gitTreeState": "clean",
        "buildDate": "2022-01-25T21:19:12Z",
        "goVersion": "go1.17.6",
        "compiler": "gc",
        "platform": "linux/amd64"
      }
    }

## Install successfully
## Minikube CLI: for start/stop/delete the cluster
> minikube start

> minikube stop

> minikube delete

> minikube status

> minikube ssh

> minikube addons list

> minikube dashboard // enable the Minikube dashboard

> minikube dashboard  --url // enable the Minikube dashboard

## Kubectl CLI: for configuring the Minikube cluster
> kubectl version -o json

> kubectl config view

> kubectl cluster-info

> kubectl get nodes

> kubectl get pods

> kubectl get pods --all-namespaces

## create 2 pod in minikube
1 nginx
>kubectl create deployment nginx-depl --image=nginx

>kubectl get pod 

2 mongo
>kubectl create deployment mongo-depl --image=mongo 

    ERROR:

    __$ kubectl get pod
    NAME                          READY   STATUS             RESTARTS      AGE
    mongo-depl-85ddc6d66-d4q4q    0/1     CrashLoopBackOff   3 (57s ago)   2m27s
    nginx-depl-5ddc44dd46-xmv6p   1/1     Running            0             153m
    __wfy@x301:~/Downloads>
    __$ kubectl logs mongo-depl-85ddc6d66-d4q4q

    WARNING: MongoDB 5.0+ requires a CPU with AVX support, and your current system does not appear to have that!
      see https://jira.mongodb.org/browse/SERVER-54407
      see also https://www.mongodb.com/community/forums/t/mongodb-5-0-cpu-intel-g4650-compatibility/116610/2
      see also https://github.com/docker-library/mongo/issues/485#issuecomment-891991814

    FIX by using old version mongo:4.4
    

>kubectl create deployment mongo-depl --image=mongo:4.4

>kubectl get pod

    __$ kubectl get pod
    NAME                          READY   STATUS    RESTARTS   AGE
    mongo-depl-75cf996768-2sb2m   1/1     Running   0          2m20s
    nginx-depl-5ddc44dd46-xmv6p   1/1     Running   0          3h1m

>kubectl describe pod mongo-depl-75cf996768-2sb2m   

## Delete deployment(-> pod -> container)
>kubectl delete deployment nginx-depl

>kubectl delete deployment mongo-depl

## Enter container to use shell
>kubectl exec -it mongo-depl-75cf996768-2sb2m -- bin/bas

## Using config file to create|update Deployment
>kubectl apply -f nginx-deployment.yaml

    __$ kubectl apply -f nginx-deployment.yaml 
    deployment.apps/nginx-deployment configured
    __$ kubectl get pod
    NAME                                READY   STATUS              RESTARTS   AGE
    nginx-deployment-7956bd8bb9-7mptc   0/1     ContainerCreating   0          3s
    nginx-deployment-7956bd8bb9-fj92j   1/1     Running             0          9m49s
    __$ kubectl get deployment
    NAME               READY   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   2/2     2            2           10m
    __$ kubectl get pod
    NAME                                READY   STATUS    RESTARTS   AGE
    nginx-deployment-7956bd8bb9-7mptc   1/1     Running   0          24s
    nginx-deployment-7956bd8bb9-fj92j   1/1     Running   0          10m


## Deployment config file

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
      labels:
         app: nginx
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:1.16
            ports:
            - containerPort: 80

## kubectl commands summary
- Create Commande
> kubectl create deployment [name] --image=imagename:tag
- Statue of k8s components
> kubectl get nodes | pod | services | replicaset | deployment
- Debugging pods
- Log to console
> kubectl logs [pod name]
- Get interactive terminal
> kubectl exec -it [pod name] -- bin/bash
- Get info about pod
> kubectl describe pod [pod name]
- Use configuration file for CRUD
> kubectl apply -f [file name]

> kubectl delete -f [file name]

## Case study: Deploying Mongo Express  and Mongo in minikube
- create pod : mongo
- create internal service: mongo-service
- create secret: mongo-user and mongo-pwd
- create pod: mongo-express
- create external service: monge-express-service
- create configmap: mongo-user,pwd, url
- map external service to outside ip
- **Using K8s Config file**

## Config file example
### mongo.yaml: mongodb-deployment and mongodb-service

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mongodb-deployment
      labels:
        app: mongodb
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mongodb
      template:
        metadata:
          labels:
            app: mongodb
        spec:
          containers:
          - name: mongodb
            image: mongo:4.4
            ports:
            - containerPort: 27017
            env:
            - name: MONGO_INITDB_ROOT_USERNAME
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-username
            - name: MONGO_INITDB_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-password
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: mongodb-service
    spec:
      selector:
        app: mongodb
      ports:
        - protocol: TCP
          port: 27017
          targetPort: 27017

### mongo-secret.yaml: mongodb-secret

    apiVersion: v1
    kind: Secret
    metadata:
      name: mongodb-secret
    type: Opaque
    data:
      mongo-root-username: dXNlcm5hbWU=
      mongo-root-password: cGFzc3dvcmQ=

- Generate Base64 credential

> echo -n "username" | base64
> echo -n "password" | base64

### mongo-express-yaml: mongo-express-deployment and mongo-express-service

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: mongo-express
      labels:
         app: mongo-express
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: mongo-express
      template:
        metadata:
          labels:
            app: mongo-express
        spec:
          containers:
          - name: mongo-express
            image: mongo-express:0.54
            ports:
            - containerPort: 8081
            env:
            - name: ME_CONFIG_MONGODB_ADMINUSERNAME  
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-username
            - name: ME_CONFIG_MONGODB_ADMINPASSWORD  
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-password
            - name: ME_CONFIG_MONGODB_SERVER        
              valueFrom:
                configMapKeyRef:
                  name: mongodb-configmap
                  key: database_url
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: mongo-express-service
    spec:
      selector:
        app: mongo-express
      type: LoadBalancer
      ports:
        - protocol: TCP
          port: 8081
          targetPort: 8081
          nodePort: 30000

### mongo-configmap.yaml: mongodb-configmap

    apiVersion: v1
    apiVersion: v1 kind: ConfigMap
    apiVersion: v1 
    kind: ConfigMap
    metadata:
      name: mongodb-configmap
    data:
      database_url: mongodb-service

## kubectl command line to deploying mongo and mongo-express
> kubectl apply -f mongo-secret.yaml

> kubectl get secret
---
> kubectl apply -f mongo.yaml

> kubectl get pod,service
---
> kubectl apply -f mongo-configmap.yaml

> kubectl get configmap
---
> kubectl apply -f mongo-express.yaml

> kubectl get pod,service
---
> kubectl get all

    __$ kubectl get all
    NAME                                      READY   STATUS    RESTARTS   AGE
    pod/mongo-express-64dfc49b47-94flz        1/1     Running   0          54m
    pod/mongodb-deployment-67fcd64675-q2q4z   1/1     Running   0          54m

    NAME                            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    service/kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP          22h
    service/mongo-express-service   LoadBalancer   10.103.100.84   <pending>     8081:30000/TCP   54m
    service/mongodb-service         ClusterIP      10.99.63.238    <none>        27017/TCP        54m

    NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/mongo-express        1/1     1            1           54m
    deployment.apps/mongodb-deployment   1/1     1            1           54m

    NAME                                            DESIRED   CURRENT   READY   AGE
    replicaset.apps/mongo-express-64dfc49b47        1         1         1       54m
    replicaset.apps/mongodb-deployment-67fcd64675   1         1         1       54m

## expose mongo-express-service to external(out k8s cluster)
> minikube service mongo-express-service 

> minikube service --all

    __$ minikube service mongo-express-service

    __$ minikube service --all
    * service default/kubernetes has no node port
    * service default/mongodb-service has no node port
    |-----------|-----------------------|--------------|-----------------------------|
    | NAMESPACE |         NAME          | TARGET PORT  |             URL             |
    |-----------|-----------------------|--------------|-----------------------------|
    | default   | kubernetes            | No node port |
    | default   | mongo-express-service |         8081 | http://192.168.59.100:30000 |
    | default   | mongodb-service       | No node port |
    |-----------|-----------------------|--------------|-----------------------------|

## expose mongo-express-service to ex-external(out minikube host(x301) <- k8s cluster)

> kubectl port-forward --address 0.0.0.0 service/mongo-express-service 8080:8081

- So the mongo-express service can be access through http://x301-ip:8080
- 8080 is the port map from mongo-express-service's port 8081