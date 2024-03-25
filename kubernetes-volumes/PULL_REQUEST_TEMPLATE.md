# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Манифест PersistentVolumeClaim
    .\kubernetes-volumes\pvc.yaml
   В манифесте .\kubernetes-volumes\pvc.yaml используеться стандартный storageClass:

      storageClassName: standard
    
 - Пункт 2
   Создан манифест configmap
     .\kubernetes-volumes\configmap.yaml
   Для пары ключь  значение:

       data:
          log_level: INFO   
       
 - Пункт 3
   В манифест deployment.yaml внесены изменения: 
   Изменена спецификация volume

       volumes:
       - name: workdir-pvs
         persistentVolumeClaim:
           claimName: websrv-pvc 

   Изменены спецификации монтировани

        volumeMounts:
        - mountPath: /homework
          name: workdir-pvs
     
        volumeMounts:
        - mountPath: /init
          name: workdir-pvs

 - Пункт 4 
 В манифесте deployment.yaml добавлено монтирование ранее созданного configMap как volume
      
       volumes: 
       - name: workdir-cm
         configMap:
           name: websrv-info

 изменена спецификация монтирования 

       volumeMounts:
       - mountPath: /homework/conf
         name: workdir-cm

 - Пункт 5 с *
   Создан манифест описывающий объект типа storageClass
      .\kubernetes-volumes\storageClass.yaml
 - Пункт 6 с *
   Внесены изменения в pvc.yaml

       storageClassName: homework-storageClass

## Как запустить проект:
 - Все пункты кроме 5
      minikube start
      cd ./kubernetes-volumes
      kubectl create -f namespace.yaml
      kubectl apply -f deployment.yaml -f service.yaml -f ingress.yaml -f pvc.yaml -f configmap.yaml
 - Пункт 5
      kubectl apply -f .\kubernetes-volumes\storageClass.yaml
    
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
V2.0 - Hello world from hostname: homework-deployment-5c656d4d68-dlfgz

## PR checklist:
 - [x] Выставлен label с темой домашнего задания