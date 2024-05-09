# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [X] Задание со *

## В процессе сделано:
 - Задание 1
   Создан Helm-chart - websrv
    cd ./kubernetes-templating/
    helm create websrv

 - Задание 2
   Создан helmfile - helmfile.yaml
    ./kubernetes-templating/kafka
    touch helmfile.yaml
## Как запустить проект:
 - Задание 1
    cd ./kubernetes-templating/websrv
    helm install --set version=0.1.1 websrv ./websrv
 - Задание 2
    cd ./kubernetes-templating/kafka
    helmfile --environment prod apply
    helmfile --environment dev apply

## Как проверить работоспособность:
 - Задание 1
    helm install websrv ./websrv -n default --dry-run --debug
    helm ls -A

    kubectl get ingress -n homework
    vi /etc/hosts > "minikube ip" homework.otus (у меня: "192.168.49.2 homework.otus" )
    minikube tunnel
    curl http://homework.otus/conf/metric

 - Задание 1
    helmfile list
    helm ls -n prod

## PR checklist:
 - [X] Выставлен label с темой домашнего задания
