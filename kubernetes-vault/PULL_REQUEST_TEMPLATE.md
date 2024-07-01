# Выполнено ДЗ №

 - [x] Основное ДЗ
 - [x] Задание со *

## В процессе сделано:

В ходе работы мы:
    установим кластер vault в kubernetes
    научимся создавать секреты роли и политики
    настроим авторизацию в vault через kubernetes
    установите External Secrets Operator 
    создайте и примените манифест crd объекта SecretStore
    создайте и примените манифест crd объекта ExternalSecret

## Как запустить проект:

1. Подготовка

        #Create cluster
            yc managed-kubernetes cluster create --name k8s-otus-vault --network-name default --zone ru-central1-d  --subnet-name default-ru-central1-d --public-ip --service-account-id aje5e2628bn218a745ks --node-service-account-id ajenlhklhqb1ugm23k7l
        #create worknode
            yc managed-kubernetes node-group create --name  k8s-otus-workers --cluster-name k8s-otus-vault --cores 2 --memory 4 --core-fraction 5 --preemptible --fixed-size 3 --network-interface subnets=default-ru-central1-d,ipv4-address=nat
        #Connect
            yc managed-kubernetes cluster get-credentials --id cat610v62q9s69bbe5lb --external

2. Инсталляция hashicorp vault HA в k8s

        Cклонируем репозиторий consul и vault

            git submodule add https://github.com/hashicorp/consul-helm.git kubernetes-vault/consul-helm
            git submodule add https://github.com/hashicorp/vault-helm.git  kubernetes-vault/vault-helm

            kubectl create namespace vault

    Установим consul
        запускаем установку
 
            helm install consul consul-helm -f ./consulValues.yaml -n vault
 
        проверим готовность
 
            kubectl get pods
            kubectl get pvc
 
    Установим vault

        подготовим файл vaultValues.yaml 
            отредактируем параметры
	            
                standalone:
		            enabled: false
		            ....
	            ha:
		            enabled: true
		            ...
		            config: |
		                ui = true

		                listener "tcp" {
			                tls_disable = 1
			                address = "[::]:8200"
			                cluster_address = "[::]:8201"
		                }
		                storage "consul" {
			                path = "vault"
			                address = "consul-consul-server.vault.svc.cluster.local:8500"
		                }

		                service_registration "kubernetes" {}
	            ui:
		            enabled: true
		            serviceType: "ClusterIP"
		
        запускаем установку	
            helm install vault vault-helm -f ./vaultValues.yaml -n vault
        проверяем
            helm status vault
            kubectl logs vault-0
 
3. Работа с Vault.

    Для подключения к графическому интерфейсу используем форвардинг.

            kubectl port-forward vault-0 -n vault 8200:8200

    Проведем инициализацию через любой под vault'а

            kubectl exec -it vault-0 -- vault status
            kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1

    Распечатали хранилище

            kubectl exec -it vault-0 -- vault operator unseal '************************'

                Key             Value
                ---             -----
                Seal Type       shamir
                Initialized     true
                Sealed          false
                Total Shares    1
                Threshold       1
                Version         1.16.1
                Build Date      2024-04-03T12:35:53Z
                Storage Type    consul
                Cluster Name    vault-cluster-85940681
                Cluster ID      f1bb49d9-1542-6064-dd11-3ee836d011b2
                HA Enabled      true
                HA Cluster      https://vault-0.vault-internal:8201
                HA Mode         active
                Active Since    2024-06-23T12:51:51.290196027Z

            kubectl exec -it vault-1 -- vault operator unseal '************************'

                Key                    Value
                ---                    -----
                Seal Type              shamir
                Initialized            true
                Sealed                 false
                Total Shares           1
                Threshold              1
                Version                1.16.1
                Build Date             2024-04-03T12:35:53Z
                Storage Type           consul
                Cluster Name           vault-cluster-85940681
                Cluster ID             f1bb49d9-1542-6064-dd11-3ee836d011b2
                HA Enabled             true
                HA Cluster             https://vault-0.vault-internal:8201
                HA Mode                standby
                Active Node Address    http://10.112.128.35:8200

            kubectl exec -it vault-2 -- vault operator unseal '************************'

                Key                    Value
                ---                    -----
                Seal Type              shamir
                Initialized            true
                Sealed                 false
                Total Shares           1
                Threshold              1
                Version                1.16.1
                Build Date             2024-04-03T12:35:53Z
                Storage Type           consul
                Cluster Name           vault-cluster-85940681
                Cluster ID             f1bb49d9-1542-6064-dd11-3ee836d011b2
                HA Enabled             true
                HA Cluster             https://vault-0.vault-internal:8201
                HA Mode                standby
                Active Node Address    http://10.112.128.35:8200

    Проверим состояние хранилища

            kubectl exec -it vault-0 -- vault status
 
                Key             Value
                ---             -----
                Seal Type       shamir
                Initialized     true
                Sealed          false
                    Total Shares    1
                    Threshold       1
                    Version         1.16.1
                    Build Date      2024-04-03T12:35:53Z
                    Storage Type    consul
                    Cluster Name    vault-cluster-85940681
                    Cluster ID      f1bb49d9-1542-6064-dd11-3ee836d011b2
                    HA Enabled      true
                    HA Cluster      https://vault-0.vault-internal:8201
                    HA Mode         active
                    Active Since    2024-06-23T12:51:51.290196027Z

    Посмотрим список доступных авторизаций

        получаем ошибку 
            kubectl exec -it vault-0 -- vault auth list
                Error listing enabled authentications: Error making API request.

                URL: GET http://127.0.0.1:8200/v1/sys/auth
                Code: 403. Errors:

                * permission denied
                command terminated with exit code 2

    Залогинимся в vault (у нас есть root token)

            kubectl exec -it vault-0 -- vault login

                Token (will be hidden): 
                Success! You are now authenticated. The token information displayed below
                is already stored in the token helper. You do NOT need to run "vault login"
                again. Future Vault requests will automatically use this token.

                Key                  Value
                ---                  -----
                token                '************************'
                token_accessor       '************************'
                token_duration       ∞
                token_renewable      false
                token_policies       ["root"]
                identity_policies    []
                policies             ["root"]

            kubectl exec -it vault-0 -- vault auth list

                Path      Type     Accessor               Description                Version
                ----      ----     --------               -----------                -------
                token/    token    auth_token_b9d2402d    token based credentials    n/a

    Заводим секрет 

        kubectl exec -it vault-0 -- vault secrets enable --path=otus/ kv-v2

            Success! Enabled the kv-v2 secrets engine at: otus/

        проверяем создание
        kubectl exec -it vault-0 -- vault secrets list --detailed

        заводим секрет согласно заданию 
        kubectl exec -it vault-0 -- vault kv put otus/cred username='otus' password='asajkjkahs'

            = Secret Path =
            otus/data/cred

            ======= Metadata =======
            Key                Value
            ---                -----
            created_time       2024-06-23T13:06:19.75502697Z
            custom_metadata    <nil>
            deletion_time      n/a
            destroyed          false
            version            1

    Проверяем

        kubectl exec -it vault-0 -- vault read otus/data/cred

            Key         Value
            ---         -----
            data        map[password:asajkjkahs username:otus]
            metadata    map[created_time:2024-06-23T13:06:19.75502697Z custom_metadata:<nil> deletion_time: destroyed:false version:1]

    Включим авторизацию через k8s

        kubectl exec -it vault-0 -- vault auth enable kubernetes
        kubectl exec -it vault-0 -- vault auth list

    Создадим yaml для ServiceAccount и ClusterRoleBinding

        kubectl apply -f vault-auth-ServiceAccount.yaml -f vault-auth-ClusterRoleBinding.yaml -f vault-auth-secret.yaml

    Подготовим переменные для записи в конфиг кубер авторизации

        export SA_SECRET_NAME=$(kubectl get secrets --output=json | jq -r '.items[].metadata | select(.name|startswith("vault-auth-")).name')
        export SA_JWT_TOKEN=$(kubectl get secret $SA_SECRET_NAME --output 'go-template={{ .data.token }}' | base64 --decode)
        export SA_CA_CRT=$(kubectl config view --raw --minify --flatten --output 'jsonpath={.clusters[].cluster.certificate-authority-data}' | base64 --decode)
        export K8S_HOST=$(kubectl config view --raw --minify --flatten --output 'jsonpath={.clusters[].cluster.server}')

    Запишем конфиг в vault

        kubectl exec -it vault-0 -- vault write auth/kubernetes/config \
                token_reviewer_jwt="$SA_JWT_TOKEN" \
                kubernetes_host="$K8S_HOST" \
                kubernetes_ca_cert="$SA_CA_CRT" \
                issuer="https://kubernetes.vault.svc.cluster.local"
            
        Success! Data written to: auth/kubernetes/config
 
    Создадим файл политики

        vi otus-policy.hcl

            path "otus/data/cred" {
                capabilities = ["read", "list"]
                }

    Cоздадим политику и роль в vault

        kubectl cp otus-policy.hcl vault-0:/tmp
        kubectl exec -i -t vault-0 -- sh
	        ~ $ cd /home/vault
	        ~ $ mv /tmp/otus-policy.hcl ./
	        ~ $ ls -la
	
        kubectl exec -it vault-0 -- vault policy write otus-policy /home/vault/otus-policy.hcl
        
        Success! Uploaded policy: otus-policy

        kubectl exec -it vault-0 -- vault write auth/kubernetes/role/otus \
            bound_service_account_names=vault-auth \
            bound_service_account_namespaces=vault \
            token_policies=otus-policy \
            ttl=24h
        
        Success! Data written to: auth/kubernetes/role/otus

    Установка External Secrets Operator:

        cd external-secrets && helmfile apply; cd ..

    Создание SecretStore:

        kubectl apply -f secret-store.yaml

    Создание ExternalSecret:

        kubectl apply -f external-secret.yaml
    
    Проверяем
       
        kubectl get secret otus-cred

## Как проверить работоспособность:
 - kubectl port-forward vault-0 -n vault 8200:8200
   http://localhost:8200
