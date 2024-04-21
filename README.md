# ArgoCD and Application configurations
This repo stores configurations to install ArgoCD and Applications

## To bootstrap ArgoCD
Have a working Kubernetes cluster, kubectl installed along with kubeconfig

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

> apiVersion: v1<br>
> kind: Secret<br>
> metadata:<br>
>   annotations:<br>
>     managed-by: argocd.argoproj.io <br>
>   labels: <br>
>     argocd.argoproj.io/secret-type: repository <br>
>   name: repo-secret-hobby-cluster <br>
>   namespace: argocd <br>
> type: Opaque <br>
> stringData: <br>
>   name: "kind-configs"<br>
>   project: "default"<br>
>   type: "git"<br>
>   url: "git@github.com:frenoid/hobby-cluster.git"<br>
>   sshPrivateKey: <replaceMe><br>


Replace the *<replaceMe>* with the private key used to access the github repo and apply the secret
`kubectl -nargocd apply -f argocd/repo-secret.yaml`


