# DevOps Kubernetes, Docker & Microservices Q&As   Java Success.com

## Table of Contents

- [Q1: What is the dif ference between Docker & Kubernetes?](#q1)
- [Q2: What are the key components of Kubernetes?](#q2)
- [Q3: Where will you use Docker?](#q3)
- [Q4: Where will you use Kubernetes?](#q4)

---

## Q1: What is the dif ference between Docker & Kubernetes?

**Answer:**

This is like comparing apples to oranges. Docker & Kubernetes can function without each other , and also both can compliment each other .
Docker container
Docker enables us to run, create and manage containers on a single operating system. If you have Docker installed on a number of hosts with same or dif ferent operating systems, you can use Docker compose, Docker swarm or Kubernetes to
automate container provisioning and networking. Docker on multiple hosts is required for high availability & scalability .
Kubernetes will automate container provisioning, networking, load-balancing, security and scaling up/down across all these nodes from a single command line or dashboard. A collection of nodes that is managed by a single Kubernetes
instance is referred to as a Kubernetes cluster .
So, Kubernetes is a container orchestration platform, whilst Docker enables us to have containers in the first place. Kubernetes can work with any containerization technology . Two of the most popular options that Kubernetes can integrate
with are rkt and Docker .

---

## Q2: What are the key components of Kubernetes?

**Answer:**

Node & Pod.

Kubernetes – Architecture
Nodes
There are two types of nodes.
Master Node
Master Node is where Kubernetes is installed. It is responsible for managing the entire Kubernetes cluster . You can have multiple master nodes for Kubernetes to be fault-tolerant. It controls the scheduling of pods across various worker nodes
(a.k.a just nodes). It consists of 4 components:
1) API Server: Interacts with W ebUI dashboards and command-line utility like kubeclt. These utilities are used by users to interact with the Kubernetes cluster .
The API server parses the Y AML configuration and stores the configuration in the etcd key value store.

2) Scheduler: decides how events and jobs to be scheduled across the cluster depending on the availability of resources, policy set by users, etc. It also listens on API Server for information about the state of the cluster .
3) Contr oller Manager: is responsible for considering the current state of the cluster as number of running Vs, idle pods and making decisions to achieve the desired state as to having fewer/more number of active pods instead. It listens on
API Server for information about the state of the cluster
4) etcd: is the key-value pair persistent store for the Kubernetes master nodes. It stores save policies, definitions, secrets, state of the system, etc.
Worker Node
Worker nodes are the nodes where the applications run. The worker nodes are controlled by the master node using the kubelet process. Container platforms like Docker Engine must be running on each worker node, and it works together with
the kubelet to run the containers.
1) kubelet: in each worker node relays the information about the health of the node back to the master as well as execute instructions given to it by master node.
2) kube-pr oxy: is a network proxy & load balancer that allow various microservices of your application to communicate with each other within the cluster . It cam also expose your application to the rest of the world (i.e. internet). Each pod
can talk to every other pod via this proxy .
3) Docker Engine: manages the containers.
Pods
1) A pod is a basic unit of deployment in Kubernetes. A pod consist of a group of 1 or more containers deployed to a single worker node. For example, A pod may have 2 containers – one for the web server , which may need to be deployed
with another container with redis caching server .
2) Each pod has a unique ip address within the cluster . Containers in a pod share the same ip address, host name, and the resources like CPU, memory , etc.
3) Containers within the same pod has access to the shared volumes.
4) Pods abstract network and storage away from the container , hence allowing you to move the containers more easily in the cluster .

---

## Q3: Where will you use Docker?

**Answer:**

If you are using a micr oservices based architecture you should definitely use Docker containers for each microservice.
Microservices is also known as the micr oservice ar chitectur e, which is an architectural style that structures an application as a collection of services that are highly maintainable and testable, loosely coupled, independently deployable , and
organized around business capabilities.
Docker is useful for building code pipelines. As the code travels from the developer ’s machine to production, there are many dif ferent environments it has to go through to get there. Docker provides a consistent environment for the application
from dev through production, easing the code development and deployment pipeline.
Docker gives you application isolation, and solves the dependency hell issues. For example, if you have two microservices servers, and both of which use flask. But, each of them uses a slightly dif ferent version of flask and other
dependencies. Running these API servers under dif ferent containers provides an easy way to avoid the “dependency hell” issue.
Docker provides “Multi-tenancy”. It is easy and inexpensive to create isolated environments for running multiple instances of application tiers for each tenant.

---

## Q4: Where will you use Kubernetes?

**Answer:**

Container -based microservices architectures have profoundly changed the way development and operations teams test and deploy modern software. Kubernetes is becoming the new standard for deploying and managing software in the
cloud – backed by key players like Google, A WS, Microsoft, IBM, Intel, Cisco, and Red Hat.
The microservice architectural style is an approach to developing an application as a suite of small independently deployable services based on specific business capabilities. A microservice approach is well suited to containers and Kubernetes
they provide – modularity , extensive parallelism, and auto scaling by deploying services across many nodes. Microservices modularity facilitates independent updates/deployments and helps to avoid single point of failure.
Kubernetes makes it easy to deploy and operate applications in a microservice architecture by creating an abstraction layer on top of a group of hosts, so that development teams can deploy their applications and let Kubernetes manage:
1) resource consumption by applications or team.
2) loads evenly across multiple host machines. Y ou can auto scale up or down on the cloud. For example, new hosts can be added amd made available.
3) automatic stopping of applications from consuming too many resources and restarting the applications again.
4) moving an application instance from one host to another if there is a shortage of resources in another host, or if a particular host dies.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
