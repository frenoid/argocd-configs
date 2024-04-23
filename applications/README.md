# ArgoCD applications
These files are [Application manifests](https://argo-cd.readthedocs.io/en/stable/core_concepts/) which define 
1. A group of Kubernetes resources
2. The application name
3. The source (i.e. the configuration coordinates)
4. The destination (i.e. where to deploy said resources)
5. etc.

An example of such a manifest can be found [here](/argocd-configs/argocd/examples/application.yaml)


## How to install an application
Simply use kubectl to apply the manifest like so<br>
`kubectl apply -f <appName>.yaml`<br>

The Sealed Secrets operator is required for most applications, if you have not done so, first [deploy Sealed Secrets](/argocd-configs/README.md#install-the-sealed-secrets-application)

## Source configuration
The application sources are predominantly helm charts found [here](https://github.com/frenoid/hobby-cluster/)