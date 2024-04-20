# ArgoCD and Application configurations
This repo stores configurations to install ArgoCD and Applications

# To bootstrap ArgoCD
Have a working Kubernetes cluster, kubectl installed along with kubeconfig

Create an argocd namespace
`kubectl create ns argocd`

Install the ArgoCD resources
`kubectl -nargocd apply -f argocd/install.yaml`

Install the ArgoCD ingress
`kubectl -nargocd apply -f argocd/ingress.yaml`

Get the ArgoCD admin password
`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

Log into the ArgoCD at [http://argocd.mlnow.frenoid.com:30080/](http://argocd.mlnow.frenoid.com:30080/)
Username: admin
Password: <argoCDAdminPassword>



