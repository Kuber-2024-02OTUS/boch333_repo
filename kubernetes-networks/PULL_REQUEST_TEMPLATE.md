# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Манифест пространства имен
      .\kubernetes-networks\namespace.yaml
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
      kubectl get all -n homework
 - Пункт 2
      kubectl get all -n homework
      kubectl get service -n homework

           NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
           service/websrv-service   ClusterIP   10.103.194.140   <none>        80/TCP    23m   

      kubectl exec -it homework-deployment-5c656d4d68-dlfgz  -n homework -- /bin/bash
      curl http://CLUSTER-IP

           $ curl http://10.103.194.140
           V2.0 - Hello world from hostname: homework-deployment-5c656d4d68-dlfgz  

 - Пункт 3
      minikube addons list
	  
 - Пункт 4
	  kubectl apply -f ingress.yaml
	  kubectl get ingress -n homework

              NAME             CLASS   HOSTS           ADDRESS        PORTS   AGE
              websrv-ingress   nginx   homework.otus   192.168.49.2   80      73s

	 vi /etc/hosts > "minikube ip" homework.otus (у меня: "192.168.49.2 homework.otus" )
	 minikube tunnel
	    
            curl http://homework.otus/index.html
            V2.0 - Hello world from hostname: homework-deployment-5c656d4d68-dlfgz

 - Пункт 5
        curl http://homework.otus/homepage

            curl http://homework.otus/homepage
            V2.0 - Hello world from hostname: homework-deployment-5c656d4d68-dlfgz

## PR checklist:
 - [x] Выставлен label с темой домашнего задания
