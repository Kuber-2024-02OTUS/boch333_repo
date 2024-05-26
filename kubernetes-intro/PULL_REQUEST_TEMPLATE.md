# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [ ] Задание со *

## В процессе сделано:
 - Пункт 1
   Установка minikube и kubectl. Скачать файлы minikube.exe и kubectl.exe
   Прописать пути в переменном окружение к файлам
 - Пункт 2
   Манифест пространства имен
    .\kubernetes-intro\namespace.yaml
 - Пункт 3
   Манифест пода имен
    .\kubernetes-intro\pod.yaml

## Как запустить проект:
 - Пункт 1
    minikube start
 - Пункт 2
    kubectl create -f .\kubernetes-intro\namespace.yaml
 - Пункт 3
    kubectl apply -f .\kubernetes-intro\pod.yaml
    kubectl delete -f .\kubernetes-intro\pod.yaml
    
## Как проверить работоспособность:
 - Пункт 1
      minikube status
      kubectl cluster-info
      kubectl config view
      kubectl config get-contexts
   - Пункт 2
    - kubectl describe ns homework
    - kubectl get namespaces homework --show-labels 
   - Пункт 3
      kubectl get pod -A
      kubectl get pods -n=homework
      kubectl describe pods -n=homework
      kubectl exec -it homework-websrv -- /bin/bash
              cd usr/share//nginx/html/homework

## PR checklist:
 - [ ] Выставлен label с темой домашнего задания
