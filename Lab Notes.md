## Lab
---

### Table of contents (Udemy sections)
- [After Start the VM](#after-start-the-vm)
- [Section 3.19 : Pushing Docker Image (Demo)](#section-3.19) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6044424#overview)
- [Section 3.26 : Node Architecture](#section-3.26) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6058574#overview)
- [Section 3.27 : Replication Controller](#section-3.27) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6059038#overview)
- [Section 3.28 : Replication Controller (Demo)](#section-3.28) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6062916#overview)
- [Section 3.30 : Deployments (Demo)](#section-3.30) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6065908#overview)
- [Section 3.32 : Services (Demo)](#section-3.32) — [Udemy](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6076530#overview)
- [Quick health check (my part)](#quick-health-check)

---

<a id="after-start-the-vm"></a>
After Start the VM
------------------

#### ✅ What to do
- Start Minikube
- Verify `kubectl`
- Check which process is using a port (useful when Docker says port is already used)

#### Commands (copy/paste)
```bash
minikube start

# Better check (client only):
kubectl version --client
# (your note used: kubectl version)

# Check listening ports (who is using which port)
ss -tulnp
```

#### Notes (my part)
- If `minikube start` fails, run:
```bash
minikube status
minikube logs
```
- If you’re using Minikube with Docker driver, confirm Docker works:
```bash
docker ps
```

---

- If using port-forwarding, when u cancel the command or turn off the terminal, the port-forwarding will be shutdown too.

**Example**
```bash
kubectl port-forward pod/<pod-name> 8080:3000
# Ctrl+C stops port-forwarding
```

---
<a id="section-3.19"></a>
### [Section 3.19 : Pushing Docker Image (Demo)](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6044424#overview)

#### ✅ Steps (your commands)
- `docker login`
- `docker images`
- `docker tag  <image id> <your login>/<repo>`
- `docker push <your login>/<repo>`
- `docker push <your login>/<repo>:<tag>`

#### Copy/paste template (my part)
```bash
docker login
docker images

# tag format: username/repo:tag
docker tag <image-id> <your-dockerhub-username>/<repo-name>:<tag>

docker push <your-dockerhub-username>/<repo-name>:<tag>
```

**Example**
```bash
docker tag abc123 myname/helloworld:v1
docker push myname/helloworld:v1
```

---
<a id="section-3.26"></a>
### [Section 3.26 : Node Architecture](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6058574#overview)

#### Flow (your note)
- Internet -> Load Balancer -> iptables -> node (minikube) -> pod -> container -> Application

#### Terms (cleaned)

**Container**
- Running instance of the application
- Lives inside Pods

**Pods**
- Has own IP
- Kubernetes schedules pods

**Node**
- VM or Physical Machine
- Pods assigned to Node

**Cluster**
- Group of Nodes
- Managed by Control Plane

> #### Kubernetes (Cluster) manages Pods, Pods contain containers, containers run applications  

> #### Service route traffic to Pods

#### Extra (my part)
A good mental model:
- You deploy **Pods**
- Pods run **containers**
- A **Service** gives a stable way to reach Pods (because Pod IPs can change)

---
<a id="section-3.27"></a>
### [Section 3.27 : Replication Controller](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6059038#overview)

Stateless = application doesn't have a state

#### Extra (my part)
- RC’s job: **keep N pods running**
- If a pod dies → RC creates a new one
- In modern Kubernetes, you’ll usually use **Deployments/ReplicaSets**, but RC is still good for learning.

---
<a id="section-3.28"></a>
### [Section 3.28 : Replication Controller (Demo)](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6062916#overview)

##### ✅ Commands (your list)
- `kubectl get node`
- `cat kubernetes-course/replication-controller/helloworld-repl-controller.yml`
- `kubectl create -f kubernetes-course/replication-controller/helloworld-repl-controller.yml` : To create a new controller
- `kubectl get pods`
- `kubectl describe <pods>`
- `kubectl delete pod <pods>` : Try to delete a pod, then the replicator controller will create a new controller to replace the terminated pods
- `kubectl scale --replicas=4 -f <controller file name>` : Try to scale the pods from 2 to 4 using RC file name
- `kubectl get rc` : Detail of Replicator Controller
- `kubectl scale --replicas=1 rc/<RC Name>` : Try to scale the pods to 1 using RC name
- `kubectl delete rc/<RC Name>` : Delete the Replicator Controller using RC Name

#### Copy/paste “happy path” (my part)
```bash
# (common correction: use 'nodes' not 'node')
kubectl get nodes

cat kubernetes-course/replication-controller/helloworld-repl-controller.yml
kubectl create -f kubernetes-course/replication-controller/helloworld-repl-controller.yml

kubectl get pods
kubectl describe pod <pod-name>

# delete a pod (RC will recreate it)
kubectl delete pod <pod-name>
kubectl get pods

# scale replicas
kubectl scale --replicas=4 -f kubernetes-course/replication-controller/helloworld-repl-controller.yml
kubectl get rc

# scale by RC name
kubectl scale --replicas=1 rc/<rc-name>

# cleanup
kubectl delete rc/<rc-name>
```

---
<a id="section-3.30"></a>
### [Section 3.30 : Deployments (Demo)](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6065908#overview)

#### ✅ Commands (your list)

**Create + check**
- `cat kubernetes-course/deployment/helloworld.yml`
- `kubectl create -f deployment/helloworld.yml`
- `kubectl get deployments`
- `kubectl get rs` : To see the replica set that being used for the deployment
- `kubectl get pods`
- `kubectl get pods --show-labels` : To see the label, should be got the pod-template-hash
- `kubectl rollout status deployment/<deployment name>` : To check the status of the deployment

**Expose + test**
- `kubectl expose deployment <deployment name> --type=NodePort`
- `kubectl get service`
- `kubectl describe service <deployment name>`
- `minikube service <deployment name> --url`
- `curl <url>`

**Change Version**
- `kubectl decribe deployment <deployment name>`
- `kubectl set image deployment/<deployment name> <container name>=<image name : tag>`
- `kubectl rollout status deployment/<deployment name>`
- `curl <IP>`
- `kubectl get pods` : Will see that the old pods that using old image will be terminating and new pods using new image will be running
- `kubectl rollout history deployment/<deployment name>`
- `kubectl rollout undo deployment/<deployment name>`
- `kubectl rollout status deployment/<deployment name>`
- `kubectl get pods`
- `curl <IP>`
- `kubectl edit deployment/<deployment name>`
- `kubectl rollout undo deployment/<deployment name> --to-revision=<revision number>`
- `kubectl rollout history deployment/<deployment name>`

#### Copy/paste “happy path” (my part)
```bash
# create
kubectl create -f kubernetes-course/deployment/helloworld.yml
kubectl get deployments
kubectl get rs
kubectl get pods --show-labels
kubectl rollout status deployment/<deployment-name>

# expose
kubectl expose deployment <deployment-name> --type=NodePort
kubectl get svc
minikube service <service-name> --url
curl <url>

# update image (rolling)
kubectl set image deployment/<deployment-name> <container-name>=<image-name>:<tag>
kubectl rollout status deployment/<deployment-name>

# rollback if needed
kubectl rollout history deployment/<deployment-name>
kubectl rollout undo deployment/<deployment-name>
```

**Extra tip (my part):** avoid using `:latest` when learning—use `v1`, `v2` so you can control rollouts clearly.

---
<a id="section-3.32"></a>
### [Section 3.32 : Services (Demo)](https://www.udemy.com/course/learn-devops-the-complete-kubernetes-course/learn/lecture/6076530#overview)

If your helloworl app is not initialized yet

#### ✅ Commands (your list)
- `kubectl get node`
- `kubectl get pods`
- `kubectl create -f <file name>`
- `kubectl describe pod <pod name>`

**Create NodePort service**
- `cat first-app/helloworld-nodeport-service.yml`
- `cat first-app/helloworld.yml` : just to show the nodejs-port
- `kubectl create -f first-app/helloworld-nodeport-service.yml`
- `minikube service helloworld-service --url` : To see the port for the app url
- `kubectl describe service <service name>` : To get details of the service
- `kubectl get service` : To get summarize of the service
- `kubectl delete service <service name>`

#### Copy/paste “happy path” (my part)
```bash
# (common correction: use 'nodes' not 'node')
kubectl get nodes
kubectl get pods

# create service
kubectl create -f first-app/helloworld-nodeport-service.yml
kubectl get svc
kubectl describe svc helloworld-service

# open URL
minikube service helloworld-service --url
curl <url>

# cleanup
kubectl delete service helloworld-service
```

---
<a id="quick-health-check"></a>
### Quick health check (my part)

Run these when something feels “off”:
```bash
minikube status
kubectl get nodes
kubectl get pods -A
kubectl get svc -A
kubectl get events --sort-by=.metadata.creationTimestamp | tail -n 30
```
