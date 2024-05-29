# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
В этом уроке установили Argo CD с Helm и настроили его так, чтобы он мог управлять собой. Это позволяет нам обновлять ArgoCD: путем изменения манифеста argo-cd.yaml внутри репозитория Git.
Cоздали корневое приложение root-app.yaml , которое использует шаблон «приложение из приложений» для декларативного управления нашими приложениями.
Создали манифесты приложении kubernetes-network.yaml, kubernetes-templating.yaml c заданными параметрами.

## Как запустить проект:
  helm repo add argo-cd https://argoproj.github.io/argo-helm
  helm dep update kubernetes-gitops/charts/argo-cd
  helm install argo-cd kubernetes-gitops/charts/argo-cd
  
  helm template root-app/ | kubectl apply -f -

    git add charts/root-app/templates/agro-cd.yaml
    git commit -m 'add app agro-cd.yaml'
    git push

    git add charts/root-app/templates/kubernetes-network.yaml
    git commit -m 'add app kubernetes-network.yaml'
    git push

    git add charts/root-app/templates/kubernetes-templating.yaml
    git commit -m 'add app kubernetes-templating.yaml'
    git push

## Как проверить работоспособность:
   kubectl get pods
   kubectl port-forward svc/argo-cd-argocd-server 8080:443
   перейти по ссылке http://localhost:8080

## PR checklist:
 - [x] Выставлен label с темой домашнего задания