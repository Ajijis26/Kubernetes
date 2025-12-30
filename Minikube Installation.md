# Install Minikube on Ubuntu (Docker Driver) — Steps 1 to 5

This guide stores **Steps 1 to 5** for installing **Minikube on Ubuntu** using the **Docker driver** (the most common and beginner-friendly setup).  
You will install: prerequisites → Docker → kubectl → Minikube → start the cluster.

---

## 1) Update packages + install prerequisites

### What this does
- Updates your local package index so Ubuntu knows about the latest package versions.
- Installs basic tools used to download and verify software securely.

### Commands
```bash
sudo apt update
sudo apt install -y curl wget ca-certificates gnupg
```

---

## 2) Install Docker (recommended Minikube driver)

### What this does
Minikube needs a “driver” to run Kubernetes nodes. With the **Docker driver**, Minikube runs Kubernetes inside Docker containers (no need for VirtualBox).

### Commands
```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

### Explanation
- `docker.io` installs Docker from Ubuntu’s repository.
- `systemctl enable --now docker` starts Docker now and on every boot.
- `usermod -aG docker $USER` allows your current user to run Docker **without** `sudo`.

### Apply the Docker group permission
After adding yourself to the `docker` group, you must refresh your session:

Choose **one**:
```bash
newgrp docker
```
or **log out and log in** again.

### Quick Docker test
```bash
docker run hello-world
```
If this runs successfully, Docker is working.

---

## 3) Install kubectl

### What this does
`kubectl` is the command-line tool used to interact with Kubernetes (view pods, deploy apps, check nodes, etc.).

### Commands
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### Explanation
- Downloads the latest stable `kubectl` binary.
- Installs it into `/usr/local/bin` so it’s available as a system command.
- Verifies it by printing the client version.

---

## 4) Install Minikube

### What this does
Minikube is the tool that creates a **local Kubernetes cluster** on your machine.

### Commands
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
```

### Explanation
- Downloads the latest Minikube binary.
- Installs it into `/usr/local/bin`.
- Verifies installation by printing Minikube’s version.

---

## 5) Start your Kubernetes cluster

### What this does
This creates a local Kubernetes cluster using Docker as the driver.

### Commands
```bash
minikube start --driver=docker
minikube status
kubectl get nodes
```

### Explanation
- `minikube start --driver=docker` boots the cluster.
- `minikube status` confirms the cluster components are running.
- `kubectl get nodes` verifies Kubernetes sees at least one node in a **Ready** state.

---

## (Optional) Useful extras after Step 5

### Enable metrics-server (for resource metrics)
```bash
minikube addons enable metrics-server
```

### Open Kubernetes Dashboard
```bash
minikube dashboard
```

---

## Common notes (quick troubleshooting)

### If Docker permission is denied
You likely didn’t refresh your session after adding your user to the Docker group. Run:
```bash
newgrp docker
```
or log out and log in again.

### Check what driver Minikube is using
```bash
minikube profile list
minikube status
```

---

**Done ✅** You now have Minikube + kubectl installed and a local Kubernetes cluster running.
