- [Лекции слерм](#%D0%9B%D0%B5%D0%BA%D1%86%D0%B8%D0%B8%20%D1%81%D0%BB%D0%B5%D1%80%D0%BC)
		- [Pod](#Pod)
	- [Абстракции приложений](#%D0%90%D0%B1%D1%81%D1%82%D1%80%D0%B0%D0%BA%D1%86%D0%B8%D0%B8%20%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B9)
		- [ReplicaSet](#ReplicaSet)
		- [Deployment](#Deployment)
		- [NameSpaces](#NameSpaces)
		- [Resources](#Resources)
	- [Хранение конфигураций приложений](#%D0%A5%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%B0%D1%86%D0%B8%D0%B9%20%D0%BF%D1%80%D0%B8%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B9)
		- [Настройки окружения](#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B8%20%D0%BE%D0%BA%D1%80%D1%83%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F)
		- [ConfigMap](#ConfigMap)
		- [Secrets](#Secrets)
		- [Volumes (тома)](#Volumes%20(%D1%82%D0%BE%D0%BC%D0%B0))
		- [Downward API](#Downward%20API)
	- [Хранение данных](#%D0%A5%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%20%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)
		- [HostPath](#HostPath)

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

### Pod

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
`kubectl get object object_name -o yaml` - вывести информацию об объекте в yaml формате

`kubectl create -f pod.yaml` - создать объект (под, replicaset). после создания невозможно создать еще раз.
`kubectl apply -f pod.yml` - применить изменения по файлу. используется после изменений в файле или для создания объекта, если он не был создан

`kubectl delete pod pod_name` - удалить под
`kubectl delete -f pod.yaml` - удаление пода по файлу
`kubectl delete pod --all` - удалить все запущенные поды
`kubectl delete all --all -A` - удаляет все поды во всех namespace 

`kubectl edit object name_object` - изменить объект внутри кубера который запущен, будет изменено сразу в кластере, локально файл не будет изменен

`kubectl describe po pod_name` - полное описание пода и всю информацию

`kubectl explain po/rs/deploy` - help по описанию полей объекта в файле описания

`kubectl rollout undo object name_object` - откатить последнее выполненное действие

`kubectl rollout history object name_object` 

(редко/ для скриптов) `kubectl patch object name_object -- patch '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","resources":{"requests":{"cpu":"10"},"limits":{"cpu":"10"}}}]}}}}'`

`kubectl exec -it object_name -- bash` - выполнение скрипта на поде, `--` указывает на ограничение между командой кубера и исполняемого скрипта

`kubectl create secret generic test --from-literal=test1=asdf --from-literal=dbpassword=1q2w3e` - создание generic секрета. При просмотре секретов тип generic будет указан как Opaque (так сложилось исторически)

`kubectl port-forward my-deployment-5f477f44fd-9b9bt 8080:80 &` - установка соединения с подом хост:под

## Абстракции приложений

### ReplicaSet
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

### Deployment 
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
  strategy:
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 1
      type: RollingUpdate
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

!стратегия обновления rollingupdate

### NameSpaces
Пространство имен (dev, stage). Не может быть одинаковых имен в одном пространстве, в разных можно

![[Pasted image 20250511140648.png]]

### Resources
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

## Хранение конфигураций приложений

### Настройки окружения
- указываются напрямую в файле
```
spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        env:
        - name: TEST
          value: foo
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
```
### ConfigMap
ConfigMap - yaml файл в котором указываются настройки приложения. Потом этот файл указывается для различных объектов. Обычно используется для хранения общей информации, не для секретной (пароли/ключи). Есть возможность передавать сразу текст (передать файл конфигурации, лучше использовать малые файлы). ! при указании файла он будет монтироваться на диск в указанный путь, но сначала будет создана директория с текущем временем, в ней директория data и уже туда будет помещен файл. После сам файл не будет скопирован, а будет создана symlink и помещена в монтируемую директорию. При создании symlink кубер зачищает директорию куда помещается ссылка, и таким образом, если указать запись в `/etc` все файлы оттуда будут очищены и могут возникать ошибки. При работе с docker compose он добавляет файлы, и может быть что в докере все работает, но в кубере нет

![[Pasted image 20250512135039.png]]

```configmap.yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap-env
data: # идут переменные окружения
  dbhost: postgresql
  DEBUG: "false"
  default.conf: | # палочка для многострочного текста
      server {
          listen       80 default_server;
          server_name  _;

          default_type text/plain;
 
          location / {
              return 200 '$hostname\n';
          }
      }
...
```

Для применения переменных окружения из configmap в манифесте deploy указывается 

```
spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        env:
        - name: TEST
          value: foo
        envFrom:
        - configMapRef: # ссылка на использование configmap и назавние откуда брать переменные
            name: my-configmap-env
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
```

При обновлении значений в configmap, переменные окружения поменять в запущенной среде linux затруднительно, поэтому необходимо убить все поды и запустить заново для применения новых настроек. При работе с файлами configmap кубер понимает, что был изменен файл и сам обновит файл, но процесс должен сам понимать, что файл был изменен

### Secrets
Secrets - такая же структура манифеста как у configmap. данные кодируются в base64, не шифруются. Просмотр информации обычно организован только с помощью прав доступа (админ) и не доступен всем, когда configmap доступен всем. base64 используется для приведения символов к единому формату для избежания ошибок. Можно зайти в под и вручную ввести `env` для просмотра секретов (если есть права)
	- generic - пароли/токены для приложений
	- docker-registry - данные авторизации в docker registry (доступ к приватному репозиторию контейнеров)
	- tls - TLS сертификаты для lngress (для сертификатов, https)


```
spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        envFrom:
        - configMapRef: # берется вся информация из configmap
            name: my-configmap-env
        env:
        - name: TEST
          value: foo
        - name: TEST_1 # укзаывается знчение переменной для приложеня
          valueFrom:
            secretKeyRef: # берется конкретное ключ-значени из секрета
              name: test
              key: test1
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
```

манифест secret
```secret.yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: test
stringData: # указывается прямым текстом, все значения, которые потом кубер сам закодирует в base64
  test: updated
...
```

При merge файлов с секретами будет выдан warning при отсутствии настроек merge. при добавлении нового он будет добавлен, при изменении существующего его значение изменено, если существующее имя поменять на другое, секрет в поде поменяется, старый примет пустое значение

### Volumes (тома)
volumeMounts - описывает точки монтирования внутри контейнера, куда записать и что записать
volume - указывается имя и файл, который будет монтироваться. Существует множество типов томов для монтирования (пример для файла конфигурации из configmap)

```
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
            cpu: 100m
            memory: 100Mi
        volumeMounts:
        - name: config
          mountPath: /etc/nginx/conf.d/
      volumes:
      - name: config
        configMap:
          name: my-configmap
```

### Downward API

## Хранение данных

Good state - dead state (stateless)
Хранить данные в бд, файлы в S3.

Для хранения state в кубере

### HostPath
Аналог в DockerCompose. Не контролируется настрой доступов, большая утечка безопасности может быть и используется только для служебных компонентов

![[Pasted image 20250514143144.png]]

```
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
            cpu: 100m
            memory: 100Mi
        volumeMounts:
        - name: data
          mountPath: /files
      volumes:
      - name: data
        hostPath:
          path: /data_pod
```
### EmptyDir

создается временный диск пока под жив, когда под удаляется директория тоже удаляется. Если в ноде под перезагружается, то данные остаются (аналог Docker Volume). Используется обычно для тестов, где нужно временно запустить и посмотреть. Ограничение на размер этой директории (определяются со статусом узлка как ephemeral storage) и по фатку хранятся в каталоге `/var/lib/kublet` и ограничение идет от этой директории. Лучше использовать это вместо напрямую записывать в файлы в контейнеры, потому что контейнеры представляют из себя слои, которые дорого обслуживать и делать постоянную запись невыгодно, лучше создать отдельное место и его примонтировать

![[Pasted image 20250514144008.png]]

```
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
            cpu: 100m
            memory: 100Mi
        volumeMounts:
        - name: data
          mountPath: /files
      volumes:
      - name: data
        emptyDir: {}
```

используется `{}` - для инициализации пустого словаря, потому что без значения будет ошибка инициализации

### PV/PVC (Persistent Volume (Claim))

Ссылка на какое-то хранилище к которому мы подключаемся и читаем/записываем данные. Задается своими параметрами. 

```volumes:
	- name: mypd
	  persistentVolumeClaim:
	    claimName: myclaim
```

Параметры хранилища
- Storage class - хранит параметры подключения
- PersistentVolumeClaim - описывает требования к тому
- PersistentVolume - хранит параметры и статус тома

Вариант получения PV
- админ идет в систему хранения данных, руками создает диски и по id дискам создает манифесты PV внутри кубера. После идет bound на диск и его монтирование и диск уходит в статус bound. Главное это удовлетворить запрос на объем диска, если будет запрос на 10, а есть диск на 100, то будет выделен полный диск на 100. Получается неэффективная схема
![[Pasted image 20250514152311.png]]
- PV Provisioner - программа, которая самостоятельно может получать диски в системе хранения и создавать манифесты
![[Pasted image 20250514230430.png]]

```pvc.yaml
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: fileshare
spec:
  storageClassName: csi-ceph-hdd-ms1 # зависит от системы хранения 
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 10Mi # требуемый объем файлов
```

Подключение pvc к поду с директорией data

```
spec:
      initContainers:
      - image: busybox
        name: mount-permissions-fix
        command: ["sh", "-c", "chmod 777 /data"]
        volumeMounts:
        - name: data
          mountPath: /data
      containers:
      - image: centosadmin/reloadable-nginx:1.12
        name: nginx
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
        volumeMounts:
        - name: config
          mountPath: /etc/nginx/conf.d
        - name: data
          mountPath: /data
      volumes:
      - name: config
        configMap:
          name: fileshare
      - name: data
        persistentVolumeClaim:
          claimName: fileshare
```


### initContainers

Контейнер, который запускается до запуска основного приложения, обычно используется для настройки доступов. Может быть несколько и запускаются по порядку описания в манифесте. Можно монтировать те же тома, что и в основных контейнерах, можно запускать от другого пользователя. Сначала запускается и потом останавливается, дальше постоянно работают контейнеры с приложением

```
spec:
      initContainers:
      - image: busybox
        name: mount-permissions-fix
        command: ["sh", "-c", "chmod 777 /data"]
        volumeMounts:
        - name: data
          mountPath: /data
      containers:
      - image: centosadmin/reloadable-nginx:1.12
        name: nginx
```


## Сетевые абстракции. Probes
Виды healthcheck для приложений
### LivenessProbes
- Контроль за состоянием приложения во время его жизни
- Исполняется постоянно
- В случе потери связи приложение убивается и перезагружается

### Readiness Probe
- Проверят готово ли приложение принимать трафик
- В случае неудачного выполнения приложение убирается из балансировки (трафик не будет идти, и будет проверять кубером на работоспособность)
- Исполняется постоянно
### Startup Probe
- Проверяет запустилось ли приложение (в период какого-то времени будет проверяться работа приложения)
- Исполняется при старте

```
spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        ports:
        - containerPort: 80
        readinessProbe:
          failureThreshold: 3 # кол-во провальный попыток которые можно допустить
          httpGet: # сама проверка, просто http запрос, проверка является успешной со статусом 200-399. есть еще проверка exec на проверку командой в контейнере. tcpsocket если есть ответ то успех
            path: /
            port: 80
          periodSeconds: 10
          successThreshold: 1 # достаточно хотя бы 1 успешной проверки
          timeoutSeconds: 1
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /
            port: 80
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
          initialDelaySeconds: 10 # сколько ждать перед проверкой (устарело)
        startupProbe:
          httpGet:
            path: /
            port: 80
          failureThreshold: 30
          periodSeconds: 10
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
```


## Способы публикации

### Service
- ClusterIP - тип сервиса для внутрекластерного взаимодействия. Для корректной работы необходимо правильно настроенный select по labels. Все должно быть все в одном namespace. Команды для прокидки портов
![[Pasted image 20250515141657.png]]
- NodePort - можно опубликовать приложение наружу. На каждой ноде кластера будет создано правило, которое будет транслировать подключение с порта (диапазон 30000-32676) ноды на приложение. Данный тип сервиса преимущественно используется если снаружи используется центральный балансировщик.
![[Pasted image 20250515144355.png]]
- LoadBalancer - преимущественно используется облачными провайдерами
![[Pasted image 20250515144848.png]]
- ExternalName - переадресация
![[Pasted image 20250515145052.png]]
- ExternalIPs - здесь идет работс с ip а не с портами
![[Pasted image 20250515145113.png]]

### Headless
главное поле `ClusterIP: none`. Создается DNS запись в DNS сервере кубера и не будет присвоен IP. Используется в statefull state приложениях (запись данных в конкретные места) 
### Ingress (контроллер)
Задача для публикации приложения наружу, аналог nginx куда приходят запросы и оттуда идут в приложение. Работает только для http http, но через анотации можно определить другие протоколы

![[Pasted image 20250515151941.png]]

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress-nginx
  # можем указать annotations для дополниельных парамаетров который yaml не может распознать
  # может создать snippet который будут поддерживать кастомные параметры (скрещивание 2 usesr agent)
spec:
  rules: # можно указывать несколько хостов или вообще не будет
  - host: my.s<свой номер логина>.mcs.slurm.io # какой домен
    http:
      paths: # может быть несколько путей
      - pathType: Prefix # просто путь, есть еще Exact
        path: "/"
        backend:
          service:
            name: my-service
            port:
              number: 80

```

Для реализации HTTPS
Подключение сертификатов и указание в ingress сертификатов
![[Pasted image 20250515173905.png]]

cert-manager - для автоматизации получения сертификатов

![[Pasted image 20250515173939.png]]






Сокращения для `kubectl`
pod - po
replicaset - rs
deployment - deploy
configmap - mp
service - svc
ingress - ing

Поднятие прод кластера

[kube spray][https://github.com/kubernetes-sigs/kubespray]
kube atom