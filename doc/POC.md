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
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/setup_k3d_cluster_argo.gif)

- Copy the output and paste the password in the ArgoCD user interface on the website:
``` 
    Username: admin
    Password: <ctrl+V>
```
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/login.gif)

4. Configuring GitOps Repositories
- In the ArgoCD user interface, click on "Settings" in the left sidebar.
- Under the "Repositories" section, click on "Connect Repo" to add your Git repository containing the AsciiArtify application's configuration and mainfests.
- Provide the necessary information, such as the repository URL and Name.
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/settings.gif)

5. Deploying the AsciiArtify Application
- In the ArgoCD user interface, click on "Applications" in the left sidebar.
- Click on "New App" and fill in the required details, including the application name, the Git repository and path configured in the previous step.
- Select the Automatic synchronization and namespace creation.
- Review and confirm the application details, then click on "Create" to deploy the AsciiArtify application.
- ArgoCD will automatically synchronize the application with the desired state defined in the Git repository.
- Monitor the ArgoCD user interface to observe the deployment progress.

![](https://github.com/ng-n/AsciiArtify/blob/main/.data/settings.gif)

6. Verifying the Deployment
- Once the AsciiArtify application is successfully deployed, verify its status and ensure that the desired resources are created in the Kubernetes cluster:
For example, the resource called `ambassador`
```
    kubectl port-forward -n demo svc/ambassador 8088:80
```
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/ambassador_port_forward.gif)

- Open another terminal and verify it:
```
    curl -F 'image=@/tmp/g.png' localhost:8088/img/
```
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/curl_google_image.gif)

## Conclusion

By following the above steps, inclduing the port forwarding, you will be able to access the ArgoCD user interface through a local port on your machine. Please ensure that port 8080 is not occupied by any other process on your system. 

