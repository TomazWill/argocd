# ArgoCD

<br>

## Installing ArgoCD
- ### Option 1 - Installing ArgoCD (by manifests):
    ```sh
    kubectl create namespace argocd

    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    ```
- ### Option 2 - Installing ArgoCD (by Helm):
    ```sh
    git clone git@github.com:argoproj/argo-helm.git

    helm dependency up argo-helm/charts/argo-cd

    helm upgrade --install argo argo-helm/charts/argo-cd --values argo-helm/charts/argo-cd/values.yaml --namespace=argo --create-namespace
    ```

--- 
<br>


## Configure ArgoCD:
```sh
#### Installing ArgoCD CLI (by brew)
brew install argocd
#### To configure CLI access
argocd login --core
#### Login Using The CLI
argocd admin initial-password
#### Port Forwarding
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

--- 
<br>

## Application
```sh
### To Apply an Application:
argocd app create app-game --repo https://github.com/TomazWill/argocd.git --path k8s/game-2048 --dest-server https://kubernetes.default.svc --dest-namespace game
### To Delete an Application:
argocd app delete app-game

### To Enable 'Sync' for an Application:
argocd app set app-game --sync-policy auto
### To Disable 'Sync' for an Application:
argocd app set app-game --sync-policy none
### To Force an Application to 'Sync':
argocd app sync app-game

```

--- 
<br>

## Projects
> [Projects Docs](https://argo-cd.readthedocs.io/en/stable/user-guide/projects/)
```sh
### Creating Projects:
argocd proj create proj-game -d https://kubernetes.default.svc,game -s https://github.com/TomazWill/argocd.git

### Assign Application To A Project
argocd app set app-game --project proj-game

### Managing Projects Add or Remove 
argocd proj add-source <PROJECT> <REPO>
argocd proj remove-source <PROJECT> <REPO>
```