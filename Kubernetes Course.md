# [Kubernetes Course](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6121104#overview)



- Container

- Pod

- Service



---



Docker → builds images

Minikube → runs Kubernetes locally

kubectl → controls Kubernetes



---



“Do Docker and Kubernetes do the same thing?”

❌ No.

Docker → runs one container

Kubernetes → manages many containers



---



### [Docker](https://docs.docker.com/engine/install/)

 Basic Command :

- docker build .
- docker images
- docker container ls

---



### [**Minikube**](https://minikube.sigs.k8s.io/docs/)

Kubernetes in a single local machine (locally)

* Runs single node Kubernetes cluster inside a Linux VM
* For test and development only.
* Works on Windows, Linux and MacOS
* Basic Command :
```bash
 	Minikube Start \& Stop

 	- minikube start

 	- minikube stop

 	- minikube delete

 	- minikube start -p <cluster name>

 	Cluster Status

 	- minikube status

 	- minikube ip

 	- minikube version

 	Accessing Kubernetes Dashboard

 	- minikube dashboard

 	Minikube Resources

 	- minikube profile list

 	- minikube logs

 	Minikube Addons

 	- minikube addons enable dashboard

 	- minikube addons disable dashboard

 	- minikube addons list

 	Working with Kubernetes and Kubectl

 	- kubectl config use-context minikube

 	- kubectl cluster-info

 	- kubectl get nodes

 	- kubectl get pods

 	Minkube Configuration \& Setting

 	- minikube config set memory 4096

 	- minikube config set cpus 2

 	- minikube config view

 	Minikube \& Docker

 	- eval $(minikube -p minikube docker-env)

 	- docker run <container>

 	Networking \& Services

 	- minikube service <service-name> --url

 	- kubectl port-forward svc/<service-name> <local port>:<target port of the application>
```
---



If not using package manager,

### [Kubectl](https://kubernetes.io/docs/tasks/tools/)

Command-line client (remote control for Kubernetes)

* Basic Command :

 	Cluster Information

 	- kubectl cluster-info

 	- kubectl version

 	Managing Pods

 	- kubectl get pods

 	- kubectl get pods -n <namespace>

 	- lubectl describe pod <pod-name>

 	- kubectl delete pod <pod-name>

 	Managing Deployment

 	- kubectl get deployments

 	- kubectl apply -f <deployment.yaml>

 	- kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>

 	- kubectl delete deployment <deployment-name>

 	- kubectl describe deployment <deployment-name>

 	Managing Services

 	- kubectl get svc

 	- kubectl describe svc <service-name>

 	- kubectl expose pod <pod-name> --port=80 --target-port=8080

 	Managing Namespaces

 	- kubectl get namespaces

 	- kubectl create namespace <namespace-name>

 	- kubectl config set-context --current --namespace=<namespace-name>

 	Logs \& Troubleshooting

 	- kubectl logs <pod-name>

 	- kubectl logs <pod-name> -c <container-name>

 	- kubectl logs -f <pod-name>

 	- kubectl exec -it <pod-name> -- /bin/bash

 	Configuring \& Managing Resources

 	- kubectl get all

 	- kubectl describve <resource-type> <resource-name>

 	- kubectl apply -f <file.yaml>

 	- kubectl delete -f <file.yaml>

 	Managing ConfigMaps \& secrets

 	- kubectl get configmaps

 	- kubectl crate configmap <configmap-name> --from-literal=<key>=<value>

 	- kubectl get secrets

 	Port Forwarding

 	- kubectl port-forward pod/<pod-name> <local-port>:<pod-port>

 	- kubectl port-forward svc/<service-name> <local-port>:<service-port>

 	Kubectl Context \& Configurations

 	- kubectl config current-context

 	- kubectl config get-contexts

 	- kubectl config use-context <context-name.

  	Advanced kubectl Command

 	- kubectl run -I --tty busybox --image=busybox --restart=Never -- sh

 	- kubectl top pod

 	- kubectl scale deployment <deployment-name> --replicas=3

