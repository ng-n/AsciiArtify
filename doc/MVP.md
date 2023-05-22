# AsciiArtify MVP Development using ArgoCD
## Objective
The objective of this deployment is to set up the AsciiArtify Minimum Viable Product (MVP) by leveraging ArgoCD for continuous deployment and synchronization with a Git repository.

## Prerequisites
- A functioning k3d Kubernetes custer with ArgoCD already deployed (following the previous [steps](https://github.com/ng-n/AsciiArtify/blob/main/doc/POC.md))
- Familiarity with Git and the AsciiArtify application repository: https://github.com/den-vasyliev/go-demo-app

## Steps
1. Configuring GitOps Repositories
- In the ArgoCD user interface, click on "Settings" in the left sidebar.
- Under the "Repositories" section, click on "Connect Repo" to add your Git repository containing the AsciiArtify application's configuration and mainfests.
- Provide the necessary information, such as the repository URL and Name.
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/settings.gif)

2. Deploying an Application in ArgoCD nd Enable Automatic Synchronization
- In the ArgoCD user interface, click on "Applications" in the left sidebar.
- Click on "New App" and fill in the required details, including the application name, the Git repository and path configured in the previous step.
- Select the Automatic synchronization and namespace creation, and choose "helm" in the "Path" field.
- Review and confirm the application details, then click on "Create" to deploy the AsciiArtify application.
- ArgoCD will automatically synchronize the application with the desired state defined in the Git repository.
- Monitor the ArgoCD user interface to observe the deployment progress.

![](https://github.com/ng-n/AsciiArtify/blob/main/.data/settings.gif)

3. Verifying the Deployment
- Once the AsciiArtify application is successfully deployed, verify its status and ensure that the desired resources are created in the Kubernetes cluster:
For example, the resource called `ambassador`
```
    kubectl port-forward -n demo svc/ambassador 8088:80
```
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/ambassador_port_forward.gif)

- Open another terminal and verify it:
```
    wget -O /tmp/g.png https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png
    curl -F 'image=@/tmp/g.png' localhost:8088/img/
```
![](https://github.com/ng-n/AsciiArtify/blob/main/.data/curl_google_image.gif)

## Conclusion

By following the outlined steps, the AsciiArtify MVP will be deployed and continuously synchronized using ArgoCD. This setup allows for easy management and updates of the application based on changes pushed to the Git repository.
