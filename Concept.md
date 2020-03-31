# Kubernetes
Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications.

# Kubernetes Arch

![KubernetesArch](../docs/images/kubernetes_arc.jpg)

## UI
You can use Dashboard to deploy containerized applications to a Kubernetes cluster,
troubleshoot your containerized application, and manage the cluster resources.
You can use Dashboard to get an overview of applications running on your cluster, as well as for creating or modifying individual Kubernetes resources

## Kubectl
kubectl controls the Kubernetes cluster manager. It's Cli for running commands to manage the cluster.

## Pods
Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.

## kubelet
The kubelet is the primary `node agent` that runs on each node. The kubelet works in terms of a PodSpec.
A PodSpec is a `YAML` or `JSON` object that describes a pod.
The kubelet takes a set of PodSpecs that are provided through various mechanisms (primarily through the apiserver) and ensures that the containers described in those PodSpecs are running and healthy.
The kubelet doesn’t manage containers which were not created by Kubernetes

## Kubernetes Master
The Kubernetes master is responsible for maintaining the desired state for your cluster.
When you interact with Kubernetes, such as by using the kubectl command-line interface, you’re communicating with your cluster’s Kubernetes master.
The “master” refers to a collection of processes managing the cluster state. Those processes are: kube-apiserver, kube-controller-manager, kube-scheduler and etcd.
Typically these processes are all run on a single node in the cluster, and this node is also referred to as the master.
The master can also be replicated for availability and redundancy.

- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd

## Kubernetes Nodes
The nodes in a cluster are the machines (VMs, physical servers, etc) that run your applications and cloud workflows.
The Kubernetes master controls each node; you’ll rarely interact with nodes directly.

## Understand the Kubernetes API primitives
##### Common Components

Below is the sample template while creating any `.yml` file for kubernetes component, it usually include `metadata header`

    kind: KIND
    apiVersion: VERSION
    metadata:
      name: NAME

##### Containers

Below is the template to define a container with specific image want to use in any pod. Containers are always embedded in a prod spec.

> Below example is using `nginx image` for container and exposing the port 80.

    name: nginx
    image: nginx:1.10
    ports:
    - containerPort: 80

##### Pods
Pods are the smallest deployable units of computing that contains one or more containers.

    kind: Pod
    apiVersion: v1
    metadata:
      name: pod-example
      namespace: k8s-namespace
      labels:
        app: sample-app
    spec:
      containers:
      - name: first-container
        image: busybox
        command: ["echo"]
        args: ["First Container"]
      - name: second-container
        image: nginx
        ports:
          - containerPort: 80

#### Services
A service is a logical group of pods together with a policy for accessing them. It is a networking abstraction that allows the containers in a pod to be accessed via DNS entries and TCP or UDP channels.

    kind: Service
    apiVersion: v1
    metadata:
      name: service-name
    spec:
      ports:
      - name: http
        port: 80
        targetPort: 80
      - name: https
        port: 443
        targetPort: 443
      selector:
        app: sample-app    

#### Volumes
A volume is a local filesystem which is accessible to the containers in a pods as though it were present in a local directory. The lifetime of a volume is tied to the lifetime of the pod it is attached to.

    apiVersion: v1
    kind: Pod
    metadata:
      name: pod-example
    spec:
      containers:
      - image: nginx
        name: test-container
        volumeMounts:
        - mountPath: /nginx/logs
          name: cache-volume
      volumes:
      - name: cache-volume
        emptyDir: {}           

#### Deployments

Deployments manage a group of replicated pods which use the same spec. They can be scaled up and down on demand.

Deployment internally uses ReplicaSets and ReplicationControllers to maintain the correct status of pods.

- ReplicaSets
> A ReplicaSet’s purpose is to maintain a stable set of replica Pods running at any given time

- ReplicationControllers
> A ReplicationController ensures that a specified number of pod replicas are running at any one time


    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
      labels:
        app: nginx
    spec:
      replicas: 3
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
            image: nginx
            ports:
            - containerPort: 80

#### StatefulSets

StatefulSet is used to manage stateful applications.
It manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

They provide persistent network identifiers, persistent storage, and all deployments and rolling updates also occur in order.

> A StatefulSet cannot exist without an accompanying Service            

    apiVersion: v1
    kind: Service
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      ports:
      - port: 80
        name: web
      clusterIP: None
      selector:
        app: nginx

    ---

    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      selector:
        matchLabels:
          app: nginx
      serviceName: "nginx"
      replicas: 2
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:latest
            ports:
            - containerPort: 80
              name: web

#### DaemonSet

It ensures that nodes always run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some Use case for DaemonSet are
- running a logs collection daemon (`fluentd or filebeat`) on every node
- running a node monitoring daemon (`Prometheus`) on every node


    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: fluentd-elasticsearch
      namespace: kube-system
      labels:
        k8s-app: fluentd-logging
    spec:
      selector:
        matchLabels:
          name: fluentd-elasticsearch
      template:
        metadata:
          labels:
            name: fluentd-elasticsearch
        spec:
          nodeSelector:      # Selects nodes to run this pod on.
            env: prod
          containers:
          - name: fluentd-elasticsearch
            image: k8s.gcr.io/fluentd-elasticsearch:1.20
            resources:
              limits:
                memory: 200Mi
              requests:
                cpu: 100m
                memory: 200Mi

#### Job

A Job is a temporary deployment which Kubernetes will start and ensure that a required number of them successfully terminate.

    apiVersion: batch/v1
    kind: Job
    metadata:
      name: pi
    spec:
      template:
        spec:
          containers:
          - name: pi
            image: perl
            command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
          restartPolicy: Never
      backoffLimit: 4
