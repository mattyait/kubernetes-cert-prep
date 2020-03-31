
###### The online exam consists of a set of performance-based items (problems) to be solved in a command line and candidates have 3 hours to complete the tasks.

> The Certification focuses on the skills required to be a successful Kubernetes Administrator in industry today. This includes these general domains and their weights on the exam:

      Core Concepts 19%
      Installation, Configuration & Validation 12%
      Application Lifecycle Management 8%      
      Networking 11%
      Scheduling 5%
      Security 12%
      Cluster Maintenance 11%
      Logging / Monitoring 5%
      Storage 7%
      Troubleshooting 10%

The cost is $300 and includes one free retake. For questions on the exam, please reach out to certificationsupport@cncf.io.

> Reference: https://www.cncf.io/certification/cka/

### Core Concepts
- Understand the Kubernetes API primitives.
- Understand the Kubernetes cluster architecture.
- Understand Services and other network primitives.

### Installation, Configuration & Validation
- Design a Kubernetes cluster.
- Install Kubernetes masters and nodes.
- Configure secure cluster communications.
- Configure a Highly-Available Kubernetes cluster.
- Know where to get the Kubernetes release binaries.
- Provision underlying infrastructure to deploy a Kubernetes cluster.
- Choose a network solution.
- Choose your Kubernetes infrastructure configuration.
- Run end-to-end tests on your cluster.
- Analyse end-to-end tests results.
- Run Node end-to-end tests.
- Install and use kubeadm to install, configure, and manage Kubernetes clusters

### Application Lifecycle Management

- Understand Deployments and how to perform rolling updates and rollbacks.
- Know various ways to configure applications.
- Know how to scale applications.
- Understand the primitives necessary to create a self-healing application.


### Networking
- Understand the networking configuration on the cluster nodes.
- Understand Pod networking concepts.
- Understand service networking.
- Deploy and configure network load balancer.
- Know how to use Ingress rules.
- Know how to configure and use the cluster DNS.
- Understand CNI.

### Scheduling
- Use label selectors to schedule Pods.
- Understand the role of DaemonSets.
- Understand how resource limits can affect Pod scheduling.
- Understand how to run multiple schedulers and how to configure Pods to use them.
- Manually schedule a pod without a scheduler.
- Display scheduler events.
- Know how to configure the Kubernetes scheduler.

### Cluster Maintenance
- Understand Kubernetes cluster upgrade process.
- Facilitate operating system upgrades.
- Implement backup and restore methodologies.

### Logging / Monitoring
- Understand how to monitor all cluster components.
- Understand how to monitor applications.
- Manage cluster component logs.
- Manage application logs.

### Storage
- Understand persistent volumes and know how to create them.
- Understand access modes for volumes.
- Understand persistent volume claims primitive.
- Understand Kubernetes storage objects.
- Know how to configure applications with persistent storage

### Troubleshooting
- Troubleshoot application failure.
- Troubleshoot control plane failure.
- Troubleshoot worker node failure.
- Troubleshoot networking.


## CKA Test Environment

> - Each task on this exam must be completed on a designated cluster/configuration context.
- To minimize switching, the tasks are grouped so that all questions on a given cluster appear
consecutively.
- There are six clusters (CKA) that comprise the exam environment, made up of varying numbers of containers, as follows:

| Cluster  | Members | CNI  | Description |
| -------- | ------- |------|-------------|
| k8s  | 1 master, 2 worker |flannel|k8s cluster
| hk8s | 1 master, 2 worker |calico|k8s cluster
| bk8s | 1 master, 1 worker |flannel|k8s cluster
| wk8s | 1 master, 2 worker |flannel|k8s cluster
| ek8s | 1 master, 2 worker |flannel|k8s cluster
| ik8s | 1 master, 1 base node|loopback|k8s cluster missing worker node|

## Reference

- [Candidate Handbook] https://training.linuxfoundation.org/wp-content/uploads/2020/03/CKA-CKAD-Candidate-Handbook-v1.10.pdf
- [Curriculum Overview] (https://github.com/cncf/curriculum)
- [Exam Tips] http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD
