## Kustomize Exemplos
---
<br>

# Para mostrar como ficaria o arquivo final:
kubectl kustomize <pasta_do_kustomization>

# Para aplicar o arquivo final gerado pelo kustomize:
kubectl apply -k <pasta_do_kustomization>

# Para deletar o arquivo final gerado pelo kustomize:
kubectl delete -k <pasta_do_kustomization>