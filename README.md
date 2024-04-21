# ArgoCD and Application configurations
This repo stores configurations to install
1. A kind cluster
2. Nginx Ingress controller
3. ArgoCD
4. Sealed Secrets
5. Other applications in [hobby-cluster](https://github.com/frenoid/hobby-cluster)

## Bootstrapping
We first begin with creating a Kubernetes cluster using kind. <br> 

### Create a kind cluster
You must have [docker](https://www.docker.com/) and [kind](https://kind.sigs.k8s.io/) installed.<br>

Then create a kind cluster<br>
`kind create cluster --config ./kind-cluster/cluster-config.yaml`

This creates 1 master node and 2 worker nodes using docker containers
Note that 1 of the worker notes has ports 30080 and 30443 exposed on the host machine for ingress<br>
You can see this by running<br>
`docker ps`
![Kind Cluster running in Docker](./kind-cluster/images/docker-ps-output.png "1 master 2 worker")<br>

### Install the Nginx Ingress Controller
Create a namespace for the nginx ingress controller<br>
`kubectl create ns ingress-nginx`<br>

Install nginx-ingress controller<br>
`kubectl -n ingress-nginx apply -f ./kind-cluster/ingress-nginx.yaml`<br>

Check that the ingress pod is running on worker 1<br>
`kubectl -n ingress-nginx get pod -owide`<br>
![Ingress pods are running](./kind-cluster/images/ingress-pod-node-location.png "Ingress pods are running")<br>

Test that the http ingress port is working <br>
`curl http://localhost:30080`<br>

Test that the https ingress port is working <br>
`curl https://localhost:30443 --insecure` <br>
![Ingress ports are working](./kind-cluster/images/testing-ingress-ports.png "Ingress ports 30080 and 30443 are responding")<br>

### Install ArgoCD
Create an argocd namespace<br>
`kubectl create ns argocd`

Install the ArgoCD resources<br>
`kubectl -nargocd apply -f argocd/install.yaml`

Install the ArgoCD ingress<br>
`kubectl -nargocd apply -f argocd/ingress.yaml`

Get the ArgoCD admin password<br>
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

Log into the ArgoCD at [http://argocd.mlnow.frenoid.com:30080/](http://argocd.mlnow.frenoid.com:30080/)<br>
Username: admin<br>
Password: <argoCDAdminPassword>

### Install the repository secret
Install the repo-secret for hobby-cluster

In [./argocd/repo-secret.yaml](./argocd/repo-secret.yaml), You will see <br>

```yaml
apiVersion: v1
kind: Secret
metadata:
  annotations:
    managed-by: argocd.argoproj.io
  labels:
    argocd.argoproj.io/secret-type: repository
  name: repo-secret-hobby-cluster
  namespace: argocd
type: Opaque
stringData:
  name: "kind-configs"
  project: "default"
  type: "git"
  url: "git@github.com:frenoid/hobby-cluster.git"
  sshPrivateKey: |
    <replaceMe>
```

Replace the `<replaceMe>` with the private key used to access the github repo and apply the secret <br>
`kubectl -nargocd apply -f argocd/repo-secret.yaml`

## Install the sealed-secrets application
The sealed-secrets application is needed to decrypt secrets for all other applications

### Install the application
`kubectl apply -f applications/sealed-secrets.yaml`<br>

Go to ArgoCD UI and you will see that the application is installed<br>
![Installed Sealed Secrets](./argocd/images/sealed-secrets-argocd-ui.png "ArgoCD UI showing Sealed Secrets installed")



