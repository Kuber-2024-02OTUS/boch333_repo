# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Манифест  ServiceAccount, ClusterRole, ClusterRoleBinding
    .\kubernetes-security\sa-monitoring.yaml
    .\kubernetes-security\rb-monitoring.yaml
    .\kubernetes-security\cr-monitoring.yaml
    
 - Пункт 2
    В манифест deployment.yaml добавлена строка

      serviceAccountName: monitoring 
       
 - Пункт 3
   Созданы манифесты ServiceAccount, Role, RoleBinding
      .\kubernetes-security\sa-cd.yaml
      .\kubernetes-security\rb-cd.yaml
      .\kubernetes-security\r-cd.yaml

 - Пункт 4 
    Создан файл kubeсonfig

 - Пункт 5 
    kubectl create token cd --duration 24h > ~/.kube/token
    Для постоянного токена используеться секрет cd-user-secret созданный при создание SA.

 - Пункт 6 *

## Как запустить проект:
 - Все пункты
      minikube start
      cd ./kubernetes-security
      kubectl label nodes minikube homework=true
      kubectl create -f namespace.yaml
      kubectl apply -f deployment.yaml -f service.yaml -f ingress.yaml -f pvc.yaml -f configmap.yaml
      kubectl apply -f sa-monitoring.yaml -f sa-cd.yaml -f 
      kubectl apply -f cr-monitoring.yaml r-cd.yaml
      kubectl apply -f rb-monitoring.yaml -f rb-cd.yaml
      
      применить ConfigFile
      export KUBECONFIG=~/.kube/kubeconfig
      Использовать контекст если их сделать несколько в моем случае контекст оставил 1
      kubectl config use-context homework
      Сгенерировать токен и применить в кофигфайле
      kubectl config set-credentials cd --token=$(kubectl create token cd --duration 24h)
      Использовать постоянный токен 
      kubectl config set-credentials sa-user --token=$(kubectl get secret cd-user-secret -o jsonpath={.data.token} | base64 -d)
    
## Как проверить работоспособность:

 - Пункт 1, Пункт 3    
          kubectl  get roles
          kubectl  get roles -n homework
          kubectl  get clusterroles
          kubectl  get rolebinding
          kubectl  get rolebinding -n homework
          kubectl  get clusterrolebinding
          kubectl  get rolebinding -n homework

 - Пункт 2

      kubectl describe pods homework-deployment-6bcc5f5f7f-5sjkh

        Name:             homework-deployment-6bcc5f5f7f-5sjkh
        Namespace:        homework
        Priority:         0
        Service Account:  monitoring
        ...

 - Пункт 4
      kubectl config view
      export KUBECONFIG=~/.kube/kubeconfig
      kubectl config --kubeconfig=kubeconfig view
      kubectl config get-contexts
      kubectl config current-context
      kubectl config view

      kubectl config set-credentialscdr --token=$(kubectl get secret cd-user-secret -o jsonpath={.data.token} | base64 -d)
      при использование не временного токена появляеться сообщение

        Warning: Use tokens from the TokenRequest API or manually created secret-based tokens instead of auto-generated secret-based tokens.
        User "cd" set.

      провести операции доступные админу.


 - Пункт 5

## PR checklist:
 - [x] Выставлен label с темой домашнего задания