# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [X] Задание со *

## В процессе сделано:
 - Задание
   Создан Helm-chart - monitoring
    cd ./kubernetes-monitoring/
    helm create monitoring

## Как запустить проект:
 - Задание
    cd ./kubernetes-monitoring/monitoring
    helm install monitoring ./monitoring -n default

## Как проверить работоспособность:
 - Задание 1
    helm ls -A

    kubectl get ingress -n homework
    vi /etc/hosts > "minikube ip" homework.otus (у меня: "192.168.49.2 homework.otus" )
    minikube tunnel
    curl http://homework.otus:8000/stub_status
         http://homework.otus/homepage
         http://homework.otus/metrics

    helm ls -n homework

## PR checklist:
 - [X] Выставлен label с темой домашнего задания
