
Ansible - инструмент для автоматизации задачи по подготовке и конфигурированию инфраструктуры  таких процессов как:
- настройка серверов
- развертывание приложений
- управление конфигурациями
- оркестрация (координация работы нескольких машин)

Принцип работы:
- используется SSH для подключения к серверам (работает без агента)
- настройки описываются в YAML (файл с настройками называется playbook)
- декларативный подход

Основные компоненты
- Inventory - список управляемых устройств и их переменных
- Playbook - инструкции для применения
- Task - отдельное задача
- Role - структурированная группа задач и файлов
- Module - встроенные функции в ansible

Пример
```
# хосты - серверы
- hosts: webservers
  # суперпользователь - sudo
  become: yes
  # задачи
  tasks:
    - name: Установить nginx
      # модуль
      apt:
        name: nginx
        state: present
```

Pull - на управляемых серверах стоит Agent который делает Pull настроек от Master
Push - на управляемых серверах ничего не установлено, Master делает Push настроек

Аналоги
- Chef
- Puppet
- Saltstack

Control Server - Master Server

Controlled Servers - Managed Servers

hosts.txt - файл для подключения к серверам. Можно использовать password `ansible_pass=pass` для авторизации по ssh. Для избежания установки fingerprint с каждым сервером можно создать `ansible.cfg`.

плохая практика
```hosts.txt
11.11.11.11 - ungrouped

[test_servers] - группа
linux1 ansible_host=111.111.111.111 ansible_user=root ansible_ssh_port = 111 ansible_ssh_private_key_file=~/.ssh/111

[test2]

[test_all:children] - указание большой группы куда входят другие
test_servers
test2

[test_servers:vars] - указание переменных для всей группы
ansible_user=root
```

хорошая практика

`mkdir group_vars`
`micro group_vars/group_name` - вынести все переменные по файлам групп

`ansible -i hosts (group) -m ping` - выполнение модуля ping на серверах из inventory hosts

```ansible.cfg
[defaults]
host_key_checking = false
inventory = ./hosts.txt
```

`ansible-inventory --list/--graph` - просмотр всего inventory файла

Ad-hoc commands
`ping`
`setup` - полная информации по серверу
`shell -a 'command'` - запуск команды
`command -a 'command'` - запуск без shell (нет переменных окружения $HOME, '>' все потоки ввода/вывода не работают)
`copy -a "src=/path/to/file dest=/path/to/file (mode=777)"` - копирует файлы на машины, если файл уже есть, то status unchanged
`file -a "path=/path/to/file state=abscent"` - выполнить действие с файлов (удалить)
`get url -a "url=link dest=/path/to/dir"` - скачать файл по url
`yum -a "name=tool_name state=installed"` - установить через пакетный менеджер Fedora программу
`apt -a "name=tool_name` - ubuntu пакетный менеджер
`uri -a "url=link (return_content=yes)"` - проверка подключения к link, без контента (флаги для показа контента)
`service -a "name=service_name state=started/stopped/restarted enable=yes` - выполнить systemctl для сервиса
`ansible-doc l` - все модули 


`-b` - become, выполнить команду от sudo
`-v -vv -vvv -vvvv -vvvvv` - verbose расширенная информация по команде 

Playbook - файл с инструкциями в формате yml
`ansible-playbook playbook_name` - запуск скрипта. Можно вызвать любые переменные: из vars, hosts, `setup`. Для записи stdout команды, необходимо записать в переменную через `register`

```playbook.yml
- name: Test connection
  hosts: all
  become: yes

  tasks:
    - name: Ping servers
      ping:
```

handlers - вызов функции в каком-то блоке

```playbook.yml
- name: Install nginx and Start Web Pag
  hosts: all
  become: yes

  vars:
    source_file: ./site/index.html
    destin_file: /var/www/html

  tasks:
  - name: Install Nginx
    apt: name=nginx

  - name: Copy Page to Server
    copy: src={{ source_file }} dest={{ destin_file }}
    notify: Restart Nginx

  - name: Start Page and make it enable on boot
    service: name=nginx state=started enabled=yes

  handlers:
  - name: Restart Nginx
    service: name=nginx state=restarted
```

Условия и блоки

```
---
- name: Install neofetch
  hosts: all
  become: true

  tasks:
    - name: Print Linux version
      debug:
        var: ansible_os_family

#     - name: Install neofetch for Ubuntu
#       apt: name=neofetch
#       when: ansible_os_family == "Debian"
#
#     - name: Install neofetch for RedHat
#       yum: name=neofetch
#       when: ansible_os_family == "RedHat"

    - block: # RedHat
        - name: Install neofetch for RedHat
          yum: name=neofetch
      when: ansible_os_family == "RedHat"

    - block: # Ubuntu
        - name: Install neofetch for RedHat
          yum: name=neofetch
      when: ansible_os_family == "RedHat"

    - name: Run neofetch
      shell: neofetch
      register: results

    - name: Print command result
      debug:
        var: results
```


Циклы

```playbook.yml
---
- name: Loops Playbook
  hosts: all
  become: true

  vars:
    source_folder = "./folder"
    dest_folder = "/var/site/"

  tasks:
    - name: for each
      debug:
        msg: "Hello {{ item }}"
    #  with_items: depracated
      loop:
        - "1"
        - "2"
        - "3"
        - "4"

    - name: loop until
      shell: echo -n A >> file.txt && cat file.txt
      register: output
      delay: 2 #  default 0
      retries: 10 #  default 3 
      until: output.stdout.find("AAAA") == false


    - name: copy all files
      copy: src={{ item }} dest={{ dest_folder }} mode=0555
      with_fileglob: "{{ source_folder }}/*.*"
```

Шаблоны Jinja
изменение шаблонов html с передачей переменных их окружения ansible
```
template: src={{ source_folder }} dest={{ dest_folder }}
```

Роли

`asnible-galaxy init role_name`

сразу знает из каких директорий брать зависимости 

структура роли по директориям
defaults - переменные по умолчанию
files - файлы для работы
handlers - обработчики
meta - мета информация
tasks - задачи
templates - блоки с использованием шаблонов
tests 
vars - остальные переменные 

для вызова playbook

```
roles:
  - deploy_test
  - deploy_db
  - role_name
  - { role: super_admin, when: ansible_system == "Linux"}
```

extra-vars

```
hosts: "{{ MUHOSTS }}" 
```

Для передачи переменной

`ansible-playbook playbook.yml -e "MYHOSTS=STAGING"`
Переменные переданные в extra-vars имеют наивысший приоритет
флаги для передачи
-e 
--extra-var
--extra-vars

Include, Import

подключение внешний файлов для задач
include - при выполнении playbook в сценарий будет помещено только на конкретном шаге 
import - при выполнении playbook, содержимое будет сразу вставлено в основной сценарий и проверено на синтаксис и подставлены переменные

Можно передать переменные вручную

```
tasks:
...

  - name: Create folder
    include: create_folders.yml

  - name: Create files
    inlcude: create_files.yml mytext="123"
```

Перенаправление Task из Playbook на другой сервер

`delegate_to: server_name` - выполнить команду только на определенном сервере

Можно выполнить команду на удаленном сервере и перенаправить вывод на основой сервер

```
- name: Only on 1 server
  copy: ...
  delegate_to: server_name

- name: Send to another server
  shell: echo Hello from {{ inventory_hostname }} >> /var/log.txt
  delegate_to: 127.0.0.1
```

```
- name: Reload servers
  shell: reboot now
  async: 1 # 2 обязательных параметра для отлючения проверки по ssh и проверка каждую 1 сек
  poll: 0

- name: wait reload
  wait_for:
    host: "{{ inventory_hostname }}"
    state: started
    delay: 5
    timeout: 40
  delegate_to: 127.0.0.1 # ждем на основном хосте
```

```
- name: Run once
  shell: echo ...
  run_once: true # запуск только 1 сервере первым в списке inventory
  
```

Обработка ошибок

```
any_errors_fatal: true # аварийное заверешение playbook при любой ошибке

- name: error
  apt: name=treeee
  ignore_errors: true # если будет failed то playbook продолжится

- name: check stdout
  shell: echo hello world
  register: output
  #failed_when: "'world' on output.stdout"
  failed_when: output.rc != 0
```

Хранение секретов

`ansible-vault create file_name`
`ansible-vault view file_name`
`ansible-vault edit file_name`
`ansible-vault encrypt file_name`
`ansible-vault decrypt file_name`

`ansible-playbook playbook.yml --ask-vault-pass`
`ansible-playbook playbook.yml --vault-passwoed-file password.txt`

`ansible-vault encrypt_string`:
password

`ansible-vault encrypt_string --stdin-name "name"`

Ввести пароль и будет сгенерирован пароль который начинается с `!vault`, зашифрованный пароль можно вставить в playbook

Для запуска с декриптом пароля `ansible-playbook playbook.yml --ask-vault-pass`

# Ansible: Руководство по автоматизации инфраструктуры

**Ansible** — инструмент для **автоматизации** задач по подготовке и конфигурированию инфраструктуры, включая следующие процессы:

- **Настройка серверов**
- **Развертывание приложений**
- **Управление конфигурациями**
- **Оркестрация** (координация работы нескольких машин)

## Принцип работы

- Использует **SSH** для подключения к серверам (работает **без агента**).
- Настройки описываются в формате **YAML** в файлах, называемых **playbook**.
- Применяет **декларативный подход**, где описывается желаемое состояние системы, а Ansible приводит её к этому состоянию.

## Основные компоненты

- **Inventory**: Список управляемых устройств (серверов) и их переменных.
- **Playbook**: Файл с инструкциями для применения конфигураций.
- **Task**: Отдельная задача, выполняемая на сервере.
- **Role**: Структурированная группа задач, файлов и шаблонов для повторного использования.
- **Module**: Встроенные функции Ansible для выполнения конкретных действий (например, установка пакетов, копирование файлов).

## Пример Playbook

```yaml
# Указываем целевые серверы
- hosts: webservers
  # Выполнение от имени суперпользователя (sudo)
  become: yes
  # Список задач
  tasks:
    - name: Установить nginx
      # Используем модуль apt для установки пакета
      apt:
        name: nginx
        state: present
```

## Режимы работы

- **Pull**: На управляемых серверах установлен **Agent**, который запрашивает (pull) настройки с **Master-сервера**.
- **Push**: На управляемых серверах ничего не установлено, **Master-сервер** отправляет (push) настройки через SSH.

## Аналоги

- **Chef**
- **Puppet**
- **SaltStack**

## Терминология

- **Control Server**: **Master Server** — сервер, с которого выполняется управление.
- **Controlled Servers**: **Managed Servers** — серверы, на которые применяются конфигурации.

## Настройка Inventory

Файл **hosts.txt** используется для указания серверов и их параметров. Пример плохой практики с переменными в одном файле:

```yaml
# Плохая практика
11.11.11.11  # ungrouped

[test_servers]  # Группа серверов
linux1 ansible_host=111.111.111.111 ansible_user=root ansible_ssh_port=111 ansible_ssh_private_key_file=~/.ssh/111

[test2]

[test_all:children]  # Указание группы, включающей другие группы
test_servers
test2

[test_servers:vars]  # Переменные для группы
ansible_user=root
```

### Хорошая практика

Вынос переменных в отдельные файлы для групп:

```bash
mkdir group_vars
micro group_vars/test_servers.yml
```

Пример содержимого `group_vars/test_servers.yml`:

```yaml
ansible_user: root
ansible_ssh_port: 22
```

### Отключение проверки SSH fingerprint

Для избежания ручного подтверждения fingerprint создайте файл **`ansible.cfg`**:

```ini
[defaults]
host_key_checking = false
inventory = ./hosts.txt
```

## Ad-hoc команды

Ad-hoc команды позволяют выполнять задачи без создания playbook. Примеры:

- **`ping`**: Проверяет доступность серверов.
    
    ```bash
    ansible -i hosts.txt test_servers -m ping
    ```
    
    **Описание**: Проверяет, доступны ли серверы из группы `test_servers` через SSH.
    
- **`setup`**: Собирает полную информацию о сервере (фактические данные).
    
    ```bash
    ansible -i hosts.txt test_servers -m setup
    ```
    
    **Описание**: Возвращает системные параметры, такие как ОС, память, процессоры и т.д.
    
- **`shell -a 'command'`**: Выполняет команду через оболочку.
    
    ```bash
    ansible -i hosts.txt test_servers -m shell -a 'uptime'
    ```
    
    **Описание**: Запускает команду `uptime` на серверах, поддерживает переменные окружения.
    
- **`command -a 'command'`**: Выполняет команду без оболочки.
    
    ```bash
    ansible -i hosts.txt test_servers -m command -a 'whoami'
    ```
    
    **Описание**: Выполняет команду `whoami`, но не поддерживает перенаправления (`>`) и переменные окружения.
    
- **`copy -a "src=/path/to/file dest=/path/to/file mode=777"`**: Копирует файлы на сервер.
    
    ```bash
    ansible -i hosts.txt test_servers -m copy -a "src=./index.html dest=/var/www/html/index.html mode=777"
    ```
    
    **Описание**: Копирует файл `index.html` на серверы, устанавливая права доступа.
    
- **`file -a "path=/path/to/file state=absent"`**: Управляет файлами (например, удаление).
    
    ```bash
    ansible -i hosts.txt test_servers -m file -a "path=/var/www/html/index.html state=absent"
    ```
    
    **Описание**: Удаляет файл `index.html` с серверов.
    
- **`get_url -a "url=link dest=/path/to/dir"`**: Скачивает файл по URL.
    
    ```bash
    ansible -i hosts.txt test_servers -m get_url -a "url=https://example.com/file.txt dest=/tmp/file.txt"
    ```
    
    **Описание**: Скачивает файл по указанному URL и сохраняет его в `/tmp/file.txt`.
    
- **`yum -a "name=tool_name state=installed"`**: Устанавливает пакеты через менеджер Fedora.
    
    ```bash
    ansible -i hosts.txt test_servers -m yum -a "name=neofetch state=installed"
    ```
    
    **Описание**: Устанавливает пакет `neofetch` на Fedora-based серверах.
    
- **`apt -a "name=tool_name state=installed"`**: Устанавливает пакеты через менеджер Ubuntu.
    
    ```bash
    ansible -i hosts.txt test_servers -m apt -a "name=neofetch state=installed"
    ```
    
    **Описание**: Устанавливает пакет `neofetch` на Ubuntu-based серверах.
    
- **`uri -a "url=link return_content=yes"`**: Проверяет доступность URL.
    
    ```bash
    ansible -i hosts.txt test_servers -m uri -a "url=https://example.com return_content=yes"
    ```
    
    **Описание**: Проверяет доступность URL и возвращает содержимое, если указано `return_content=yes`.
    
- **`service -a "name=service_name state=started enabled=yes"`**: Управляет сервисами через systemctl.
    
    ```bash
    ansible -i hosts.txt test_servers -m service -a "name=nginx state=started enabled=yes"
    ```
    
    **Описание**: Запускает сервис `nginx` и включает его автозапуск.
    
- **`ansible-doc -l`**: Показывает список всех доступных модулей.
    
    ```bash
    ansible-doc -l
    ```
    
    **Описание**: Выводит список всех модулей Ansible с кратким описанием.
    

### Флаги

- **`-b`**: Выполняет команду с правами суперпользователя (sudo).
- **`-v`, `-vv`, `-vvv`, `-vvvv`, `-vvvvv`**: Уровни подробности вывода (verbose).

### Просмотр Inventory

- **`ansible-inventory --list`**: Показывает содержимое inventory в формате JSON.
    
    ```bash
    ansible-inventory --list
    ```
    
- **`ansible-inventory --graph`**: Показывает структуру групп в inventory.
    
    ```bash
    ansible-inventory --graph
    ```
    

## Playbook

**Playbook** — файл в формате YAML с инструкциями. Пример базового playbook:

```yaml
- name: Test connection
  hosts: all
  become: yes

  tasks:
    - name: Ping servers
      ping:
```

### Handlers

**Handlers** — функции, вызываемые при определённых событиях. Пример:

```yaml
- name: Install nginx and Start Web Page
  hosts: all
  become: yes

  vars:
    source_file: ./site/index.html
    destin_file: /var/www/html

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Copy Page to Server
      copy:
        src: "{{ source_file }}"
        dest: "{{ destin_file }}"
      notify: Restart Nginx

    - name: Start Page and make it enable on boot
      service:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

### Условия и блоки

Условия позволяют выполнять задачи в зависимости от переменных. Пример:

```yaml
- name: Install neofetch
  hosts: all
  become: true

  tasks:
    - name: Print Linux version
      debug:
        var: ansible_os_family

    - block:
        - name: Install neofetch for RedHat
          yum:
            name: neofetch
            state: present
      when: ansible_os_family == "RedHat"

    - block:
        - name: Install neofetch for Ubuntu
          apt:
            name: neofetch
            state: present
      when: ansible_os_family == "Debian"

    - name: Run neofetch
      shell: neofetch
      register: results

    - name: Print command result
      debug:
        var: results
```

### Циклы

Циклы позволяют выполнять задачи для нескольких элементов. Пример:

```yaml
- name: Loops Playbook
  hosts: all
  become: true

  vars:
    source_folder: "./folder"
    dest_folder: "/var/site/"

  tasks:
    - name: For each
      debug:
        msg: "Hello {{ item }}"
      loop:
        - "1"
        - "2"
        - "3"
        - "4"

    - name: Loop until
      shell: echo -n A >> file.txt && cat file.txt
      register: output
      delay: 2
      retries: 10
      until: output.stdout.find("AAAA") == false

    - name: Copy all files
      copy:
        src: "{{ item }}"
        dest: "{{ dest_folder }}"
        mode: 0555
      with_fileglob: "{{ source_folder }}/*.*"
```

### Шаблоны Jinja

**Jinja2** используется для динамической генерации файлов. Пример:

```yaml
- name: Copy template
  template:
    src: "{{ source_folder }}/index.html.j2"
    dest: "{{ dest_folder }}/index.html"
```

**Пример шаблона `index.html.j2`**:

```html
<html>
  <body>
    <h1>Welcome to {{ ansible_hostname }}</h1>
  </body>
</html>
```

### Роли

**Роли** структурируют задачи для повторного использования. Создание роли:

```bash
ansible-galaxy init role_name
```

**Структура роли**:

- `defaults`: Переменные по умолчанию.
- `files`: Файлы для копирования.
- `handlers`: Обработчики событий.
- `meta`: Мета-информация о роли.
- `tasks`: Основные задачи.
- `templates`: Шаблоны Jinja2.
- `tests`: Тесты для роли.
- `vars`: Дополнительные переменные.

Пример вызова роли в playbook:

```yaml
- hosts: all
  roles:
    - deploy_test
    - deploy_db
    - role_name
    - { role: super_admin, when: ansible_system == "Linux" }
```

### Extra-vars

Переменные с наивысшим приоритетом, передаваемые через командную строку:

```bash
ansible-playbook playbook.yml -e "MYHOSTS=STAGING"
```

Пример playbook с использованием extra-vars:

```yaml
- hosts: "{{ MYHOSTS }}"
  tasks:
    - name: Print hosts
      debug:
        msg: "Running on {{ inventory_hostname }}"
```

### Include и Import

- **`include`**: Подключает внешний файл задач во время выполнения playbook.
- **`import`**: Вставляет содержимое файла в playbook на этапе парсинга.

Пример:

```yaml
tasks:
  - name: Create folder
    include: create_folders.yml

  - name: Create files
    include: create_files.yml mytext="123"
```

### Делегирование задач

**`delegate_to`**: Выполняет задачу на указанном сервере. Пример:

```yaml
- name: Only on 1 server
  copy:
    src: ./file.txt
    dest: /tmp/file.txt
  delegate_to: server_name

- name: Send to another server
  shell: echo Hello from {{ inventory_hostname }} >> /var/log.txt
  delegate_to: 127.0.0.1
```

### Асинхронное выполнение

Для задач, не требующих ожидания (например, перезагрузка):

```yaml
- name: Reload servers
  shell: reboot now
  async: 1
  poll: 0

- name: Wait reload
  wait_for:
    host: "{{ inventory_hostname }}"
    state: started
    delay: 5
    timeout: 40
  delegate_to: 127.0.0.1
```

### Выполнение один раз

**`run_once`**: Выполняет задачу только на одном сервере из inventory.

```yaml
- name: Run once
  shell: echo "Task executed" >> /var/log.txt
  run_once: true
```

### Обработка ошибок

- **`any_errors_fatal: true`**: Останавливает выполнение playbook при любой ошибке.
- **`ignore_errors: true`**: Игнорирует ошибку и продолжает выполнение.
- **`failed_when`**: Указывает условие, при котором задача считается неудачной.

Пример:

```yaml
- name: Error handling
  apt:
    name: treeee
  ignore_errors: true

- name: Check stdout
  shell: echo hello world
  register: output
  failed_when: output.rc != 0
```

### Хранение секретов

**Ansible Vault** используется для шифрования чувствительных данных:

- **`ansible-vault create file_name`**: Создаёт зашифрованный файл.
    
    ```bash
    ansible-vault create secrets.yml
    ```
    
- **`ansible-vault view file_name`**: Просматривает зашифрованный файл.
    
    ```bash
    ansible-vault view secrets.yml
    ```
    
- **`ansible-vault edit file_name`**: Редактирует зашифрованный файл.
    
    ```bash
    ansible-vault edit secrets.yml
    ```
    
- **`ansible-vault encrypt file_name`**: Шифрует существующий файл.
    
    ```bash
    ansible-vault encrypt secrets.yml
    ```
    
- **`ansible-vault decrypt file_name`**: Расшифровывает файл.
    
    ```bash
    ansible-vault decrypt secrets.yml
    ```
    
- **`ansible-vault encrypt_string`**: Шифрует строку.
    
    ```bash
    ansible-vault encrypt_string 'my_password' --name 'my_secret'
    ```
    
    **Вывод**:
    
    ```yaml
    my_secret: !vault |
              $ANSIBLE_VAULT;1.1;AES256
              663861663365663...
    ```
    

Запуск playbook с Vault:

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

Или с файлом пароля:

```bash
ansible-playbook playbook.yml --vault-password-file password.txt
```