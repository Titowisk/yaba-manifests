# Yaba Project Manifests
This repository holds all manifests from Yaba project in order to use ArgoCD.

## Yaba-API

# How to Use

## Setup
- Start minikube: 
    - `minikube start`
- Apply the argocd manifest: 
    - `kubectl apply -f application.yaml`
- Forward the service to access the ArgoCD UI: 
    - `kubectl port-forward svc/argocd-server -n argocd 8080:443`
- Get the admin password
    - using argocd CLI: `argocd admin initial-password -n argocd`
    - using kubeclt CLI: `kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d`
- access the UI dashboard on 127.0.0.1:8080
    - username: admin
    - password: [admin_password]
- any changes to the manifests on this repository will be synchronized eventually
    - a webhook can be configured to apply changes quickly

## API
- Expose the `yaba-api-service` to access the api
    - `minikube service yaba-api-service`
- Use the URL to explore the API

### Optional - API
- Import Postman Collection and Environments to easily browse the defined API
    - check `/postman-collection` folder

# References
- TechWorld with Nana
    - https://www.youtube.com/watch?v=MeU5_k9ssrs (ArgoCD Tutorial for Beginners | GitOps CD for Kubernetes)
- Application.yaml official example
    - https://argo-cd.readthedocs.io/en/stable/operator-manual/application.yaml 
-

## How to test before pushing changes
I created another application.yaml for testing: `application-local.yaml`. It uses my current development branch to track changes by looking at
`source.targetRevision: feature/kafka-worker`. It set up a whole new application using the same k8s components.
With this, I can create another application in my argoCD cluster to test how new k8s components will behave with the existing ones.
I had to remove the original application inside minikube because running two application has become too much for docker desktop to handle.

So, to summarize:
- remove the previous application
- create another application file: `application-local.yaml`
    - change the necessary configuration to avoid conflicts, e.g: namespaces, names, etc.
    - set it up to track the current development branch in `source.targetRevision`
- apply the new application file
- test
- remove the test application and apply the main application file

> argocd commands for some reason only works when I: kubectl config set-context --current --namespace argocd
