# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Манифест пространства имен
    .\kubernetes-intro\namespace.yaml
   В манифесте .\kubernetes-controllers\deployment.yaml пода изменен объект readinessProbe:

      readinessProbe:
         httpGet:
            path: /index.html
            port: websrv-port
         initialDelaySeconds: 5
         periodSeconds: 5

   Измененый манифест .\kubernetes-networks\deployment.yaml
    
 - Пункт 2
   Создан манифест 
      .\kubernetes-networks\service.yaml

 - Пункт 3
   Установлен аддон к minikub ingress-контроллера nginx
      minikube addons enable ingress
      kubectl get pods -n ingress-nginx

 - Пункт 4 
 Создан манифест объекта ingress (Согласно заданию)
      .\kubernetes-networks\ingress.yaml

 - Пункт 5 с *
   Создан манифест объекта с описанием правила rewrite
      .\kubernetes-networks\ingress_c.yaml

## Как запустить проект:
 - Пункт 1
      minikube start
      kubectl create -f .\kubernetes-networks\namespace.yaml
      kubectl apply -f .\kubernetes-networks\deployment.yaml
 - Пункт 2
      kubectl apply -f .\kubernetes-networks\service.yaml
 - Пункт 3 (проверить)
      kubectl get pods -n ingress-nginx    
 - Пункт 4         
      kubectl apply -f .\kubernetes-networks\ingress.yaml
 - Пункт 5   
      kubectl delete -f .\kubernetes-networks\ingress.yaml
      kubectl apply -f .\kubernetes-networks\ingress_c.yaml
    
## Как проверить работоспособность:

 - Пункт 1

 - Пункт 2

 - Пункт 3

 - Пункт 4
 
 - Пункт 5

## PR checklist:
 - [x] Выставлен label с темой домашнего задания
