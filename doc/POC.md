# Proof of Concept (PoC) for AsciiArtify
## Deployment of ArgoCD on Kubernetes

### Objective
The objective of this Proof of Concept (PoC) is to demonstrate the technical feasibility of deploying ArgoCD, the recommended GitOps tool, on the chosen Kubernetes infrastructure (k3d) for the AsciiArtify project.

### Prerequisites
- A functioning k3d Kubernetes tool isntalled on the local machine;
- A web browser to access the ArgoCD user interface;

### Steps
1. Setting up the Kubernetes Cluster
```
    k3d cluster create argo
```
- Verify that the Kubernetes Cluster is up and running:
```
    kubectl get all -A             
```
2. Installation of ArgoCD
- Create a namespace
```
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- Verify that the ArgoCD components are successfully deployed:
```
    kubectl get pods -n argocd
```
3. Accessing the ArgoCD User Interface
- Set up port-forwarding running in the background:
```
    kubectl port-forward svc/argocd-server -n argocd 8080:443&
```
- Open a web browser and enter the following URL to access the ArgoCD user interface:
```
    https://localhost:8080
```
- Get password for Log in:
```
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
- Copy the output and paste the password in the ArgoCD user interface on the website:
``` 
    Username: admin
    Password: <ctrl+V>
```

4. Configuring GitOps Repositories
- In the ArgoCD user interface, click on "Settings" in the left sidebar.
- 
5. Deploying the AsciiArtify Application

6. Verifying the Deployment

