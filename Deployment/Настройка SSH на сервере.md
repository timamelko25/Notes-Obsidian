
port knocking


1. сформировать ключ ssh
	1. `ssh-keygen -t ed25519`
2. добавить ключ public на сервер (для рута)
	1. в панели хостинга
	2. ```ssh-copy-id -i ~/.ssh/mykey user@host```
3. добавить нового пользователя
	1. `adduser www`
4. добавить пользователя в группу `sudo`
	1. `adduser www sudo`
5. изменить файл `/etc/ssh/sshd_config`
	1. `PasswordAuthentification` - `yes` (temporary)
	2. `sudo systemctl restsrt ssh`
6. добавить ключ public на сервер (новый пользователь `www`)
	1. ```ssh-copy-id -i ~/.ssh/mykey user@host```
7. изменить файл `/etc/ssh/sshd_config`
	1. `PermitRoorLogin` - `no`
	2. Add line `Allowusers` - `www` (username)
	3. `Port` - `45789`
	4. `sudo systemctl restsrt ssh`

port knocking
Технология _Port Knocking_ осуществляет последовательность попыток подключения к закрытым портам.
Сервер, чаще всего, никак не отвечает на эти подключения, но он считывает и обрабатывает их. Но если же серия подключений была заранее обозначена пользователем, то выполнится определенное действие. Как пример, подключение к SSH-сервису на порту 22.


fail2ban

```
ssh-copy-id [-f] [-n] [-i identity file] [-p port] [-o ssh_option] [user@]hostname
```

**-f** Don't check if the key is already configured as an authorized key on the server. Just add it. This can result in multiple copies of the key in `authorized_keys` files.

**-i** Specifies the identity file that is to be copied (default is `~/.ssh/id_rsa`). If this option is not provided, this adds all keys listed by `ssh-add -L`. Note: it can be multiple keys and **adding extra authorized keys can easily happen accidentally**! If `ssh-add -L` returns no keys, then the most recently modified key matching `~/.ssh/id*.pub`, excluding those matching `~/.ssh/*-cert.pub`, will be used.

**-n** Just print the key(s) that would be installed, without actually installing them.

**-o ssh_option** Pass `-o ssh_option` to the SSH client when making the connection. This can be used for overriding configuration settings for the client. See [ssh command line options](https://www.ssh.com/ssh/command) and the possible configuration options in [ssh_config](https://www.ssh.com/ssh/config).

**-p port** Connect to the specifed SSH port on the server, instead of the default port 22.

**-h** or **-?** Print usage summary.

SSH Config
`~/.ssh/config`

```
Host my_server
	Hostname fd12::8
	Port 8022
	User www
	IdentityFile ~/.ssh/id_ed25519
```

https://serverspace.ru/support/help/ssh-config-dlya-servera-bystro-i-bezopasno/?utm_source=google.com&utm_medium=organic&utm_campaign=google.com&utm_referrer=google.com