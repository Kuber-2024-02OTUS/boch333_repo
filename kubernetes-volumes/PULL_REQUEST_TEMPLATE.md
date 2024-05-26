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
      ubectl label nodes minikube homework=true
      kubectl create -f namespace.yaml
      kubectl apply -f deployment.yaml -f service.yaml -f ingress.yaml -f pvc.yaml -f configmap.yaml
 - Пункт 5
      kubectl apply -f .\kubernetes-volumes\storageClass.yaml
    
## Как проверить работоспособность:

 - Пункт 1    
      kubectl get pvc -n homework
	  
 - Пункт 2

      kubectl get configmap -n homework 

 - Пункт 3-4

      kubectl describe deployment -n homework 

            Mounts:
              /homework from workdir-pvs (rw)
              /homework/conf from workdir-cm (rw)
            Volumes:
              workdir-pvs:
                Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
                ClaimName:  websrv-pvc
                ReadOnly:   false
              workdir-cm:
                Type:      ConfigMap (a volume populated by a ConfigMap)
                Name:      websrv-info
                Optional:  false

Зайти в под убедиться, что существут дериктория ./homework а в ней есть файл server_test.py и дериктория  ./homework/conf

      kubectl exec -it homework-deployment-57b995d98c-67f5m -n homework -- /bin/bash
      ls -la ./homework

проверить содержимое файла log_level должно быть: INFO

      cat ./homework/conf/log_level
        INFO

для проверки содержимого файла по ссылке

	    kubectl get ingress -n homework
	    vi /etc/hosts > "minikube ip" homework.otus (у меня: "192.168.49.2 homework.otus" )
	    minikube tunnel
	    curl curl http://homework.otus/conf/file

          Open file log_level: INFO

 - Пункт 5-6 c *
        kubectl get storageClass

          NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
          homework             k8s.io/minikube-hostpath   Retain          Immediate           false                  63m
          standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  81m
  произвести замену в файле pvc.yaml
        
        storageClassName: homework

  Проверим как в пунктаз 3-4. предварительно пересоздав deploymen и pvc

## PR checklist:
 - [x] Выставлен label с темой домашнего задания