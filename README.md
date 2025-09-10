# k8s-estudos-gitops

Este repositório faz parte de um conjunto de estudos sobre Kubernetes (K8s), ArgoCD, Keycloak e GitHub. O objetivo é aprender e demonstrar práticas de GitOps para automação de deploys e gerenciamento de infraestrutura.

## Repositórios relacionados

- [k8s-estudos-frontend](https://github.com/tadeubdev/k8s-estudos-frontend)
- [k8s-estudos-backend](https://github.com/tadeubdev/k8s-estudos-backend)

## Tecnologias utilizadas

- Kubernetes
- ArgoCD
- Keycloak
- GitHub

## Sobre o projeto

Este repositório contém os manifestos e configurações para orquestrar o deploy das aplicações frontend e backend utilizando GitOps.

# Para executar localmente

1. Clone o repositório:
```bash
git clone https://github.com/tadeubdev/k8s-estudos.git
```

2. Navegue até o diretório do projeto:
```bash
cd k8s-estudos/gitops
```

3. Instalar ingress-nginx:
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

4. Rode:
```bash
kind create cluster --config base/kind-config.yaml
kubectl apply -k overlays/homolog
kubectl apply -k overlays/prod
```

5. Instale o ArgoCD:
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

6. Pegue a senha admin do ArgoCD:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

7. Acesse a interface web do ArgoCD:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

8. Instale o ArgoCD:
```bash
kubectl apply -f argocd-app-homolog.yaml -n argocd
kubectl apply -f argocd-app-prod.yaml -n argocd
```