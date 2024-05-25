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
cd ./kubernetes-operators/deploy
kubectl apply -f CustomResourceMySql.yaml -f CustomResourceDefinition.yaml -f ServiceAccount.yaml -f ClusterRoleFull.yaml -f ClusterRoleMin.yaml -f ClusterRoleBinding.yaml -f DeploymentOperator.yaml 

в зависимости от задания запускаем используя образы oflmaoinmysoul/mysql-operator:1.0.0 или homework/mysql-operator-py:latest и соответственно с  ролями 
    
## Как проверить работоспособность:
Получить список созданных ресурсов:
kubectl get pvc,pv,svc,deploy,pods,mysqls,sa,crd

Подключиться к MySQL:
kubectl port-forward $(kubectl get pods | grep mysql-crd | awk '{print $1}') 3306:3306
telnet 127.0.0.1 3306
mysql -h 127.0.0.1 -P 3306 -u root -psecret

## PR checklist:
 - [x] Выставлен label с темой домашнего задания