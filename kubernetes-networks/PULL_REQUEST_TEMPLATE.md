# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Манифест пространства имен
    .\kubernetes-intro\namespace.yaml
 - Пункт 2
   Манифест пода имен
    .\kubernetes-controllers\deployment.yaml
 - Пункт 3
   В мвнифест deployment.yaml добвылен объект 
      nodeSelector:
        homework: "true"
   выполнена команда назначения лейбла на ноду
      kubectl label nodes minikube homework=true

## Как запустить проект:
 - Пункт 1
    minikube start
 - Пункт 2
    kubectl create -f .\kubernetes-intro\namespace.yaml
 - Пункт 3
     

    
## Как проверить работоспособность:
 - Пункт 1
      kubectl describe ns homework
      kubectl get namespaces homework --show-labels 
 - Пункт 2
      kubectl get pod -A
      kubectl get pods -o wide
      kubectl describe pods -n=homework
      kubectl exec -it homework-websrv -- /bin/bash
              rm -R /usr/share/nginx/html/homework/index.html
      kubectl get pod -A
      Статус пода 
         NAMESPACE     NAME                                   READY   STATUS    RESTARTS        AGE
         homework      homework-deployment-577bc7fc6f-5z7gn   0/1     Running   0               8m32s
      В этом состаяние удалить еще 1 хост для проверки стратегии
         kubectl delete pod homework-deployment-577bc7fc6f-5z7gn
      для проверки задания со звездочкой 
        kubectl label node minikube homework-
        kubectl delete pod homework-deployment-577bc7fc6f-5z7gn
      Статус пода
         NAME                                   READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
         homework-deployment-577bc7fc6f-6h4zd   0/1     Pending   0          30s     <none>        <none>     <none>           <none>
      kubectl label nodes minikube homework=true
      Статус пода
         NAME                                   READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
         homework-deployment-577bc7fc6f-6h4zd   1/1     Running   0          2m22s   10.244.0.12   minikube   <none>           <none>  

## PR checklist:
 - [x] Выставлен label с темой домашнего задания
