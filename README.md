# Podman with Minikube

## macOS, Requirements

###### Using Homebrew:

- [Brew](https://brew.sh/)
- [Podman](https://podman.io/getting-started/installation)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
- [Kubectx](https://github.com/ahmetb/kubectx#homebrew-macos-and-linux)

```
brew install podman
brew install minikube
brew install kubectl
brew install kubectx
```

###### Using gcloud:

- [gcloud](https://cloud.google.com/sdk/docs/install)

```
gcloud components list
gcloud components install kubectl
```

## Podman

### Settings

```
~/.ssh/podman-machine-default
~/.ssh/podman-machine-default.pub
~/.config/containers/podman/machine/qemu/podman-machine-default.ign
~/.local/share/containers/podman/machine/qemu/podman-machine-default_fedora-coreos-35.20220227.2.0-qemu.x86_64.qcow2
~/.config/containers/podman/machine/qemu/podman-machine-default.json
```

### General

```
podman info
```

```
podman system connection list
podman system connection default podman-machine-default-root
podman network ls
```

### Machine

#### init
```
podman machine init --cpus 2 --memory 2048 --disk-size 20
podman machine init --cpus 4 --memory 4096 --disk-size 40
```

#### Root (Ports <1024):
```
podman machine set --rootful
```

#### Podman-managed VM (CLI):
- `podman machine start`
- `podman machine stop`
- `podman machine rm`
- `podman machine info`

---

## Minikube

Requires: `podman machine start`

### config

```
minikube config set driver podman
minikube config unset vm-drive
minikube config set cpus 2
```

Set: 
- `minikube config set driver podman`
- `minikube config set container-runtime containerd`

### minikube CLI:
- `minikube start`
- `minikube stop`
- `minikube delete`
- `minikube status`

#### start

```
minikube start --driver=podman --container-runtime=cri-o
minikube start --driver=podman --container-runtime=containerd
```

Shortened with config set above

```
minikube start
```

```
minikube start --listen-address=192.168.127.2
minikube start --cpus=2 --memory="4GB" --disk-size 20GB --wait=all --wait-timeout=20m0s --alsologtostderr
```

##### Example Log: `minikube start`

```
😄  minikube v1.28.0 on Darwin 12.6.1
✨  Using the podman (experimental) driver based on user configuration
📌  Using rootless Podman driver
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.25.3 preload ...
    > preloaded-images-k8s-v18-v1...:  638.52 MiB / 638.52 MiB  100.00% 15.72 M
    > gcr.io/k8s-minikube/kicbase:  386.27 MiB / 386.27 MiB  100.00% 9.19 MiB p
E1213 10:28:00.570672   38782 cache.go:203] Error downloading kic artifacts:  not yet implemented, see issue #8426
🔥  Creating podman container (CPUs=2, Memory=1969MB) ...
📦  Preparing Kubernetes v1.25.3 on containerd 1.6.9 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
❗  /Users/username/google-cloud-sdk/bin/kubectl is version 1.22.9-dispatcher-dirty, which may have incompatibilities with Kubernetes 1.25.3.
    ▪ Want kubectl v1.25.3? Try 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

---

## Kubectl

- `kubectx`
- `kubectx minikube`
- `kubectl get po -A`

##### Namespace: kube-system

```
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-565d847f94-gctlp           1/1     Running   0          13m
kube-system   etcd-minikube                      1/1     Running   0          14m
kube-system   kindnet-jrmmf                      1/1     Running   0          13m
kube-system   kube-apiserver-minikube            1/1     Running   0          14m
kube-system   kube-controller-manager-minikube   1/1     Running   0          14m
kube-system   kube-proxy-5tddm                   1/1     Running   0          13m
kube-system   kube-scheduler-minikube            1/1     Running   0          14m
kube-system   storage-provisioner                1/1     Running   0          14m
```
