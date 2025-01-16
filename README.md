# Yaba Project Manifests
This repository holds all manifests from Yaba project in order to use ArgoCD.

## Yaba-API

# How to Use
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
- any changes to the manifests on this repository will be synchronized eventually
    - a webhook can be configured to apply changes quickly


# References
- TechWorld with Nana
    - https://www.youtube.com/watch?v=MeU5_k9ssrs (ArgoCD Tutorial for Beginners | GitOps CD for Kubernetes)
- Application.yaml official example
    - https://argo-cd.readthedocs.io/en/stable/operator-manual/application.yaml 
-

