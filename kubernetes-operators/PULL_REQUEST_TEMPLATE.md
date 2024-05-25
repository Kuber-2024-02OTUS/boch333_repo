# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:
 - Пункт 1
   Созданы манифесты 
   CustomResourceMySql - кастомный объект MySQL
   CustomResourceDefinition - манифест объекта с заданными параметрами 
   ServiceAccount - пользователь Оператор
   ClusterRoleFull - роль с полными правами 
   ClusterRoleMin - роль с минимально необходимыми 
   ClusterRoleBinding - присвоение прав пользователю оператор 
   DeploymentOperator - деплоймент оператора с образом roflmaoinmysoul/mysql-operator:1.0.0
    
 - Пункт 2
   Подготовил скрипт оператора mysql-operator.py и Docker образ homework/mysql-operator-py:latest

## Как запустить проект:
minikube start 
kubectl apply -f kubernetes-operators/deploy/crd.yml
kubectl apply -f kubernetes-operators/deploy/service-account.yml
kubectl apply -f kubernetes-operators/deploy/role.yml
kubectl apply -f kubernetes-operators/deploy/role-binding.yml
kubectl apply -f kubernetes-operators/deploy/deploy-operator.yml
kubectl apply -f kubernetes-operators/deploy/cr.yml
    
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