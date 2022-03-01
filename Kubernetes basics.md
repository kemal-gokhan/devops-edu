# Kubernetes Basics

There are two main parts of kubernetes which are master and node terms.

$Home/.kube = config file

`kubectl explain <object>`

`kubectl config get-context`

`kubectl config current-context`

`kubectl config use-context <contextname>`

`kubectl config set-context --current --namespace=<desirednamespace>`

kubectl verb object (general rule)

spesific code run: `get pods -n <namespace>`

### COMPONENTS

**etcd**: key-value

**API server**: ---- connected to kubelet

**Controller**: brain

**kubelet**: agent

**container runtime**: ex. docker (rkt-cri-o)

**scheduler**: distribute work or container

`kubectl version`

`kubectl run nginx`

`kubectl cluster-info`

`kubectl get nodes`

 `kubectl get pods`

`kubectl describe pod <podname>`

`kubectl logs <podname>`

`kubectl delete pod <podname>`

`kubectl run redisapp --image redis`

kubernetes create pods instead of images or smthing. Pods can contains more images nevertheless it is not recommended.

`kubectl run nginx --image=nginx`

`kubectl get pods -o wide` (more common easy to read)

`kubectl get nodes -o wide` (more common easy to read)

`kubectl edit pods <podname>` (after that automatically control configuration order)

`kubectl exec <podname> -- <command>`

`kubectl exec -it <podname> -- /bin/sh`

`kubectl exec -it <podname> -c <containername> -- /bin/sh` if it has many containers, you should use -c parameter. These containers have same IP. 

Creating Pod:

etcd-> kubelet-->image-->

### YAML

You need to know yaml format to write declerative commands to kubernetes.

All type of file XML, JSON and YAML switch each other.

Key Value Pair - Array  List - Dictionary Map

### PODS WITH YAML

VSCode Yaml Extension with Kubernetes output. 

```yaml
apiversion: v1

kind: Pod

metadata:

  name: myapp

  labels:

    name: myapp

spec:

  containers:

  - name: myapp

    image: <Image>

    resources:

      limits:

        memory: "128Mi"

        cpu: "500m"

    ports:

      - containerPort: <Port>
```

### REPLICATION

RP  controlls all nodes.

RP Controller(kind: ReplicationController) is older, RP Set is newer.

`kubectl get replicatiset`

``RP Controller:

--!--

apiVersion: v1

kind: ReplicationController

metadata:

  name: myapp

spec:

  replicas: <Replicas>

  selector:

    app: myapp

  template:

    metadata:

      name: myapp

      labels:

        app: myapp

    spec:

      containers:

        - name: myapp

          image: <Image>

          ports:

            - containerPort: <Port>

--!--

RP Set:

--!--

```yaml
apiversion: apps/v1

kind: ReplicaSet

metadata:

  name: myapp

spec:

  replicas: <Replicas>

  selector: ##difference between controller and set

    app: myapp

    **matchLabels:**

      **type: front-end**

  template:

    metadata:

      name: myapp

      labels:

        app: myapp

    spec:

      containers:

        - name: myapp

          image: <Image>

          ports:

            - containerPort: <Port>
```

RSet: desired state

Deployment: apply rs. Deployment creates Replica Set.

--!--

init container: before you start main application, it could be prerequirement that you have to run. Therefore you can use this and should be mentioned initContainers: on spec.

---: keep going.

### Label

key-value

`kubectl get pods -l "<key>" --show-labes`

`kubectl get pods -l "<key=values>" --show-labes`

`kubectl label pods <podname> <key=value>`

nodeSelector:

  hddtype: ssd

annotations: on metada "all infos not related k8s"

`  kubectl annotate pods <podname> <key=value>`

### Namespaces

`kubectl get namespaces`

kube-system kube-node-lease kube-public default

`kubectl create namespaces <namespacename>`

# Deployment

`kubectl get deployments`

`kubectl set image deployment/first nginx=httpd` (change image nginx to httpd in depyloyment/first)

Deployment and Replica set is same on yaml. It's better to create deployment yaml because there are many undo types.

--!--

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:

  name: myapp

spec:

  replicas: 3

  strategy:

    type: Recreate

  selector:

    matchLabels:

      app: myapp

  template:

    metadata:

      labels:

        app: myapp

    spec:

      containers:

      - name: myapp

        image: <Image>

        resources:

          limits:

            memory: "128Mi"

            cpu: "500m"

        ports:

        - containerPort: <Port>
```

--!--

### Rollout & Rollback

`rollout history deployment <deploymentname>`

`rollout undo deployment <deploymentname> --to-revision=1`

`kubectl rollout status deployment <deploymentname>`

spec:

  replicas: 3

  **strategy**:

**    type: Recreate** /all pods deleted immediately

**    type: RollingUpdate** /in sequence delete and create

**    rollingUpdate: 

**      maxUnavailable: 2

**      maxSurge: 2

**    type: Recreate** /all pods deleted immediately    

`kubectl  rollout undo deployment <deploymentname>`

# Container Network Interface

### [Calico](https://kubernetes.io/docs/concepts/cluster-administration/networking/#calico)

### [Flannel](https://kubernetes.io/docs/concepts/cluster-administration/networking/#flannel)

### Service

Services create endpoint. 

`kubectl get endpoints `

**ClusterIP**

--service-cluster-ip-range 10.0.0.0/16

**NodePort**

    Proxy

**LoadBalancer**

****    Cloud Provider 

**ExternalName**

****

--!--

```yaml
apiVersion: v1

kind: apiVersion: v1

kind: Service

metadata:

  name: myapp

spec:

  type: ClusterIP

  selector:

    app: myapp

  ports:

  - port: <Port>

    targetPort: <Target Port>
```

--!--

**Livenessprobe**: It provides that your application working well thanks to livenessprobe key.

tcpSocket is another key.

--!--

```yaml
apiVersion: v1

kind: Pod

metadata:

  labels:

    test: liveness

  name: liveness-exec

spec:

  containers:

  - name: liveness

    image: k8s.gcr.io/busybox

    args:

    - /bin/sh

    - -c

    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600

    livenessProbe:

      exec:

        command:

        - cat

        - /tmp/healthy

      initialDelaySeconds: 5

      periodSeconds: 5
```

--!--

**Readinessprobe**:  After the check it will add services.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: goproxy
  labels:
    app: goproxy
spec:
  containers:
  - name: goproxy
    image: k8s.gcr.io/goproxy:0.1
    ports:
    - containerPort: 8080
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```
