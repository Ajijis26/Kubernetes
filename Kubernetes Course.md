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

 

===



### Lab



After Start the VM

\- Start the minikube

 	= minikube start

\- Check kubectl is okey

 	= kubectl version

\- Check either docker is running

 	= ss -tulnp

 	= If don't have localhost at 3000

 	= docker run -p 3000:3000 -t \[docker image id]



\- If using port-forwarding, when u cancel the command or turn off the terminal, the port-forwarding will be shutdown too.



---



###### [Section3.19 : Demo:-Pushing Docker Image](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6044424#notes)



* docker login
* docker images
* docker tag  <image id> <your login>/<repo>
* docker push <your login>/<repo>
* docker push <your login>/<repo>:<tag>



---



###### Section 3.26 : Node Architecture



* Internet -> Load Balancer -> iptables -> node (minikube) -> pod -> container -> Application



**Container**

* Container = Running instance of the application
* Lives inside Pods



**Pods**

* Has own IP
* Kubernetes schedule pods



**Node**

* VM or Physical Machine
* Pods assigned to Node



**Cluster**

* Group of Nodes
* Managed by Control Plane



\# Kubernetes (Cluster) manages Pods, Pods contain containers, containers run applications

\# Service route traffic to Pods



---



###### Section 3.27 : Replication Controller



Stateless = application doesn't have a state



---



###### Section 3.28 : Demo:-Replication Controller



* kubectl get node
* cat kubernetes-course/replication-controller/helloworld-repl-controller.yml
* kubectl create -f kubernetes-course/replication-controller/helloworld-repl-controller.yml : To create a new controller
* kubectl get pods
* kubectl describe <pods>
* kubectl delete pod <pods> : Try to delete a pod, then the controller will create a new controller to replace the terminated pods
* kubectl scale --replicas=4 -f <controller file name> : Try to scale the pods from 2 to 4 using RC file name
* kubectl get rc : Detail of Replicator Controller
* kubectl scale --replicas=1 rc/<RC Name> : Try to scale the pods to 1 using RC name
* kubectl delete rc/<RC Name> : Delete the Replicator Controller using RC Name



---



###### [Section 3.30 : Demo:-Deployments](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6065908#notes)



* cat kubernetes-course/deployment/helloworld.yml
* kubectl create -f deployment/helloworld.yml
* kubectl get deployments
* kubectl get rs : To see the replica set that being used for the deployment
* kubectl get pods
* kubectl get pods --show-labels : To see the label, should be got the pod-template-hash
* kubectl rollout status deployment/<deployment name> : To check the status of the deployment



* kubectl expose deployment <deployment name> --type=NodePort
* kubectl get service
* kubectl describe service <deployment name>
* minikube service <deployment name> --url
* curl <url>



**Change Version**

* kubectl decribe deployment <deployment name>
* kubectl set image deployment/<deployment name> <container name>=<image name : tag>
* kubectl rollout status deployment/<deployment name>
* curl <IP>
* kubectl get pods : Will see that the old pods that using the old iamge will be terminating and new pods using new image will be running
* kubectl rollout history deployment/<deployment name>
* kubectl rollout undo deployment/<deployment name>
* kubectl rollout status deployment/<deployment name>
* kubectl get pods
* curl <IP>
* kubectl edit deployment/<deployment name>
* kubectl rollout undo deployment/<deployment name> --to-revision=<revision number>
* kubectl rollout history deployment/<deployment name>



---



###### [Section 3.32 : Demo:-Services](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6076530#notes)



If your helloworl app is not initialized yet

* kubectl get node
* kubectl get pods
* kubectl create -f <file name>
* kubectl describe pod <pod name>



* cat first-app/helloworld-nodeport-service.yml
* cat first-app/helloworld.yml : just to show the nodejs-port
* kubectl create -f first-app/helloworld-nodeport-service.yml
* minikube service helloworld-service --url : To see the port for the app url
* kubectl describe service <service name> : To get details of the service
* kubectl get service : To get summarize of the service
* kubectl delete service <service name>
