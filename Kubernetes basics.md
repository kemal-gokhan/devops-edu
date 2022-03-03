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

**ClusterIp exposure < NodePort exposure < LoadBalancer exposure**

- **ClusterIp**  
  Expose service through **k8s cluster** with `ip/name:port`
- **NodePort**  
  Expose service through **Internal network VM's** also external to k8s `ip/name:port`
- **LoadBalancer**  
  Expose service through **External world** or whatever you defined in your LB.

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

### Env

--pirntenv

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
```

### Port forward

`kubectl port-forward pod/<name> 8080:80` (host-container)

### Volume

EmptyDir:     file share . restart container ok delete pod notok

Hostpath: 

Valid whlie pod processing

### Secret ~~ Config Map

kind: Secret

namespaces should be same place.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-dockercfg
type: kubernetes.io/dockercfg
data:
  .dockercfg: |
        "<base64 encoded ~/.dockercfg file>"

or

type: Opaque
stringData:
  db_server: db.example.com
  db_username: admin
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secretpodvolume
spec:
  containers:

- name: secretcontainer
  image: ozgurozturknet/k8s:blue
  volumeMounts:
  - name: secret-vol
    mountPath: /secret
    volumes:
- name: secret-vol
  secret:
    secretName: mysecret3

---

apiVersion: v1
kind: Pod
metadata:
  name: secretpodenv
spec:
  containers:

- name: secretcontainer
  image: ozgurozturknet/k8s:blue
  env:
  - name: username
    valueFrom:
      secretKeyRef:

        name: mysecret3
        key: db_username
  - name: password
    valueFrom:
      secretKeyRef:

        name: mysecret3
        key: db_password
  - name: server
    valueFrom:
      secretKeyRef:

        name: mysecret3
        key: db_server

---

apiVersion: v1
kind: Pod
metadata:
  name: secretpodenvall
spec:
  containers:

- name: secretcontainer
  image: ozgurozturknet/k8s:blue
  envFrom:
  - secretRef:
      name: mysecret3
```

### Node Affinity

Affinity rules like vmware.

```
apiVersion: v1
kind: Pod
metadata:
  name: nodeaffinitypod1
spec:
  containers:
  - name: nodeaffinity1
    image: ozgurozturknet/k8s
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: app
            operator: In #In, NotIn, Exists, DoesNotExist
            values:
            - blue
---
apiVersion: v1
kind: Pod
metadata:
  name: nodeaffinitypod2
spec:
  containers:
  - name: nodeaffinity2
    image: ozgurozturknet/k8s
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: app
            operator: In
            values:
            - blue
      - weight: 2
        preference:
          matchExpressions:
          - key: app
            operator: In
            values:
            - red
---
apiVersion: v1
kind: Pod
metadata:
  name: nodeaffinitypod3
spec:
  containers:
  - name: nodeaffinity3
    image: ozgurozturknet/k8s
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: app
            operator: Exists #In, NotIn, Exists, DoesNotExist
```

### Pod Affinity

```
apiVersion: v1
kind: Pod
metadata:
  name: frontendpod
  labels:
    app: frontend
    deployment: test
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
---
apiVersion: v1
kind: Pod
metadata:
  name: cachepod
spec:
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - frontend
        topologyKey: kubernetes.io/hostname
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: color
              operator: In
              values:
              - blue
          topologyKey: kubernetes.io/hostname
    podAntiAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          labelSelector:
            matchExpressions:
            - key: deployment
              operator: In
              values:
              - prod
          topologyKey: topology.kubernetes.io/zone
  containers:
  - name: cachecontainer
    image: redis:6-alpine
```

### Taint Toleration

Not Affinity, tell schedule to work this node or not.

```
apiVersion: v1
kind: Pod
metadata:
  name: toleratedpod1
  labels:
    env: test
spec:
  containers:

- name: toleratedcontainer1
  image: ozgurozturknet/k8s
  tolerations:
- key: "platform"
  operator: "Equal"
  value: "production"
  effect: "NoSchedule"

---

apiVersion: v1
kind: Pod
metadata:
  name: toleratedpod2
  labels:
    env: test
spec:
  containers:

- name: toleratedcontainer2
  image: ozgurozturknet/k8s
  tolerations:
- key: "platform"
  operator: "Exists"
  effect: "NoSchedule"
```

### DaemonSet

Like deployment, you can create for logging containers. Each pods, each nodes.

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: logdaemonset
  labels:
    app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      # this toleration is to have the daemonset runnable on master nodes
      # remove it if your masters can't run pods
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

### Persistent Volume

CSI (container storage interface)

PV and PVC

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysqlclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: ""
  selector:
    matchLabels:
      app: mysql

apiVersion: v1
kind: PersistentVolume
metadata:
   name: mysqlpv
   labels:
     app: mysql
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  nfs:
    path: /
    server: 10.255.255.10

#Deployment    
apiVersion: v1
kind: Secret
metadata:
  name: mysqlsecret
type: Opaque
stringData:
  password: P@ssw0rd!
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysqldeployment
  labels:
    app: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          ports:
            - containerPort: 3306
          volumeMounts:
            - mountPath: "/var/lib/mysql"
              name: mysqlvolume
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysqlsecret
                  key: password
      volumes:
        - name: mysqlvolume
          persistentVolumeClaim:
            claimName: mysqlclaim
```

### Storage Class

Create dynamic storage volume.

`kubectl get storageclass`

check provisioner.

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standarddisk
parameters:
  cachingmode: ReadOnly
  kind: Managed
  storageaccounttype: StandardSSD_LRS
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

PVC

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysqlclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: "standarddisk"
```

  Deployment

```
apiVersion: v1
kind: Secret
metadata:
name: mysqlsecret
type: Opaque
stringData:
password: P@ssw0rd!
---
apiVersion: apps/v1
kind: Deployment
metadata:
name: mysqldeployment
labels:
  app: mysql
spec:
replicas: 1
selector:
  matchLabels:
    app: mysql
strategy:
  type: Recreate
template:
  metadata:
    labels:
      app: mysql
  spec:
    containers:
      - name: mysql
        image: mysql
        ports:
          - containerPort: 3306
        volumeMounts:
          - mountPath: "/var/lib/mysql"
            name: mysqlvolume
        env:
          - name: MYSQL_ROOT_PASSWORD
            valueFrom:
              secretKeyRef:
                name: mysqlsecret
                key: password
    volumes:
      - name: mysqlvolume
        persistentVolumeClaim:
          claimName: mysqlclaim
```

### Stateful Set

Each pod has each PV. Creating and deleting in sequence.

```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: cassandra
  name: cassandra
spec:
  clusterIP: None
  ports:
  - port: 9042
  selector:
    app: cassandra
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: cassandra
  labels:
    app: cassandra
spec:
  serviceName: cassandra
  replicas: 3
  selector:
    matchLabels:
      app: cassandra
  template:
    metadata:
      labels:
        app: cassandra
    spec:
      terminationGracePeriodSeconds: 1800
      containers:
      - name: cassandra
        image: gcr.io/google-samples/cassandra:v13
        imagePullPolicy: Always
        ports:
        - containerPort: 7000
          name: intra-node
        - containerPort: 7001
          name: tls-intra-node
        - containerPort: 7199
          name: jmx
        - containerPort: 9042
          name: cql
        resources:
          limits:
            cpu: "500m"
            memory: 1Gi
          requests:
            cpu: "500m"
            memory: 1Gi
        securityContext:
          capabilities:
            add:
              - IPC_LOCK
        lifecycle:
          preStop:
            exec:
              command: 
              - /bin/sh
              - -c
              - nodetool drain
        env:
          - name: MAX_HEAP_SIZE
            value: 512M
          - name: HEAP_NEWSIZE
            value: 100M
          - name: CASSANDRA_SEEDS
            value: "cassandra-0.cassandra.default.svc.cluster.local"
          - name: CASSANDRA_CLUSTER_NAME
            value: "K8Demo"
          - name: CASSANDRA_DC
            value: "DC1-K8Demo"
          - name: CASSANDRA_RACK
            value: "Rack1-K8Demo"
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
        readinessProbe:
          exec:
            command:
            - /bin/bash
            - -c
            - /ready-probe.sh
          initialDelaySeconds: 15
          timeoutSeconds: 5
        volumeMounts:
        - name: cassandra-data
          mountPath: /cassandra_data
  volumeClaimTemplates:
  - metadata:
      name: cassandra-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: standard
      resources:
        requests:
          storage: 1Gi
```

### Job & CronJob

Attention-> restartPolicy

Jobs provide to create certain pods within schedule/completion.

```
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  parallelism: 2
  completions: 10
  backoffLimit: 5
  activeDeadlineSeconds: 100
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never #OnFailure 
```

Cron

```
apiVersion: batch/v1beta1 # not stable until kubernetes 1.21.
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
#
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * *
```

### Authentication

Developer:
`openssl genrsa -out <keyname.key> 2048`  -----> `openssl req -new -key <keyname.key> -out <csrname.csr> -subj "/CN=username/0=TeamName"`

Last step

kubectl config set-credentials username --client-certificate=<crtname.crt> --client-key=<keyname.key>

Admin:

CSR

`kubectl get csr`

```
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: ozgurozturk
spec:
  groups:
  - system:authenticated
  request: $(cat ozgurozturk.csr | base64 | tr -d "\n")
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF
```

`kubectl certificate approve ozgurozturk`

`kubectl get csr ozgurozturk -o jsonpath='{.status.certificate}' | base64 -d >> ozgurozturk.crt`

`kubectl config set-context ozgurozturk-context --cluster=minikube --user=ozgur@ozgurozturk.net`

### Role Based Access Control

-Role Binding 

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: ozgur@ozgurozturk.net # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io
```

-Role

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"] # "services", "endpoints", "pods", "pods/log" etc.
  verbs: ["get", "watch", "list"] # "get", "list", "watch", "post", "put", "create", "update", "patch", "delete"
```

-Cluster RB

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-secrets-global
subjects:
- kind: Group
  name: DevTeam # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: secret-reader
  apiGroup: rbac.authorization.k8s.io
```

-Cluster role

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: secret-reader
rules:
- apiGroups: [""]
  resources: ["secrets"]
  verbs: ["get", "watch", "list"]
```

### Service Account

You can assign role, cluster role.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: testsa
  namespace: default
---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: podread
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: testsarolebinding
  namespace: default
subjects:
- kind: ServiceAccount
  name: testsa
  apiGroup: ""
roleRef:
  kind: Role
  name: podread
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: v1
kind: Pod
metadata:
  name: testpod
  namespace: default
spec:
  serviceAccountName: testsa
  containers:
  - name: testcontainer
    image: ozgurozturknet/k8s:latest
    ports:
    - containerPort: 80
```

### Ingress

off topic: `minikube addons list`
`kubectl get all -n ingess-nginx`

Ingress controller. Nginx controller.
L7 controll to distribute traffic.

3 deployment, 3  services

kind: Ingress
You can deploy many ingress.

kubectl get ingress

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: appingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
    - host: k8sfundamentals.com
      http:
        paths:
          - path: /blue
            pathType: Prefix
            backend:
              service:
                name: bluesvc
                port:
                  number: 80
          - path: /green
            pathType: Prefix
            backend:
              service:
                name: greensvc
                port
                  number: 80
```

### Dashboard

Kubernetes dashboard (github: https://github.com/kubernetes/dashboard )
3rd party: Lens https://k8slens.dev/
kinvolk 

### Repository

`
kubectl create secret docker-registry "secret_ismi" --docker-server="registry_url" --docker-username="kullanıcı_adı" --docker-password="şifre"
`

```
apiVersion: v1
kind: Pod
metadata:
  name: imagesecrettest2
  labels:
    app: imagesecret
spec:
  containers:
  - name: imagesecretcontainer
    image: ozgurozturkregistry.azurecr.io/k8s:latest
    imagePullPolicy: Always # IfNotPresent, Never
    # If the tag is latest, k8s defaults imagePullPolicy to Always. Otherwise k8s defaults imagePullPolicy to IfNotPresent
    ports:
    - containerPort: 80
  imagePullSecrets:
  - name: regscrt
```

### Static Pod

  https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/

  without kubectl command, you can create pod or whatever you want at /etc/kubernetes/manifest

  allow network with -ipBlock, - namespaceSelector, - podSelector

for using network policy with minikube
`minikube start --cpus 4 --memory 6144 --cni=calico --container-runtime=docker --host-only-cidr=172.17.17.1/24` 

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
 name: networkpolicy-example
 namespace: ns-a
spec:
 podSelector:
   matchLabels:
     team: a
 policyTypes:
 - Ingress
 - Egress
 ingress:
 - from:
   - ipBlock:
       cidr: 10.11.0.0/16
       except:
       - 10.11.1.0/24
   - namespaceSelector:
       matchLabels:
         team: b
   - podSelector:
       matchLabels:
         app: frontend
   ports:
   - protocol: TCP
     port: 80
 egress:
 - to:
   - ipBlock:
       cidr: 1.1.1.1/32
   ports:
   - protocol: TCP
     port: 80
```



### Helm

Package manager for kubernetes.

Chart=Package --> Kubernetes = Release

artifact.hub

### Prometheus

Metrics.
Monitoring with Grafana

-- Prometheus Kubernetes Stack: https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

-- Prometheus Kubernetes Operator: https://prometheus-operator.dev/

### EFK (ELASTIC SEARCH FLUENTD KIBANA)
Alternative: ELK (logstash)
Logs
Kibana is for visualization.

### Istio

Provides service mesh.
