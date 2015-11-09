#Getting started with config files.

>In addition to the imperative style commands described elsewhere, Kubernetes supports declarative YAML or JSON configuration files. Often times config files are preferable to imperative commands, since they can be checked into version control and changes to the files can be code reviewed, producing a more robust, reliable and archival system.

除了其他地方所描述的命令行式风格的命令以外，Kubernetes还支持以YAML或者JSON格式的配置方式，很多时候，配置文件是优于纯命令方式的，一旦他们能够被签入版本管理，文件的变化也能够像代码文件一样被回顾和管理，就能生产处更健壮，可靠和可存档系统。


##Running a container from a pod configuration file

$ cd kubernetes
$ kubectl create -f ./pod.yaml
Where pod.yaml contains something like:

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
You can see your cluster's pods:

$ kubectl get pods
and delete the pod you just created:

$ kubectl delete pods nginx

##Running a replicated set of containers from a configuration file

To run replicated containers, you need a Replication Controller. A replication controller is responsible for ensuring that a specific number of pods exist in the cluster.

$ cd kubernetes
$ kubectl create -f ./replication.yaml
Where replication.yaml contains:

apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
To delete the replication controller (and the pods it created):

$ kubectl delete rc nginx