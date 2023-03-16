# 🔁 ArgoCD

<br>

## 🌀 Installing ArgoCD
- ### ☝️ Option 1 - Installing ArgoCD (by manifests):
    ```sh
    #### Creating namespace
    kubectl create namespace argocd
    #### Installing ArgoCD on namespace 'argocd'
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    #### Tuning services to LoadBalancer
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    #### Port Forwarding
    kubectl port-forward svc/argocd-server -n argocd 8181:443
    ```
- ### ✌️ Option 2 - Installing ArgoCD (by Helm):
    ```sh
    #### Add Helm Repository 'ArgoCD'
    helm repo add argo https://argoproj.github.io/argo-helm
    #### Install ArgoCD and create namespace 
    helm install sre-argocd argo/argo-cd --namespace argocd --create-namespace 
    #### Port Forwarding
    kubectl port-forward svc/sre-argocd-server 8090:80 -n argocd
    ```

--- 
<br>


## ⚙️ Configure ArgoCD:
```sh
#### Installing ArgoCD CLI (by brew)
brew install argocd
#### To configure CLI access
argocd login --core
#### Login Using The CLI
argocd admin initial-password
```

--- 
<br>

## 🚀 Running my apis (by ArgoCD)
```sh
### Aplicar os Projects
kubectl apply -f argocd/argocd-projects.yaml
### Aplicar os Applications
kubectl apply -f k8s/applications/game.yaml
kubectl apply -f k8s/applications/guestbook.yaml
```

--- 
<br>


## 💻 Argo CLI (Commands)
- ### Projects
    > #### [Docs - Projects](https://argo-cd.readthedocs.io/en/stable/user-guide/projects/)
    ```sh
    ### Creating Projects (before execute: kubectl config set-context --current --namespace=argocd):
    argocd proj create proj-game -d https://kubernetes.default.svc,game -s https://github.com/TomazWill/argocd.git

    ### Assign Application To A Project
    argocd app set app-game --project proj-game

    ### Managing Projects Add or Remove 
    argocd proj add-source <PROJECT> <REPO>
    argocd proj remove-source <PROJECT> <REPO>
    ```


- ### Application
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