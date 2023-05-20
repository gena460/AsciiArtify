
# Access The Argo CD API Server

## Prerequisites

- k3d installed on your local machine
- kubectl installed on your local machine

## Installation

1. Create a k3d cluster by running the following command:

   ```
   k3d cluster create argo
   ```

2. Install the ArgoCD by running the following command:

   ```
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

   ```

3. Wait for the ArgoCD deployment to complete.

4. Check pods status

   ```
   kubectl get po -n argocd -w
   ```

5. To access the ArgoCD web interface, run the following command to expose the service:

   ```
   kubectl port-forward svc/argocd-server -n argocd 8088:443&
   ```

6. Open a web browser and navigate to `https://localhost:8080`. You should now be able to access the ArgoCD graphical interface.

## Accessing the ArgoCD Interface

![alt text](argocd.gif)


1. Open a web browser and navigate to the ArgoCD URL.

2. On the login screen, enter your username and password and click "Login".

3. Once logged in, you will be directed to the ArgoCD dashboard.

4. In the dashboard, you can view the status of your applications and repositories, as well as manage your clusters, settings, and user accounts.

5. To view the details of a specific application or repository, click on its name in the dashboard.

6. From the application or repository details page, you can view its current state, configuration, and deployment history.

7. To make changes to an application or repository, you can use the ArgoCD interface to modify its configuration, sync its state, or trigger a deployment.

8. If you encounter any issues while using the ArgoCD interface or command-line tool, you can refer to the ArgoCD documentation or contact your administrator for assistance.

Note: The ArgoCD installation and configuration process may differ depending on your setup and environment.