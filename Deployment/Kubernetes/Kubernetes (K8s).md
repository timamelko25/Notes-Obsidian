Предназначен для управления кластером контейнеров Linux как единой системой. Идет управление и запуск контейнеров Docker на большом количестве хостов, а так же обеспечивает совместное размещение и репликацию большого количества контейнеров.

Концепция Kubernetes
- Nodes (Control Plane - управляющие ноды/ Мастер ноды - устаревшее название) - машина в кластере кубера
- Pods (Worker Nodes) - группа контейнеров с общими разделами, запускаемых как единое целое
- Replication Controllers - отслеживание, что определенное количество "реплик" подов будут запущены в любой момент времени
- Services - абстракция для определения логического объединенного набора пода и политики доступа к ним
- Volumes - директория с данными
- Labels - пары ключ-значение, которые прикрепляются к объектам, например, подам. Могут быть использованы для создания и выбора наборов объектов
- Kubectl CLI - интерфейс командной строки для управления кубером

Архитектура Kubernetes
Работающий кластер кубера включает в себя агента, запущенного на нодах (kubelet) и компоненты мастера (APIs, scheduler, etc) поверх решения с распределенным хранилищем. 

Node

Kubelet
управляет подами из контейнерами, образами, разделами, etc

Kube-Proxy
На каждой ноде запускается простой proxy-балансировщик. Этот сервис запускается на каждой ноде и настраивается в Kubernetes API. Kube-Proxy может выполнять простейшее перенаправление потоков TCP UDP (round robin) между набором бэкендов.

Компоненты управления Kubernetes
Система управления имеет несколько компонентов. 

etcd
Состояние мастера хранится в экземпляре etcd. Обеспечивает надежное хранение конфигурационных данных и своевременное оповещение прочих компонентов об изменении состояния. Находится в Control Plane. Является распределенной бд (ключ-значение), где хранится вся информация: поды, сервисы, конфиги. Обычно распределено на нескольких управляющих нодах для надежности. Для полной надежности ставится отдельно.

Kubernetes API Server
Обеспечивает работу API-сервера.

Scheduler

Kubernetes Controller Manager Server

https://selectel.ru/blog/kubernetes-review/
https://habr.com/ru/companies/h3llo_cloud/articles/902188/


https://www.youtube.com/watch?v=Mw_rEH2pElw

# Лекции слерм
https://github.com/slurm-personal/school-dev-k8s

## Pod

Минимальная абстракция кубера с которой он работает. Представляет собой 1 запущенное приложение в рамках кубера. Внутри пода содержаться контейнеры (как правило 1 основной). В основной логике кубера содержится контейнер приложения и контейнер POD_*** (хранит сетевой namespace). Как правило не создается вручную, обычно указывается через replicaset. 

```pod.yaml
--- # начало файла
# file: practice/2.application-abstractions/1.pod/pod.yaml
apiVersion: v1 # версионирование апи
kind: Pod # тип объекта (secret, service, etc)
metadata: 
  name: my-pod
spec: # описание объекта
  containers:
  - image: quay.io/testing-farm/nginx:1.12
    name: nginx
    ports: # просто способ документации, порт так не открывается для пода! серивис важно
    - containerPort: 80
... # конец файла (... - редко, обычно ---)

--- # добавить еще один файл 
...
```

Способы управления:
- kubectl - командный интерфейс для работы с Kubernetes API (построено на REST)
- UI - любое веб приложение для графической работы

[Установка kubectl][https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/]

Для работу kubectl необходим файл kubeconfig который находится в `~/.kube/config`. Можно установить с помощью скрипта [kube-up.sh][https://github.com/kubernetes/kubernetes/blob/master/cluster/kube-up.sh] или при развертке локального Minikube,

Minikube - инструмент для запуска одноузлового кластера Kubernetes на виртуальной машине в персональном компьютере.
[Установка][https://kubernetes.io/ru/docs/tasks/tools/install-minikube/] 

`sudo usermod -aG docker $USER && newgrp docker`

`kubectl get pod` - показывает список запущенных подов

`kubectl create -f pod.yaml` - создать объект (под, replicaset). после создания невозможно создать еще раз.
`kubectl apply -f pod.yml` - применить изменения по файлу. используется после изменений в файле или для создания объекта, если он не был создан

`kubectl delete pod pod_name` - удалить под
`kubectl delete -f pod.yaml` - удаление пода по файлу
`kubectl delete pod --all` - удалить все запущенные поды
`kubectl delete all --all -A` - удаляет все поды во всех namespace 

`kubectl edit object name_object` - изменить объект внутри кубера который запущен, будет изменено сразу в кластере, локально файл не будет изменен

`kubectl describe po pod_name` - полное описание пода и всю информацию

`kubectl explain po/rs` - help по описанию полей объекта в файле описания

`kubectl rollout undo object name_object` - откатить последнее выполненное действие

`kubectl rollout history object name_object` 

(редко/ для скриптов) `kubectl patch object name_object -- patch '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","resources":{"requests":{"cpu":"10"},"limits":{"cpu":"10"}}}]}}}}'`

Абстракции приложений

ReplicaSet
ReplicaSet (служебная абстракция над подом)- внутри содержится template, с описанием объектов, который создает replicaset. Для определения подов каждой реплике будут поставлены labels. Есть selector для определения подов. Всегда будет поддерживаться заданное количество реплик в конфиге, даже при удалении пода или нода вышла из строя, инстанс объекта будет запущен снова

![[Pasted image 20250510002335.png]]

файл по созданию реплик объектов. название для пода не указывается, указывается selector для выбора по каким labels искать, template для указания labels. имя самого контейнера необходимо использовать

```replicaset.yaml
---
# file: practice/2.application-abstractions/2.replicaset/replicaset.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        ports:
        - containerPort: 80
...

```

Для скейла реплик можно 
- указать в файле новое количество реплик
- `kubectl scale --replicas int replicaset replicaset_name`

При даунскейле будет удаляться самая молодая реплика (один из критериев). Зачем это надо?
Нода вышла из строя (отвалилась сеть, менеджер кубера упал, но сервисы остались и недоступны) будут созданы новые инстансы объектов и при восстановлении работы ноды, созданные объекты будут отключены

Если создать под с таким же именем и лейблом, то rs сразу переведет его в Terminate, потому что реплик будет больше чем задано

Обновление образа в контейнерах
- вручную изменить в yaml конфиг
- `kubectl set image rs replica_set nginx=...` - обновить конкретный контейнер image на такой 
- `kubectl set image rs replica_set '*=...'` - обновить все контейнеры на image такой (применится ко всем образам)
При выполнении команды будет заменена версия, но приложение будет работать на старой версии. replicaset только проверяет labels и количество реплик, что происходит в template он не проверяет

Deployment 
Deployment (абстракция над replicaset). Решает проблему обновления приложения. Создает под собой replicaset, replicaset создает под собой pods. При удалении верхнеуровневой абстракции, все нижнеуровневые тоже удаляются. Скейлить необходимо сразу deployment

![[Pasted image 20250510132852.png]]

```deployment.yaml
---
# file: practice/2.application-abstractions/4.resources/deployment-with-resources.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m # милиcpu, максимум будет потреблять 0.1 работы 1 ядра процессора
            memory: 100Mi
...
```

NameSpaces
Пространство имен (dev, stage). Не может быть одинаковых имен в одном пространстве, в разных можно

![[Pasted image 20250511140648.png]]

Resources
- Limits
	- Количество ресурсов, которые POD может использовать (количество памяти...)
	- Верхняя граница
- Requests (не физический показатель, указывается в yaml файле. по этому параметру определяет на какую ноду запустить под и резервирует количество ресурсов, при этом кубер не проверяет реальное потребление самого приложения)
	- Количество ресурсов, которые резервируются для пода на ноде
	- не делятся с другими подами на ноде
QoS Class: (используется при переносе подов на другие ноды в случае проблем)
- BestEffort - лучшее приложение, без лимитов и requests, занимает все свободные ресурсы, (при нехватке памяти удаляются в первую очередь
- Burstable - есть лимиты/requests или и то и то, но лимиты выше requests (резерв 100 памяти, лимит 1000), удаляются во вторую очередь
- Guaranteed - лимиты и requests равны по параметрам, удаляются в последнюю очередь
Сокращения для `kubectl`
pod - po
replicaset - rs
deployment - deploy

Поднятие прод кластера

[kube spray][https://github.com/kubernetes-sigs/kubespray]
kube atom